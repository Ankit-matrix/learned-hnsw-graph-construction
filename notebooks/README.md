# Notebooks

Run in order — each notebook loads saved outputs from the previous one.

| Notebook | Covers | Key output |
|---|---|---|
| `01_baseline_vanilla_hnsw.ipynb` | Dataset download (Fashion-MNIST via ann-benchmarks), 80/20 train/test query split, the shared `evaluate_index` harness (Recall@10 vs. QPS sweep), and the vanilla HNSW baseline (`M=16, ef_construction=200`). | Baseline AUC = 0.9850 |
| `02_bayesian_parameter_optimization.ipynb` | Bayesian optimization (Optuna, TPE) over `M ∈ {8,16,24,32,48}` × `ef_construction ∈ {100,150,200,300,400}`, compared head-to-head against exhaustive grid search and random search on the same space. | Bayesian AUC = 0.9908; Grid = 0.9939; Random = 0.9947 (all converge on `M=48, ef_construction=400`) |
| `03_learned_layer_assignment_and_ablation.ipynb` | `CustomHNSW` (pure-Python HNSW with swappable layer-assignment / neighbor-selection hooks), query-trace collection, XGBoost-based learned layer assignment, a hub-first "combined pipeline" heuristic, and a full ablation study. | Learned layer AUC = 0.9750 (negative result, investigated); combined pipeline AUC = 0.9901 |

Notes:
- `01_baseline_vanilla_hnsw.ipynb` is a clean rerun of the Week 1 work (the original had a Windows build error installing `hnswlib` from source that resolved itself via a cached wheel; this version runs cleanly end-to-end).
- Notebook 3 explicitly logs that the learned neighbor-selection component (Section 3.3 in the proposal) was **not** run at full scale (N=60,000) due to projected ~14-hour runtime in pure Python — see the root `README.md` for the honest accounting of what was and wasn't completed.
