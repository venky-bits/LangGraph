# LangGraph

Project to learn creating an agentic framework using LangGraph

## Notebooks

### 1_simple_graph.ipynb

A simple introduction to LangGraph demonstrating a multi-step workflow using a state graph.

**What it does:**
- Defines a `PortfolioState` TypedDict with three fields: `amount_usd`, `total_usd`, and `total_inr`
- Creates two processing nodes:
  1. `calc_total_usd`: Calculates total USD by applying an 8% factor to the input amount
  2. `convert_to_inr`: Converts the total USD to Indian Rupees using an exchange rate of 85.0
- Builds a linear workflow: START → calc_total_usd → convert_to_inr → END
- Visualizes the graph as a Mermaid diagram
- Invokes the graph with a sample input of $1000 USD

**How to execute:**

1. Ensure dependencies are installed:
   ```bash
   pip install -r requirements.txt
   ```

2. Open the notebook in Jupyter:
   ```bash
   jupyter notebook 1_simple_graph.ipynb
   ```

3. Run all cells in sequence:
   - Cell 1: Import TypedDict
   - Cell 2: Define PortfolioState
   - Cell 3: Define calc_total function
   - Cell 4: Define convert_to_inr function
   - Cell 5: Build and compile the state graph
   - Cell 6: Display the graph visualization
   - Cell 7: Execute the graph with sample input

**Expected Output:**
The notebook will:
- Display a visual representation of the graph showing the workflow
- Return a dictionary with calculated values:
  ```python
  {
    'amount_usd': 1000,
    'total_usd': 1080.0,
    'total_inr': 91800.0
  }
  ```


### 2_conditional_graph.ipynb

Demonstrates conditional branching in LangGraph workflows using a state graph that makes decisions based on input parameters.

**What it does:**
- Defines a `PortfolioState` TypedDict with four fields: `amount_usd`, `total_usd`, `target_currency`, and `total`
- Creates four nodes:
  1. `calc_total_usd`: Calculates total USD by applying an 8% factor to the input amount
  2. `convert_to_inr`: Converts USD to Indian Rupees (1 USD = 85 INR)
  3. `convert_to_eur`: Converts USD to Euro (1 USD = 0.9 EUR)
  4. `choose_conversion`: A routing function that determines which conversion path to take
- Builds a conditional workflow with branching logic:
  - START → calc_total_usd → conditional_edges → (convert_to_inr or convert_to_eur) → END
  - The `choose_conversion()` function examines the `target_currency` field and routes to the appropriate conversion node
- Visualizes the graph with conditional branching as a Mermaid diagram
- Executes the graph with two different scenarios: conversion to INR and EUR

**How to execute:**

1. Ensure dependencies are installed:
   ```bash
   pip install -r requirements.txt
   ```

2. Open the notebook in Jupyter:
   ```bash
   jupyter notebook 2_conditional_graph.ipynb
   ```

3. Run all cells in sequence to see:
   - State structure definition
   - Processing function definitions
   - Graph construction with conditional edges
   - Graph visualization showing branching paths
   - Execution with INR target currency
   - Execution with EUR target currency

**Expected Output:**
The notebook will:
- Display a Mermaid diagram showing the conditional branching structure
- For INR conversion:
  ```python
  {
    'amount_usd': 1000,
    'total_usd': 1080.0,
    'target_currency': 'INR',
    'total': 91800.0
  }
  ```
- For EUR conversion:
  ```python
  {
    'amount_usd': 1000,
    'total_usd': 1080.0,
    'target_currency': 'EUR',
    'total': 972.0
  }
  ```

**Key Concepts:**
- **Conditional Edges**: Routes the workflow to different nodes based on the output of a routing function
- **State Management**: Shows how state flows through different paths in the graph
- **Multi-Path Workflows**: Demonstrates how a single graph can handle different processing paths based on input


### 3_chatbot.ipynb

Demonstrates building an interactive conversational chatbot using LangGraph with message history and context awareness powered by Google's Gemini LLM.

**What it does:**
- Loads environment variables to securely access the Google API key
- Initializes a Google Gemini Flash Lite language model using LangChain's `init_chat_model`
- Defines a `State` TypedDict with a `messages` field that uses the `add_messages` reducer to accumulate conversation history
- Creates a `chatbot()` function that:
  - Takes the current conversation state
  - Invokes the LLM with the complete message history
  - Returns the AI's response
- Builds a simple linear graph: START → chatbot → END
- Tests the chatbot with a single question
- Runs an interactive conversation loop that:
  - Maintains conversation context across multiple turns
  - Allows the LLM to reference previous messages
  - Continues until the user types 'exit' or 'quit'

**How to execute:**

1. **Create a `.env` file** in the LangGraph directory with your Google API key:
   ```
   GOOGLE_API_KEY=<Your API Key>
   ```
   **Note:** The `.env` file is excluded from version control via `.gitignore` to keep your API key secure.

2. Ensure dependencies are installed:
   ```bash
   pip install -r requirements.txt
   ```

3. Open the notebook in Jupyter:
   ```bash
   jupyter notebook 3_chatbot.ipynb
   ```

4. Run all cells in sequence to see:
   - Environment setup
   - Library imports
   - LLM initialization and testing
   - State definition and chatbot function
   - Graph construction
   - Single message test
   - Interactive chat loop

**Expected Output:**
The notebook will:
- Display a simple test response from the LLM
- Show a single Q&A exchange (e.g., "Who was the first person to walk on the moon?")
- Run an interactive chat session where you can:
  - Ask follow-up questions that reference previous context
  - Have multi-turn conversations
  - See the AI maintain awareness of the conversation history

**Key Concepts:**
- **Message History**: Uses `add_messages` reducer to accumulate conversation turns
- **Stateful Conversations**: The graph maintains state across multiple invocations
- **LLM Integration**: Demonstrates connecting LangGraph with external language models
- **Interactive Workflows**: Shows how to build user-interactive applications with LangGraph
- **Secure API Key Management**: Uses environment variables to protect sensitive credentials
