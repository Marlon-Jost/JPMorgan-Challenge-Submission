[<img src="https://qbraid-static.s3.amazonaws.com/logos/Launch_on_qBraid_white.png" width="150">](https://account.qbraid.com?gitHubUrl=https://github.com/Marlon-Jost/JPMorgan-Challenge-Submission.git&redirectUrl=start_here.ipynb)

# JPMorgan-Challenge-Submission

Qvengers Team
Members: Andrew Maciejunes | Marlon Jost | Andrei Chupakhin | Pranik Chainani | Vlad Gaylun


**Single-shot circuit generation + graph-partitioning pipeline that delivers up to 137× speed-ups on 20-asset problems and scales cleanly to 150-asset instances.**

## Key ideas
- **GPT-based circuit generator** - swaps the slow, iterative QAOA optimiser for a single transformer inference, eliminating hundreds of gradient-descent steps.  
- **Warm-start option** - feed the GPT suggestion into any classical optimiser when you want a final polish.  
- **Hardware-aware graph partitioner** - decomposes a large portfolio into *p*-node sub-graphs that fit comfortably on your GPU/QPU, then stitches the sub-solutions back together.  
- **Full pipeline notebook** - demonstrates end-to-end optimisation of a `150`-asset Nasdaq portfolio, including RMT denoising, spectral clustering, and recombination of sub-solutions.  

## Repository layout
- **benchmarks** - Scripts and Jupyter notebooks for comparing QOkit and GPT in terms of approximation ratio (AR), execution time, and memory usage. Also includes benchmarks for graph partitioning.
    - `benchmark_A.py` - This file is used to generated `results.csv`. It has been modified to take in user input when run from the terminal. It requires GPU support to run.
    - `results.csv` - Holds the results from running `benchmark_A.py` on `75` graphs. `5` graphs of each node size from `5` to `20`. 
    - `utils/graphs.py` - Contains supporting functions related to saving, loading, and generating graphs used by `benchmark_A.py`
    - `utils/parsing.py` - Contains logic for parsing the GPT tokens generated by the model. This converts tokens into quantum circuits.
    - `utils/run_circuit_qiskit.py` - A file that holds all the qiskit related code. Our model is evaluated on qiskit code in `benchmark_A.py`.
    - `utils/run_circuit_qokit.py` - A wrapper for `QOKit/qokit/examples/portfolio_optimization.ipynb` from the original QOKit repo. We compare our model to this function's output.
    - `utils/utils.py` - Miscileneous helper functions.
- **gpt-qaoa** - Training a custom model to generate quantum circuits from graphs (nodes represent assets with result attributes; edges represent correlations between assets).
- **partitioning** - Graph decomposition: solve the optimization problem for each subgraph using QOkit or the custom GPT model, then concatenate the results to obtain the final solution.
    - `Partitioning_classical_approach_210 nodes.ipynb` - demonstration of decomposition pipeline
    - `partitioning_with_gpt.py` - GPT-based circuit generator in paritioning workflow (working progress)
    - `partitioning_with_QOKit.py` - QOKit-based circuit simulator in partitioning workflow (working progress)


## ⚠️ Important Note about Checkpoints and Git LFS

> **Note:** Large binary assets (e.g. model checkpoints) are stored via **Git LFS**.  
> Due to GitHub bandwidth and file size limits, you may **encounter errors** when trying to download `gpt-qaoa/checkpoints` directly through `git clone` or `git lfs pull`.

### 🔧 Recommended workaround

1. Manually go to the [GitHub repo](https://github.com/Marlon-Jost/JPMorgan-Challenge-Submission/tree/main/gpt-qaoa/checkpoints)
2. Download the all files to your **local machine**
3. Upload them to your **qBraid instance** using the web interface

This ensures you get all the necessary files without hitting GitHub's LFS restrictions.
