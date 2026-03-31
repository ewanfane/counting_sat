# Dynamic Algorithm Selection in Exact Model Counting
### A Two-Stage Lightweight Portfolio for #SAT Solver Optimization

Exact model counting (#SAT) is a #P-complete problem, making it significantly more computationally expensive than standard SAT solving. This project addresses the "Algorithm Selection Problem" by implementing a machine learning pipeline that dynamically selects the most efficient solver for a given formula based on its structural characteristics.

## 🌟 Project Overview
The core innovation of this project is a **Two-Stage "Bouncer-Selector" Pipeline** designed to maximize reliability while minimizing overhead:

1.  **Stage 1: The Bouncer (Hardness Prediction)** – An ExtraTrees classifier identifies "intrinsically hard" instances likely to exceed the 5,000s timeout. By halting these early, the system avoids massive PAR-2 penalties.
2.  **Stage 2: The Selector (Solver Allocation)** – A multiclass Random Forest routes solvable instances to the fastest algorithm among a portfolio of 8 state-of-the-art counters (including GPMC, d4, and sharpSAT).



## ⚡ Technical Highlights
- **Lightweight Feature Engineering:** Optimized via Recursive Feature Elimination (RFE) to reduce the feature space from 75 down to the **25 most critical structural attributes**.
- **Consensus-Backed Ground Truth:** Training labels were generated using a rigorous three-model majority agreement protocol to ensure mathematical correctness across solver outputs.
- **Resource Efficiency:** The pipeline successfully prevented **550,000 seconds (6.37 days)** of wasted wall-clock CPU time.

## 📊 Performance Analytics
The portfolio was evaluated via 5-fold stratified cross-validation against the Single Best Solver (SBS) and the theoretical Virtual Best Solver (VBS).

| Metric | SBS (GPMC) | ML Portfolio (Ours) | Oracle (VBS) |
| :--- | :--- | :--- | :--- |
| **Exact Counts** | 1,378 | **1,389** | 1,401 |
| **PAR-2 Score** | 6,462,886 | **6,352,180** | 6,222,786 |
| **Oracle Gap** | 240,100 | **129,394** | 0 |

*Our model closed the gap to the theoretical optimum by **46.1%**.*



## 📂 Repository Contents
- `project.ipynb`: The primary research and development notebook containing data cleaning, RFE, model training, and evaluation.
- `instances_features_final.csv`: The dataset containing 30+ structural features for 2,023 CNF instances.
- `MC_performances_final.csv`: The performance matrix containing runtimes and model counts for all portfolio solvers.
- `requirements.txt`: Python environment dependencies.

## 🛠️ Getting Started
1. **Clone the repository:**
   ```bash
   git clone https://github.com/ewanfane/counting_sat
   ```
2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
3. **Run the notebook:**
   ```bash
   jupyter notebook project.ipynb
   ```