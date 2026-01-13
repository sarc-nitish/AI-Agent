# AI Agent 

## Project Title:
AI Agent Development using LlamaIndex and Groq

## Overview:
This project demonstrates the creation of an AI agent using LlamaIndex and Groq's language model to perform arithmetic operations through natural language queries. The agent leverages the ReAct (Reasoning + Acting) framework to reason about user inputs and execute appropriate tools, showcasing a practical application of AI agent development.

## Objectives:
- Develop an AI agent capable of processing natural language arithmetic queries.
- Enable the agent to perform basic arithmetic operations (addition, subtraction, multiplication, division) using custom tools.
- Utilize LlamaIndex's ReAct framework to structure reasoning and tool execution effectively.

## Technologies & Libraries Used:
- Python
- Jupyter Notebook
- LlamaIndex (`llama-index`)
- llama-index-llms-groq
- os (for environment variable management)

## Step-by-Step Workflow:

### 1. Install Dependencies
The notebook begins by installing the required libraries:
```bash
!pip install llama-index llama-index-llms-groq
```
- **Purpose**: Installs `llama-index` for agent development and `llama-index-llms-groq` for integration with Groq's language model.

### 2. Import Necessary Modules
Key modules for agent creation and tool definition are imported:
```python
import os
from llama_index.core.agent import ReActAgent
from llama_index.llms.groq import Groq
from llama_index.core.tools import FunctionTool
```
- **Purpose**: Sets up the environment for building the ReAct agent and defining tools.

### 3. Set Groq API Key
The Groq API key is securely set using:
```python
os.environ["GROQ_API_KEY"] = "_____________"
```
- **Purpose**: Authenticates access to Groq's language model.

### 4. Initialize Groq Language Model
The Groq language model is configured:
```python
llm = Groq(
    model="llama-3.1-8b-instant",
    temperature=0
)
```
- **Purpose**: Creates a deterministic language model instance for the agent's reasoning.

### 5. Define Arithmetic Tools
Four arithmetic functions are defined and converted into tools:
```python
def add(a: float, b: float) -> float:
    """Add two numbers and return the sum."""
    return a + b

def subtract(a: float, b: float) -> float:
    """Subtract the second number from the first number and return the result"""
    return a - b

def multiply(a: float, b: float) -> float:
    """Multiply twi numbers abd return teh product"""
    return a * b

def devide(a: float, b: float) -> float:
    """Divide the first number by the second number and return teh quotient"""
    return a / b

add_tool = FunctionTool.from_defaults(fn=add)
subtract_tool = FunctionTool.from_defaults(fn=subtract)
multiply_tool = FunctionTool.from_defaults(fn=multiply)
devide_tool = FunctionTool.from_defaults(fn=devide)

tools_list = [add_tool, subtract_tool, multiply_tool, devide_tool]
```
- **Purpose**: Provides the agent with tools to perform arithmetic operations. Note: The `devide` function has typos ("devide" instead of "divide", "teh" instead of "the").

### 6. Create ReAct Agent
The ReAct agent is initialized with the language model and tools:
```python
agent = ReActAgent.from_tools(
    tools_list,
    llm=llm,
    verbose=True
)
```
- **Purpose**: Combines the Groq LLM and arithmetic tools to create an agent capable of reasoning and acting.

### 7. Test the Agent with Queries
The agent is tested with arithmetic queries:
```python
response = agent.chat("What is 20 divided by 2")
response = agent.chat("What is 20 plus 2")
response = agent.chat("What is 20+(2*4)? Use tool to calculate evry step.")
```
- **Purpose**: Validates the agent's ability to process natural language queries and execute tools correctly.

## Sample Outputs:
Below are the outputs from the notebook, showcasing the agent's responses:

1. **Query: "What is 20 divided by 2"**
   - **Agent's Reasoning**:
     ```
     > Running step a77b8def-ed93-4b65-9376-6df52d4631a6. Step input: What is 20 divided by 2
     Thought: The current language of the user is: English. I need to use a tool to help me answer the question.
     Action: devide
     Action Input: {'a': 20, 'b': 2}
     Observation: 10.0
     > Running step 852d49d3-beda-431b-b453-4194d856989c. Step input: None
     Thought: I can answer without using any more tools. I'll use the user's language to answer
     Answer: The result of 20 divided by 2 is 10.
     ```
   - **Final Response**:
     ```
     The result of 20 divided by 2 is 10.
     ```
   - **Raw Tool Output**:
     ```
     10.0
     ```

2. **Query: "What is 20 plus 2"**
   - **Agent's Reasoning**:
     ```
     > Running step ... Step input: What is 20 plus 2
     Thought: The current language of the user is: English. I need to use a tool to help me answer the question.
     Action: add
     Action Input: {'a': 20, 'b': 2}
     Observation: 22
     > Running step ... Step input: None
     Thought: I can answer without using any more tools. I'll use the user's language to answer
     Answer: The result of 20 plus 2 is 22.
     ```
   - **Final Response**:
     ```
     The result of 20 plus 2 is 22.
     ```

3. **Query: "What is 20+(2*4)? Use tool to calculate evry step."**
   - **Agent's Reasoning**:
     ```
     > Running step e70c5d80-80b9-4213-9117-21dab85a4d8b. Step input: What is 20+(2*4)? Use tool to calculate evry step.
     Thought: I need to use a tool to help me calculate the expression 20+(2*4).
     Action: multiply
     Action Input: {'a': 2, 'b': 4}
     Observation: 8
     > Running step cfba76eb-b9c2-455f-96a6-6e16a09158a2. Step input: None
     Thought: Now I need to use the result of the multiplication to calculate the final expression 20+(2*4).
     Action: add
     Action Input: {'a': 20, 'b': 8}
     Observation: 28
     > Running step 41fe3198-64ca-4ca3-8f5d-eb4d84533ef4. Step input: None
     Thought: I can answer without using any more tools. I'll use the user's language to answer
     Answer: The result of 20+(2*4) is 28.
     ```
   - **Final Response**:
     ```
     The result of 20+(2*4) is 28.
     ```

## Use Case:
This AI agent can be applied in:
- **Arithmetic Automation**: Automates calculations for quick and accurate results in applications requiring numerical processing.
- **Educational Tools**: Assists students in learning arithmetic by providing step-by-step solutions and explanations.
- **Customer Support**: Answers numerical queries in domains like finance or e-commerce (e.g., calculating totals or discounts).
- **Prototyping AI Agents**: Serves as a proof-of-concept for building more complex ReAct-based agents.

## Future Scope:
- **Advanced Operations**: Add tools for complex mathematical functions (e.g., exponents, logarithms) or integrate libraries like SymPy.
- **Multi-Domain Capabilities**: Extend the agent to handle text processing, data retrieval, or API interactions.
- **Improved Query Handling**: Enhance natural language understanding for ambiguous or multi-step queries.
- **Real-World Applications**:
  - **Finance**: Calculate loan interest, budgets, or investment returns.
  - **E-Commerce**: Compute pricing with taxes, discounts, or shipping.
  - **Education**: Build interactive math tutors.
- **Deployment**: Create a web-based interface using Streamlit or Gradio, or deploy as an API with FastAPI.
- **Robustness**: Implement error handling (e.g., division by zero) and tool selection validation.

## Conclusion:
This project showcases a foundational AI agent built with LlamaIndex and Groq, capable of processing arithmetic queries using the ReAct framework. It demonstrates the integration of external LLMs and custom tools, forming a base for advanced applications in education, finance, e-commerce, and beyond. The notebook highlights practical AI agent development skills and opens avenues for further enhancements.
