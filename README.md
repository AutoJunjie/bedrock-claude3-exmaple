# bedrock-claude3-exmaple


## Installation:
The project is still in rapid itelating, suggest to build it youself following the following steps:

1. configure your AK/SK by running  `aws configure`
2. git clone current project: `git clone https://github.com/AutoJunjie/bedrock-claude3-exmaple.git`
3. install requirements:
    - `cd bedrock-runtime`
    - `pip install -r requirements`
4. run the project to test:
    - `cd bedrock-runtime/models/anthropic`
    - `python3 claude_3.py`

```anthropic % python3 claude_3.py
----------------------------------------------------------------------------------------
Welcome to the Amazon Bedrock Runtime demo with Anthropic Claude 3.
----------------------------------------------------------------------------------------
Invoking Claude 3 Sonnet with 'Hi, write a short sentence about yourself'...
Invocation details:
- The input length is 15 tokens.
- The output length is 16 tokens.
- The model returned 1 response(s):
I am Claude, an AI assistant created by Anthropic.
----------------------------------------------------------------------------------------
Invoking Claude 3 Sonnet with 'Tell me a short story about this image.' and ../../resources/images/robot.png ...

Invocation details:
- The input length is 409 tokens.
- The output length is 194 tokens.
- The model returned 1 response(s):
In a whimsical world where everyday objects come to life, a dapper little robot stood amid the lush greenery, donning a stylish top hat and goggles. Its metallic body was an eclectic assemblage of gears, knobs, and gadgets, each component contributing to its unique charm.

As the warm sunlight filtered through the foliage, casting a soft glow on the robot's polished surface, it seemed to radiate a sense of curiosity and wonder. Perhaps it was on a quest to unravel the mysteries of nature, or maybe it simply enjoyed basking in the tranquility of its verdant surroundings.

Regardless of its purpose, the robot's presence brought a touch of playful imagination to the scene, a reminder that even in the most ordinary of settings, a dash of whimsy can transform the world into a magical place.
----------------------------------------------------------------------------------------
