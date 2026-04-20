# GraphSAT: Neural SAT Solving with GNN and GAT

This project explores the use of Continuous Optimization and Graph Neural Networks (GNN) to solve the Boolean Satisfiability Problem (SAT) and Maximum Satisfiability Problem (MaxSAT).

This study implements and compares classical SAT solver algorithms alongside several neural SAT solvers built with PyTorch. It also includes tools for automating the download of benchmark datasets from the [MaxSAT Evaluation](https://maxsat-evaluations.github.io/).

For comprehensive research methodology, experimental details, and conclusions, please refer to the [`paper.pdf`](./paper.pdf) (or [`paper(zh).pdf`](<./paper(zh).pdf>)) included in this repository.

## Features

1. **MaxSAT Dataset Preparation**
   - Built-in `DatasetPreparer` class that automatically downloads and parses `.wcnf.xz` files from the 2017–2024 MaxSAT Evaluation Benchmarks.
   - Converts files into Soft & Hard Clauses suitable for training or classical solvers.
2. **Hard Instances Generation**
   - Utilizes `pysat.examples.genhard` to generate classic hard SAT problems, such as the Pigeonhole Principle (PHP) and Parity (PAR), for stress testing.
3. **Classical Baselines**
   - Integrates the LSU (Linear SU) MaxSAT solver from `pysat` as an optimal baseline for comparison.
4. **Neural SAT Solvers**
   - **TorchmSAT**: A SAT solver implementation based on continuous relaxations and gradient descent.
   - **GraphSAT (GNN)**: Converts SAT formulas into bipartite graphs and solves them using message passing between variables and clauses.
   - **GraphAttentionSAT (GAT)**: Introduces a masked attention mechanism during message passing, allowing the model to automatically learn the weight relations of different clauses on variable assignments.

## Experiments & Evaluation

The complete experimental pipeline is documented in `graph_sat.ipynb`:

1. **Model Evaluation**: Concurrently runs `TorchmSAT`, `GraphSAT`, and `GraphAttentionSAT` on loaded MaxSAT datasets or generated Hard Instances.
2. **Performance Comparison**: Evaluates the number of unsatisfied clauses using `MSELoss` and a custom `compute_cost` function.
3. **Visualizations**: Uses `matplotlib` to plot bar charts comparing the **Final Cost (Unsatisfied Clauses)** and **Total Time (Execution Time)** across the three models, illustrating the trade-off between model performance and convergence time.

## Dependencies

Ensure the following Python packages are installed in your environment:

- `torch` (GPU acceleration supported)
- `python-sat` (PySAT)
- `numpy`
- `matplotlib`

If you are running the project using a Jupyter Notebook, `!pip install` commands are included to automatically install missing dependencies.

## Project Structure

```text
📂 graph-sat
├── 📄 graph_sat.ipynb    # Core experimental code (data processing, SAT models, and plot generation)
├── 📄 lab.ipynb          # Scratchpad for experiments and testing
├── 📄 LICENSE            # Project license terms
├── 📄 paper.pdf          # Research paper (English)
└── 📄 paper(zh).pdf      # Research paper (Chinese)
```
