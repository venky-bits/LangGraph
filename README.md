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

