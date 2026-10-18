# 🛍️ Mall Customer Segmentation: Clustering & Dimensionality Reduction

Unsupervised machine learning project that segments mall customers into meaningful groups using **K-Means, Hierarchical Clustering and DBSCAN**, visualises them with **PCA and t-SNE**, and turns the clusters into **business recommendations**.

![Python](https://img.shields.io/badge/Python-3.x-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Problem Statement

A shopping mall wants to understand its customers better so it can run targeted marketing instead of one-size-fits-all campaigns. Using only demographic and spending data, can we discover natural customer groups, and what should the mall do for each one?

## 🎯 Objectives

- Explore and prepare the customer dataset for clustering
- Build and tune **K-Means**, **Agglomerative Hierarchical** and **DBSCAN** models
- Choose the number of clusters using the **Elbow Method** and **Silhouette Score**
- Reduce dimensions with **PCA** and **t-SNE** to visualise the segments
- Profile each segment and recommend marketing actions

## 📂 Dataset

| Item | Detail |
|---|---|
| Records | 200 customers |
| Columns | `CustomerID`, `Genre`, `Age`, `Annual Income (k$)`, `Spending Score (1-100)` |
| Missing values | None |
| Duplicates | None |

**Features used for clustering:** `Age`, `Annual Income (k$)`, `Spending Score (1-100)`

- `CustomerID` was dropped because it is only an identifier.
- `Genre` was excluded from the initial segmentation so that gender does not influence the distance calculations.
- All features were standardised with `StandardScaler`, since clustering algorithms are distance-based.

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `scikit-learn` · `SciPy` · `Jupyter Notebook`

## 🔬 Methodology

1. **Data loading and checks:** shape, data types, missing values, duplicates, summary statistics
2. **Preprocessing:** feature selection and standardisation
3. **K-Means clustering**
   - Demonstrated the *random initialisation trap* (different seeds → different inertia)
   - Used `k-means++` with `n_init=10` for stable results
   - Elbow Method suggested **k = 4**, but Silhouette Score analysis (k = 2 to 10) favoured **k = 6**
   - Compared k = 4 and k = 6 on both inertia and Silhouette Score before finalising k = 6
4. **Hierarchical clustering:** Ward-linkage dendrogram and Agglomerative Clustering with 6 clusters
5. **DBSCAN:** tuned `eps` across 0.3 to 1.0 and selected `eps = 0.5`, `min_samples = 5`
6. **Dimensionality reduction:** PCA (2 components) and t-SNE (perplexity 30), coloured by K-Means cluster
7. **Cluster analysis:** segment profiling and business recommendations

## 📊 Results

### Model comparison

| Algorithm | Clusters | Silhouette Score | Notes |
|---|---|---|---|
| **K-Means** | 6 | **0.4284** | Best score on all 200 customers |
| Agglomerative (Ward) | 6 | 0.4201 | Comparable; dendrogram shows how clusters merge |
| DBSCAN (eps=0.5) | 6 | 0.4817* | 60 customers (30%) labelled as noise |

\*DBSCAN's score excludes noise points, so it is **not directly comparable** with the other two models.

### K-Means: choosing k

| k | Inertia | Silhouette Score |
|---|---|---|
| 4 | 205.23 | 0.4040 |
| 6 | 133.87 | 0.4284 |

Lower inertia always comes with more clusters, so the Silhouette Score was used as the deciding metric. Among k = 2 to 10, k = 6 scored highest.

### PCA

Two principal components retain **77.57%** of the variance (PC1: 44.27%, PC2: 33.31%).

### Customer segments (K-Means, k = 6)

| Cluster | Segment | Size | Avg Age | Avg Income (k$) | Avg Spending Score |
|---|---|---|---|---|---|
| 0 | Mature, moderate spenders | 45 | 56.3 | 54.3 | 49.1 |
| 1 | Young, moderate spenders | 39 | 26.8 | 57.1 | 48.1 |
| 2 | High income, low spending | 33 | 41.9 | 88.9 | 17.0 |
| 3 | High income, high spending | 39 | 32.7 | 86.5 | 82.1 |
| 4 | Young, low income, high spending | 23 | 25.0 | 25.3 | 77.6 |
| 5 | Low income, low spending | 21 | 45.5 | 26.3 | 19.4 |

<!--
Add your saved plots here after exporting them from the notebook, e.g.:
![Elbow Method](images/elbow_method.png)
![K-Means Segments](images/kmeans_segments.png)
![Dendrogram](images/dendrogram.png)
![PCA vs t-SNE](images/pca_tsne.png)
-->

## 💡 Business Recommendations

- **Retain high-value customers (Cluster 3):** high income and high spending. Offer loyalty programmes, premium experiences and personalised offers.
- **Grow engagement (Cluster 2):** high income but low spending. Investigate preferences and barriers, and run targeted campaigns to convert potential into spend.
- **Win over young spenders (Cluster 4):** high spending on a low income. Affordable, trendy products and attractive deals should appeal.
- **Tailor promotions (Clusters 0, 1, 5):** each has a different age, income and spending mix. Segment-specific promotions use the marketing budget more efficiently.
- **Largest segment:** Cluster 0 (22.5% of customers) is the best fit for broad retention campaigns.

> Revenue and profitability data would be needed to confirm which segment is truly the most valuable.

## ⚠️ Limitations

- Small dataset (200 customers), so results may not generalise.
- The Silhouette Score (around 0.4) indicates moderate cluster separation, not sharply distinct groups.
- DBSCAN is sensitive to `eps`: low values flagged up to 67% of customers as noise, while high values merged everything into one cluster.
- Spending Score is a pre-computed metric; real transaction data would give richer segments.

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/Nivetha-B93/<your-repo-name>.git
cd <your-repo-name>

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn scipy openpyxl jupyter

# 3. Launch the notebook
jupyter notebook Clustering_and_Dimensionality_Reduction.ipynb
```

The notebook loads the dataset directly from a GitHub URL, so an internet connection is required (`openpyxl` is needed to read the `.xlsx` file).

## 📁 Project Structure

```
├── Clustering_and_Dimensionality_Reduction.ipynb   # Full analysis
├── README.md
└── images/                                         # Saved plots (optional)
```

## 🔮 Future Improvements

- Include `Genre` (one-hot encoded) and test whether it changes the segments
- Try Gaussian Mixture Models and Silhouette analysis plots per cluster
- Tune DBSCAN with a k-distance graph
- Build an interactive Power BI / Tableau dashboard of the segments
- Test on real transactional retail data (RFM analysis)

## 👩‍💻 Author

**Nivetha B**
Aspiring Data Scientist | Python · SQL · Power BI · Tableau · Machine Learning

- GitHub: [Nivetha-B93](https://github.com/Nivetha-B93)
- LinkedIn: [nivetha-b-822545318](https://www.linkedin.com/in/nivetha-b-822545318)

---

⭐ If you found this project useful, consider giving it a star!
