#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
使用Amazon Bedrock Invoke Model API调用Claude Sonnet 3模型的示例脚本
"""

import boto3
import json
import os
from datetime import datetime
from botocore.exceptions import ClientError
import logging

# 配置日志
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class ClaudeInvokeClient:
    """Claude Sonnet 3 调用客户端类"""
    
    def __init__(self, 
                 model_id="us.anthropic.claude-3-7-sonnet-20250219-v1:0",
                 region="us-east-1",
                 access_key=None,
                 secret_key=None,
                 max_tokens=2048,
                 temperature=1):
        """
        初始化Claude客户端
        
        参数:
            model_id (str): 模型ID
            region (str): AWS区域
            access_key (str): AWS访问密钥
            secret_key (str): AWS秘密访问密钥
            max_tokens (int): 生成的最大token数量
            temperature (float): 采样温度，控制输出的随机性
        """
        self.model_id = model_id
        self.max_tokens = max_tokens
        self.temperature = temperature
        
        # 创建Bedrock客户端
        kwargs = {
            'service_name': 'bedrock-runtime',
            'region_name': region
        }
        
        if access_key and secret_key:
            kwargs['aws_access_key_id'] = access_key
            kwargs['aws_secret_access_key'] = secret_key
            
        try:
            self.client = boto3.client(**kwargs)
        except Exception as e:
            logger.error(f"创建Bedrock客户端时出错: {str(e)}")
            raise

    def invoke_model(self, prompt, system=None):
        """
        调用Claude 3模型进行推理
        
        参数:
            prompt (str): 发送给模型的提示文本
            system (str, optional): 系统提示信息
            
        返回:
            dict: 模型的响应
        """
        try:
            # 构建请求体
            request_body = {
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": self.max_tokens,
                "temperature": self.temperature,
                "thinking":{
                    "type": "enabled",
                    "budget_tokens": 1024
                },
                "messages": [
                    {
                        "role": "user",
                        "content": [
                            {
                                "type": "text",
                                "text": prompt
                            }
                        ]
                    }
                ]
            }
            
            # 如果提供了system提示，添加到请求中
            if system:
                request_body["system"] = system
            
            # 调用模型
            response = self.client.invoke_model(
                modelId=self.model_id,
                body=json.dumps(request_body)
            )
            
            # 解析响应
            result = json.loads(response.get("body").read())
            
            # 打印调用详情
            if "usage" in result:
                input_tokens = result["usage"]["input_tokens"]
                output_tokens = result["usage"]["output_tokens"]
                logger.info(f"输入长度: {input_tokens} tokens")
                logger.info(f"输出长度: {output_tokens} tokens")
            
            return result
            
        except ClientError as err:
            logger.error(
                "调用Claude 3 Sonnet失败。原因: %s: %s",
                err.response["Error"]["Code"],
                err.response["Error"]["Message"]
            )
            raise
        except Exception as e:
            logger.error(f"发生错误: {str(e)}")
            raise

def save_conversation(prompt, response, filename=None):
    """保存对话到文件"""
    if filename is None:
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        filename = f"claude_conversation_{timestamp}.json"
    
    conversation_data = {
        "timestamp": datetime.now().isoformat(),
        "prompt": prompt,
        "response": response
    }
    
    with open(filename, 'w', encoding='utf-8') as f:
        json.dump(conversation_data, f, ensure_ascii=False, indent=2)
    
    logger.info(f"对话已保存到: {filename}")

def main():
    """主函数"""
    # 创建Claude客户端
    claude = ClaudeInvokeClient()
    
    print("欢迎使用Claude Sonnet 3 Bedrock API示例 (Invoke版本)")
    print("输入'exit'或'quit'退出程序")
    
    while True:
        # 获取用户输入
        user_input = input("\n请输入您的问题: ")
        
        # 检查是否退出
        if user_input.lower() in ['exit', 'quit']:
            print("感谢使用，再见！")
            break
        
        try:
            # 调用Claude模型
            print("正在处理您的请求...")
            response = claude.invoke_model(user_input)
            print(response)
            
            # 提取并打印模型回复
            if 'content' in response:
                for content in response['content']:
                    if content.get('type') == 'thinking':
                        print("-"*100)
                        print("\nThinking:")
                        print(content['thinking'])
                        print("-"*100)
                    if content.get('type') == 'text':
                        print("\nClaude回复:")
                        print(content['text'])
                
                # 可选：保存对话
                # save_conversation(user_input, response)
            else:
                print("无法解析模型响应:", response)
                
        except Exception as e:
            print(f"发生错误: {str(e)}")

if __name__ == "__main__":
    main() 
