NAME: SWATHI S

REG NO:212223040219
## AIM:

To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).
Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling
## THEORY:
The system combines:

Mathematical Component: Uses the formula V = πr²h for cylinder volume calculation
AI Component: Utilizes OpenAI's gpt-4-0613 model
Integration Layer: Connects the AI understanding with mathematical computation
## ALGORITHM:
1.Initialize the LLM System:

Configure the OpenAI API.
Define the function schema for calculate_cylinder_volume, including its name, description, and parameter requirements.

2.User Input:

  Receive a natural language query from the user.
3.Call LLM for Chat Completion:

  Pass the user query to the LLM along with the function schema.Set function_call="auto" to allow the LLM to decide if the function should be invoked.
   Check LLM Response:

4.If the LLM suggests a function call:
   Extract the function name and arguments from the response.
5.Else:
   Generate a direct response from the LLM (no function required).
6.Execute Function:

 Parse the arguments from the LLM's suggested function call.Call the calculate_cylinder_volume(radius, height) function.
7.Compute the volume using the formula:
  volume = math.pi * radius**2 * height
8.Return Result to LLM:

  Create a message in the format of a function's response Pass this message to the LLM for it to generate a final natural language response.
9.Generate Final Response:

 The LLM integrates the calculated result into a user-friendly message.
10.Output the Response:

  Display the final response to the user.

## CODE:
```
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
```
## OUTPUT:
![image](https://github.com/user-attachments/assets/cf031338-c1e8-48f4-b851-0187c13a08a2)


## RESULT:
Thus, design and implement a Python function for calculating the volume of a cylinder, using large language model (LLM) is executed succesfully.

