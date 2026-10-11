# K-Means Clustering: Unsupervised Learning Walkthrough

A hands-on notebook demonstrating **K-Means clustering** on synthetic data, including how to choose the optimal number of clusters (K) using the **Elbow Method**, **KneeLocator**, and the **Silhouette Score**.

---

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Key Learnings](#key-learnings)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## Overview

Unsupervised learning finds hidden structure in data without labels. This project applies **K-Means**, a centroid-based clustering algorithm, to a synthetic 2D dataset and validates the choice of K using both visual and quantitative techniques.

**Goals**
- Generate and visualize clustered data
- Split data into train/test sets and fit K-Means on the training set
- Select the optimal K using the Elbow Method (manual + automated)
- Evaluate cluster quality with the Silhouette Score
- Predict cluster assignments for unseen data

---

## Dataset

Synthetic data generated with `sklearn.datasets.make_blobs`:

| Parameter     | Value |
|---------------|-------|
| `n_samples`   | 100   |
| `centers`     | 3     |
| `n_features`  | 2     |
| `random_state`| 23    |

Train/test split: **67% / 33%** (`random_state=42`).

---

## Methodology

1. **Data generation**: create 3 Gaussian blobs in 2D.
2. **Visualization**: scatter plot of the raw data colored by true blob label.
3. **Train/test split**: `train_test_split` with `test_size=0.33`.
4. **Elbow Method (manual)**: fit K-Means for K = 1..10 using `k-means++` initialization and record WCSS (`inertia_`).
5. **Elbow Method (automated)**: use `KneeLocator` from the `kneed` library to detect the elbow programmatically.
6. **Final model**: fit `KMeans(n_clusters=3, init="k-means++")` on training data.
7. **Prediction**: assign clusters to the held-out test set with `.predict()`.
8. **Silhouette analysis**: compute silhouette scores for K = 2..10.

---

## Results

### WCSS (Elbow Method)

| K  | WCSS      |
|----|-----------|
| 1  | 3605.14   |
| 2  | 832.43    |
| 3  | 124.20    |
| 4  | 100.92    |
| 5  | 91.02     |
| 6  | 87.06     |
| 7  | 63.65     |
| 8  | 52.50     |
| 9  | 45.39     |
| 10 | 43.79     |

The sharp drop up to K=3 followed by a flattening curve indicates the elbow at **K = 3**, confirmed by `KneeLocator` (`kl.elbow == 3`).

### Silhouette Scores

| K  | Silhouette |
|----|-----------|
| 2  | 0.722     |
| 3  | **0.818** |
| 4  | 0.719     |
| 5  | 0.558     |
| 6  | 0.408     |
| 7  | 0.336     |
| 8  | 0.388     |
| 9  | 0.337     |
| 10 | 0.406     |

The silhouette score peaks at **K = 3 (0.818)**, matching the elbow result and the true number of generated blobs.

> Note: scores may vary slightly between runs because K-Means is initialized randomly; set `random_state` in `KMeans` for fully reproducible results.

---

## Project Structure

```
.
├── Unspervised_learning.ipynb   # Main notebook
└── README.md                    # Project documentation
```

---

## Installation

**Requirements**
- Python 3.9+
- Jupyter Notebook / JupyterLab

**Install dependencies**

```bash
pip install numpy pandas matplotlib plotly scikit-learn kneed jupyter
```

---

## Usage

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
jupyter notebook Unspervised_learning.ipynb
```

Run the cells top to bottom to reproduce the plots and metrics.

---

## Key Learnings

- **K-Means** partitions data by minimizing within-cluster sum of squares (WCSS).
- The **Elbow Method** is intuitive but subjective; **KneeLocator** removes the guesswork.
- The **Silhouette Score** (range -1 to 1) offers an independent check: higher means better-separated, more cohesive clusters.
- Fitting on the training set and calling `.predict()` on the test set shows how a trained clustering model generalizes to new points.
- `k-means++` initialization gives more stable, faster-converging results than random initialization.

---

## Future Improvements

- Set `random_state` / `n_init` in `KMeans` for reproducibility
- Plot the silhouette scores and cluster centroids
- Try other algorithms (DBSCAN, Agglomerative, Gaussian Mixture) and compare
- Apply the pipeline to a real-world dataset (e.g., customer segmentation)
- Add feature scaling (`StandardScaler`) for non-synthetic data

---

## License

This project is licensed under the MIT License. Feel free to use and modify it.
