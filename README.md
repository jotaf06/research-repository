# Evaluating and Improving Robustness of Once-for-All AutoML approach

This repository contains the code, notebooks, and results for the paper **"Evaluating and Improving Robustness of Once-for-All AutoML approach"**, submitted to BRACIS 2026.

We evaluate and improve the robustness of evolutionary subnet selection within the [Once-for-All (OFA)](https://github.com/mit-han-lab/once-for-all) framework against natural image corruptions, using the Visual Wake Words (VWW) dataset.

---

## Repository Structure

```
├── BRACIS-results/
│   ├── standard/              # Results from the standard evolutionary search
│   ├── fitness-robust/        # Results from the robustness-aware fitness search
│   └── fitness-weighted/      # Results from the weighted fitness search (w = 0.0 to 1.0)
│
├── notebooks/
│   ├── standard-method/       # Standard subnet search (accuracy-only fitness)
│   ├── fitness-robust/        # Robustness-aware search (one notebook per hardware config)
│   ├── fitness-weighted/      # Weighted fitness search (accuracy + robustness in a weighted sum)
│   └── utils/                 # Helper scripts for evaluation and analysis
│
└── README.md
```

---

## Methods

Three evolutionary search strategies are compared:

| Method | Description |
|---|---|
| **Standard** | Fitness based solely on clean accuracy |
| **Fitness-Robust** | Fitness integrates a robustness metric (MCA over corruptions) |
| **Fitness-Weighted** | Fitness is a weighted combination: `w * robustness + (1-w) * accuracy` |

Each strategy is evaluated across 6 hardware configurations:

| Config | MACs (M) | Peak Memory (KB) |
|---|---|---|
| 18M 128KB | 18 | 128 |
| 18M 256KB | 18 | 256 |
| 18M 512KB | 18 | 512 |
| 52M 128KB | 52 | 128 |
| 52M 256KB | 52 | 256 |
| 52M 512KB | 52 | 512 |

---

## Getting Started

### Prerequisites

This project runs on Jupyter Notebooks with all dependencies in the firsts cells.

```bash
clone github.com/path/to/research-repository
cd research-repository
```

### Running the Notebooks

#### 1. Standard Search
Open and run:
```
notebooks/standard-method/Standard-Search.ipynb
```
This performs the standard evolutionary search and evaluates subnets on clean and corrupted VWW data.

#### 2. Robustness-Aware Search
One notebook per hardware configuration, e.g.:
```
notebooks/fitness-robust/Robustness-Search-18_128.ipynb
```
Each notebook runs the robustness-aware evolutionary search under the corresponding MACs and memory constraints. This was done to enable parallel running.

#### 3. Weighted Fitness Search
```
notebooks/fitness-weighted/Robustness-Search-with-Weights.ipynb
```
Runs the weighted search for robustness weights `w ∈ {0.0, 0.2, 0.4, 0.6, 0.8, 1.0}`, repeating each search 10 times.

#### 4. Helper Scripts
```
notebooks/utils/helper_scripts.ipynb
```
Contains utilities for loading results, computing MCA, standard deviation, accuracy drop per corruption, and generating plots and tables.

---

## Results

Pre-computed results are available in `BRACIS-results/` and follow this naming convention:

**Standard:**
```
cfg_{macs}_{mem}_acc_list.json         # clean accuracy per subnet (30 samples)
cfg_{macs}_{mem}_corruption_results.json  # corrupted accuracy per subnet
```

**Fitness-Robust:**
```
ACCS_{macs}_{mem}_clean_ROBUST.json
ACCS_{macs}_{mem}_corrupted_ROBUST.json
```

**Fitness-Weighted** (config `18M_128KB`, weight `w`):
```
cfg_18_128_acc_{w}.json       # clean accuracy
cfg_18_128_cfg_{w}.json       # subnet architectures
corrupted_accs_{w}.json       # corrupted accuracy
```

---