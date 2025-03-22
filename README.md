NAME: SWATHI S

REG NO:212223040219
## AIM:

To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

## CODE:
import os
import openai
import json
from dotenv import load_dotenv, find_dotenv

# Load environment variables
_ = load_dotenv(find_dotenv())
openai.api_key = os.getenv("OPENAI_API_KEY")  # Ensure API key is masked

# Define function to calculate cylinder volume
def calculate_cylinder_volume(radius, height):
    """Calculates the volume of a cylinder."""
    volume = 3.14159265359 * (radius ** 2) * height
    return json.dumps({"radius": radius, "height": height, "volume": volume})

# Define OpenAI function schema
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder given its radius and height.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {"type": "number", "description": "The radius of the cylinder in meters."},
                "height": {"type": "number", "description": "The height of the cylinder in meters."}
            },
            "required": ["radius", "height"]
        }
    }
]

# User message (example)
messages = [{"role": "user", "content": "What is the volume of a cylinder with radius 5 and height 10?"}]

# OpenAI API call with function calling
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)

# Extract function call if present
response_message = response["choices"][0]["message"]
if "function_call" in response_message:
    args = json.loads(response_message["function_call"]["arguments"])
    result = calculate_cylinder_volume(**args)
    
    # Append function result to messages
    messages.append({
        "role": "function",
        "name": "calculate_cylinder_volume",
        "content": result,
    })
    
    # Get final response
    final_response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
    )
    print(final_response["choices"][0]["message"]["content"])
## RESULT:
Thus, design and implement a Python function for calculating the volume of a cylinder, using large language model (LLM) is executed succesfully.

