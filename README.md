<div align="center">

<img src="assets/banner.svg" alt="Wholesale Customer Segmentation" width="100%"/>

<br/>

<a href="https://git.io/typing-svg"><img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=2800&pause=900&color=00F5D4&center=true&vCenter=true&width=700&lines=Finding+hidden+customer+types+in+the+data;440+wholesale+customers.+6+spending+features.;Skewed.+Outlier-heavy.+Beautifully+messy.;EDA+done+%E2%9C%85+Feature+prep+next+%F0%9F%9A%80" alt="Typing SVG" /></a>

<br/>

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

![Status](https://img.shields.io/badge/Status-EDA%20Complete-00F5D4?style=flat-square)
![Next](https://img.shields.io/badge/Next-Feature%20Preparation-F15BB5?style=flat-square)
![Type](https://img.shields.io/badge/Learning-Unsupervised-FEE440?style=flat-square&labelColor=222)

</div>

<img src="assets/divider.svg" width="100%"/>

## 🧭 Table of Contents

- [🎯 Project Goal](#-project-goal)
- [📦 Dataset](#-dataset)
- [🗺️ Pipeline](#️-pipeline)
- [🔍 EDA Findings](#-eda-findings)
- [🧠 Key Decisions](#-key-decisions)
- [✅ Progress & Roadmap](#-progress--roadmap)
- [🚀 Getting Started](#-getting-started)

<img src="assets/divider.svg" width="100%"/>

## 🎯 Project Goal

Build an **end-to-end unsupervised machine learning project** that segments wholesale customers by their **purchasing behavior**, so each discovered segment can be understood, named, and acted on.

> *Who are the big spenders, who are the specialists, and who buys a bit of everything?*

<img src="assets/divider.svg" width="100%"/>

## 📦 Dataset

**UCI Wholesale Customers**: 440 customers of a wholesale distributor.

| Type | Columns | Role |
|---|---|---|
| 🛒 Purchasing features | `Fresh` `Milk` `Grocery` `Frozen` `Detergents_Paper` `Delicassen` | Used for clustering |
| 🏷️ Categorical metadata | `Channel` `Region` | Held back for cluster **profiling** |

<img src="assets/divider.svg" width="100%"/>

## 🗺️ Pipeline

```mermaid
flowchart LR
    A[📥 Raw Data] --> B[🔍 EDA]
    B --> C[🧪 Feature Prep<br/>Log + Scaling]
    C --> D[🎯 K-Means]
    D --> E[⚖️ Compare<br/>DBSCAN / Hierarchical]
    E --> F[🏷️ Profile Segments<br/>Channel · Region]
    F --> G[📊 Insights]

    style B fill:#00F5D4,stroke:#00F5D4,color:#000
    style C fill:#F15BB5,stroke:#F15BB5,color:#000
    style A fill:#2b2d42,stroke:#8d99ae,color:#fff
    style D fill:#2b2d42,stroke:#8d99ae,color:#fff
    style E fill:#2b2d42,stroke:#8d99ae,color:#fff
    style F fill:#2b2d42,stroke:#8d99ae,color:#fff
    style G fill:#2b2d42,stroke:#8d99ae,color:#fff
```

<sub>🟢 Completed &nbsp;•&nbsp; 🩷 Up next &nbsp;•&nbsp; ⚫ Planned</sub>

<img src="assets/divider.svg" width="100%"/>

## 🔍 EDA Findings

Data quality checks (structure, missing values, duplicates, data types) and descriptive statistics were completed first. Then the deeper analysis:

### 1️⃣ Distributions: everything is right-skewed

<div align="center">
<img src="assets/skewness.svg" alt="Skewness bar chart" width="85%"/>
</div>

Most customers spend modestly, while a small group spends enormously. `Delicassen` (11.15) is the most extreme.

> ⚠️ **ML implication:** strong skew lets huge values dominate distance calculations in distance-based clustering.

### 2️⃣ Outliers: detected with the IQR method

`Lower = Q1 − 1.5 × IQR` &nbsp;|&nbsp; `Upper = Q3 + 1.5 × IQR`

Spending can't be negative, so the **upper bound** is what matters.

| Feature | Upper Bound | Outliers |
|---|---:|---:|
| Fresh | 37,642.75 | 20 |
| Milk | 15,676.13 | 28 |
| Grocery | 23,409.88 | 24 |
| **Frozen** | 7,772.25 | **43** 🔥 |
| Detergents_Paper | 9,419.88 | 30 |
| Delicassen | 3,938.25 | 27 |

> 📌 These are **feature-level** counts. One customer can be an outlier in several features, so they don't equal unique customers.

### 3️⃣ Correlations: spending moves together

| Feature Pair | Correlation | Strength |
|---|---:|---|
| Grocery ↔ Detergents_Paper | **0.92** | 🔥🔥🔥 Very strong |
| Milk ↔ Grocery | **0.73** | 🔥🔥 Strong |
| Milk ↔ Detergents_Paper | **0.66** | 🔥 Fairly strong |

Customers who buy lots of Grocery also tend to buy lots of Detergents_Paper. Other relationships were weaker.

<img src="assets/divider.svg" width="100%"/>

## 🧠 Key Decisions

<details open>
<summary><b>🚫 Why we are NOT deleting outliers</b></summary>
<br/>

In customer segmentation, an unusually high-spending customer may be a **genuine segment** the algorithm should discover. Deleting them risks erasing the most valuable customers from the analysis.

K-Means uses distance to centroids, so extreme customers can pull centroids toward themselves. The plan is to handle this with transformations and algorithm comparison rather than deletion.

</details>

<details open>
<summary><b>🔗 Why we are NOT dropping correlated features (yet)</b></summary>
<br/>

High correlation (e.g. Grocery ↔ Detergents_Paper at 0.92) does not automatically mean a feature should be removed for clustering. We keep all six purchasing features for now.

</details>

<details open>
<summary><b>🏷️ Why Channel & Region are excluded from clustering</b></summary>
<br/>

The goal is to segment by **purchasing behavior**. Channel and Region are categorical metadata, so we use them *after* clustering to profile and interpret the segments.

</details>

<details>
<summary><b>🔬 Alternatives to investigate later</b></summary>
<br/>

- Log transformation
- DBSCAN
- Hierarchical clustering (note: it does **not** automatically solve the outlier problem, since extreme points still influence distances)
- Side-by-side comparison of clustering approaches

</details>

<img src="assets/divider.svg" width="100%"/>

## ✅ Progress & Roadmap

- [x] Dataset structure inspection
- [x] Missing-value, duplicate and data-type checks
- [x] Descriptive statistics
- [x] Distribution & skewness analysis
- [x] Outlier analysis (IQR) + boxplots
- [x] Correlation analysis
- [x] Feature-selection reasoning
- [ ] **Feature preparation** ← *you are here*
  - [ ] What problem does **log transformation** solve?
  - [ ] What problem does **StandardScaler** solve?
  - [ ] Why use **both** before K-Means?
- [ ] K-Means clustering + choosing *k*
- [ ] Compare with DBSCAN / hierarchical clustering
- [ ] Cluster profiling with Channel & Region
- [ ] Final insights & business interpretation

<img src="assets/divider.svg" width="100%"/>

## 🚀 Getting Started

```bash
# clone the repo
git clone https://github.com/Kartikshard/Whole-Sale-Customer-Segmentation.git
cd Whole-Sale-Customer-Segmentation

# install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn jupyter

# launch the notebook
jupyter notebook
```



<img src="assets/divider.svg" width="100%"/>

<div align="center">

**⭐ If this project helped you, consider giving it a star! ⭐**

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,14,18&height=120&section=footer" width="100%"/>

</div>