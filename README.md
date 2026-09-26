# Unsupervised Phenotyping of Cardiac Risk

A clustering and Bayesian Network approach to the UCI Heart Disease dataset: can distinct cardiac risk phenotypes be recovered from routine clinical measurements without any diagnostic label?

Developed as the final project for the **Unsupervised Learning** course (MSc in AI for Science and Technology), A.Y. 2025/26, by **Omar Degui, Lorenzo Luchesini, and Matteo Rigoni**.

## Overview

A fully unsupervised pipeline is applied to the [UCI Heart Disease dataset](https://archive.ics.uci.edu/dataset/45/heart+disease) (920 patients, 15 raw attributes):

1. **Preprocessing** — missing-value imputation (median/mode), one-hot encoding, `StandardScaler` standardisation, and Isolation Forest outlier removal (5% contamination).
2. **Dimensionality reduction** — PCA retaining 90% of variance (19 → 10 dimensions), mitigating the curse of dimensionality for distance-based clustering; t-SNE used only as a 2D visual diagnostic.
3. **Clustering** — three structurally distinct algorithms are compared: **K-means++** (centroid-based), **Agglomerative/Ward** (hierarchical), and **DBSCAN** (density-based), evaluated purely through internal validation (Silhouette, Davies–Bouldin).
4. **Bayesian Network** — a DAG is learned via BIC-scored Hill-Climb search over the clinical variables augmented with the discovered cluster label, then queried with exact inference (Variable Elimination) to interpret each phenotype.

**Key findings:** DBSCAN produced the cleanest partition (silhouette ≈ 0.40, Davies–Bouldin ≈ 0.90), while K-means++ and Ward independently converged on a consistent 3-cluster solution. A controlled experiment showed that standardisation is essential — the deceptively high silhouette obtained on unscaled data is an artefact of the cholesterol axis dominating the Euclidean metric. The learned Bayesian Network identifies the discovered cluster label as the most central node, directly parenting clinical findings such as chest-pain type and exercise-induced angina, confirming that the clusters correspond to distinct, clinically interpretable cardiac phenotypes.

Full methodology, figures, and results are in [`report/report.pdf`](report/report.pdf); a slide summary is in [`report/slides.pdf`](report/slides.pdf).

## Repository structure

```
notebooks/
└── cluster_analysis_heart_diseases.ipynb
report/
├── report.pdf
└── slides.pdf
```

## Data

This project uses the publicly available [UCI Heart Disease dataset](https://archive.ics.uci.edu/dataset/45/heart+disease) (Detrano et al., 1989). It is not redistributed here — download it from the UCI repository and adjust the `FILE_PATH` variable in the notebook.

## Requirements

```
numpy
pandas
matplotlib
seaborn
scikit-learn
scipy
yellowbrick
kneed
networkx
pgmpy
```

Install with:
```bash
pip install -r requirements.txt
```

## Authors

- **Omar Degui**
- **Lorenzo Luchesini** — [GitHub](https://github.com/lorenzoluchesini02)
- **Matteo Rigoni**

MSc students, AI for Science and Technology (UniMiB / UniMi / UniPv)

## License

MIT — see [LICENSE](LICENSE).
