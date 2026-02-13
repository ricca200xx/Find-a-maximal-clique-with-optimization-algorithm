# Solving the Maximum Clique Problem via Continuous Optimization

This project, developed for the **Optimization for Data Science** course at the University of Padua, addresses the **Maximum Clique Problem (MCP)** using first-order optimization algorithms.

The MCP is a well-known NP-hard problem that involves finding the largest complete subgraph (a clique) within a given undirected graph. To overcome the combinatorial nature of the problem, it has been reformulated as a continuous optimization problem.

---

## Theoretical Approach and Formulation

The approach is based on the Motzkin-Straus formulation, which links the clique problem to a quadratic program. To ensure that the local maxima of the objective function correspond to actual maximal cliques, a regularization term has been added.

The goal is to solve the following optimization problem:

$$
\text{maximize} \quad f(x) := x^T A x + \Phi(x) \quad \text{subject to} \quad x \in \Delta
$$

where:
-   **$A$** is the adjacency matrix of the graph.
-   **$x$** is the vector of optimization variables.
-   **$\Delta$** is the standard simplex: $\Delta := \{x \in \mathbb{R}^n \mid \sum x_i = 1, x_i \geq 0\}$.
-   **$\Phi(x)$** is the regularization function.

Two different regularization functions have been implemented and compared:
1.  **$L_2$ Regularization**: $\Phi_{l2}(x) = \frac{\alpha}{2} \|x\|_2^2$
2.  **$L_0$ Approximation**: $\Phi_{l0}(x) = \alpha \sum_{i=1}^n (\exp(-\beta x_i) - 1)$

---

## Project Structure

```
.
├── .DS_Store
├── .gitattributes
├── .gitignore
├── README.md
├── final_project.ipynb
├── requirements.txt
└── results.csv
```

-   **`final_project.ipynb`**: The Jupyter Notebook containing the full code implementation, experiments, and results visualization.
-   **`requirements.txt`**: The Python dependencies required to run the project.
-   **`results.csv`**: The raw data obtained from the experiments.

---

## Prerequisites and Installation

Ensure you have **Python 3.x** installed.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/ricca200xx/Find-a-maximal-clique-with-optimization-algorithm.git
    cd Find-a-maximal-clique-with-optimization-algorithm
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```bash
    python -m venv venv
    # On macOS/Linux:
    source venv/bin/activate
    # On Windows:
    .\venv\Scripts\activate
    ```

3.  **Install the dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

---

##  Usage

The entire project can be executed through the Jupyter Notebook **`final_project.ipynb`**.

1.  Start Jupyter Lab or Jupyter Notebook from the project's root directory:
    ```bash
    jupyter lab
    ```
2.  Open the `final_project.ipynb` file.
3.  Run the cells in order to reproduce the entire workflow:
    * Loading libraries and utility functions.
    * Defining the algorithms and step-size strategies.
    * Running experiments on benchmark graphs.
    * Generating analysis tables and plots.

---

## Implemented Algorithms

Four first-order optimization algorithms were implemented, each tested with three different step-size strategies.

### Algorithms
1.  **Projected Gradient Descent (PGD)**: Performs a gradient step followed by a projection onto the simplex.
2.  **Frank-Wolfe (FW)**: A projection-free method that optimizes a linear approximation of the objective function.
3.  **Pairwise Frank-Wolfe (PFW)**: A variant that moves "mass" between the best and worst vertices, accelerating convergence.
4.  **Away-step Frank-Wolfe (AFW)**: Improves PFW by dynamically choosing between a standard (FW) direction and an "away" direction.

### Step-Size Strategies
* **Optimal (Exact Line Search)**: Computes the optimal step-size at each iteration.
* **Armijo**: A backtracking rule that ensures sufficient progress.
* **Fixed**: Uses a constant, predefined step-size.

---

## Results and Analysis

The algorithms were evaluated on benchmark graphs from the **DIMACS challenge**. The analysis yielded the following insights:

* **Best Performance**: The combination of **Projected Gradient Descent (PGD)** with **$L_0$ regularization** and a **fixed step-size** proved to be the most effective, finding the largest cliques on average.
* **Impact of Regularization**: The $L_0$ regularization showed a clear superiority, better guiding the algorithms toward sparse solutions that represent cliques.
* **Computational Efficiency**: Fixed step-size strategies were the fastest. Exact line search, while theoretically powerful, was too slow for practical use with PGD.
* **Robustness**: The Frank-Wolfe algorithms (FW and AFW) proved to be more stable and less sensitive to the choice of step-size compared to PGD and PFW.

In conclusion, the project highlighted the strong synergy between the PGD algorithm and a regularization function that accurately models the combinatorial nature of the problem, demonstrating the effectiveness of continuous optimization for solving the maximum clique problem.
