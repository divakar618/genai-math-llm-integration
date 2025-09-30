## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating simple mathematical operations of two numbers, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1:
Import required libraries (os, openai, json) and load the OpenAI API key from the environment

#### STEP 2:
Define the calculation function

#### STEP 3:
Create a function schema (functions) that tells GPT which functions it can call.
Function name: 

#### STEP 4:
Define the user message. Ex: 13+15

#### STEP 5:
Call the OpenAI ChatCompletion API

Extract the function call arguments from the model’s response, pass them to perform_calculation, and get the result.

### PROGRAM:
```

import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']


import json

def perform_calculation(operation, a, b):
    """Perform a basic calculation with two numbers"""
    if operation == "add":
        result = a + b
    elif operation == "subtract":
        result = a - b
    elif operation == "multiply":
        result = a * b
    elif operation == "divide":
        result = a / b if b != 0 else "Error: Division by zero"
    else:
        result = "Invalid operation"
    
    return json.dumps({
        "operation": operation,
        "a": a,
        "b": b,
        "result": result
    })


# define a function
functions = [
    {
        "name": "perform_calculation",
        "description": "Perform a basic arithmetic operation on two numbers",
        "parameters": {
            "type": "object",
            "properties": {
                "operation": {
                    "type": "string",
                    "enum": ["add", "subtract", "multiply", "divide"],
                    "description": "The arithmetic operation to perform"
                },
                "a": {"type": "number", "description": "First number"},
                "b": {"type": "number", "description": "Second number"},
            },
            "required": ["operation", "a", "b"],
        },
    }
]



messages = [
    {
        "role": "user",
        "content": "Calculate 13+23"
    }
]


import openai


# Call the ChatCompletion endpoint
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)


print(response)


response_message = response["choices"][0]["message"]

response_message

response_message["content"]

response_message["function_call"]

json.loads(response_message["function_call"]["arguments"])


```

### OUTPUT:

<img width="881" height="90" alt="Screenshot 2025-09-30 092606" src="https://github.com/user-attachments/assets/5d9e1516-cdea-4ce6-bfce-3abd284264aa" />



### RESULT:
Thus, the program to design and implement a Python function for calculating simple mathematical operations of two numbers is implmented.
