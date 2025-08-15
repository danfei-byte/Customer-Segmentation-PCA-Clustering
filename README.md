# Customer Segmentation with PCA & K-Means

## Overview
This project applies **Principal Component Analysis (PCA)** and **K-Means clustering** to retail customer data (2240 observations, 29 variables) sourced from Kaggle. Our goal is to uncover distinct customer segments based on demographic and behavioral patterns, then translate findings into **managerial actions** (who to target, how to message, where to invest).

## Dataset
We uses customer records from a grocery retailer’s internal database, originally sourced via Kaggle. 

The dataset includes:
1. **Demographics** – birth year, education, marital status, household composition, income, enrollment date.  
2. **Purchase Behavior** – 2-year spend across wine, fruits, meat, fish, sweets, gold. 
3. **Promotional Response** – participation in past campaigns, discount purchases.  
4. **Shopping Channel Use** – purchase counts by website, store, and catalog; recent website visits.

## Method & Workflow

### 1. Data Cleaning & Feature Engineering
   - Removed **24 rows** with missing `Income`.
   - Outlier caps: **Income ≤ $600,000**; **Age ≤ 90**.
   - Engineered: **Tenure_Days** (from `Dt_Customer`), **Age**, **Living_With** (Alone/Partner), **Children**, **Family_Size**, **Is_Parent**, **Spent** (total across categories).
   - Consolidated **Education** to Undergraduate / Graduate / Postgraduate.
   - Dropped IDs and redundant columns.
   - **Note on timing:** `Tenure_Days` computed relative to the dataset’s latest enrollment date; `Age` reflects current-year calculation (absolute), while tenure/recency are relative to the snapshot.

### 2. PCA
  - Standardized features for equal weighting.
  - Excluded campaign response variables from PCA input.
  - Selected **3 components** (explained ~56% variance) based on scree plot elbow.
  - Interpreted PCA loadings to understand behavioral patterns.

    <img src="https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/scree_plot.png" width="700">

### 3. Clustering
  - Applied **K-Means** to standardized PCA scores.
  - Chose **K = 4** (Elbow Method; K=5/7 also plausible; opted for simplicity/interpretability).
  - **Silhouette Score**: 0.359 (moderate separation).
  - Merged cluster labels back to original data for demographic/value profiling.

    <img src="https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/elbow_plot.png" width="700">

## PCA Interpretation (high level)
- **PC1:** Higher spend/income and frequent purchases across core categories; negatively aligned with larger family structure.
- **PC2:** Digital/promotions engagement (web purchases, deal usage) and larger households with teens/kids; less tied to niche/premium categories.
- **PC3:** Longer tenure and higher web visit frequency; negatively aligned with older age/higher education/family-with-teens → skew younger, digitally active.

   <img src="https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/pca_loadings.png" width="500"> <img src="https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/pc1_pc2_scatter_clusters.png" width="500">


## Cluster Profiles (behavior aligned with PCA + factual summaries)

### Cluster 1 — Socially Engaged, Lifestyle-Oriented Shoppers
- **Behavior (PCs):** High **PC2**, moderately +**PC1/+PC3** → lifestyle/social/digital engagement; active across channels; trend-driven.
- **Facts:** Income **$58,462**; Age **57.9**; Children **1.28**; Family Size **1.94**; **99%** parents; **Spent ≈ $860**.
- **Use:** Community & influencer campaigns; lifestyle storytelling; omni-channel activation.

### Cluster 2 — Premium High-Value Customers
- **Behavior (PCs):** Very high **PC1**, slightly −**PC2/PC3** → affluent, consistent high-volume spending in core categories.
- **Facts:** Income **$75,792** (highest); Age **57.0**; Children **0.02**; Family Size **0.61**; **2%** parents; **Spent ≈ $1,353** (highest).
- **Use:** VIP/tiered loyalty, exclusives, retention-first programs.

### Cluster 3 — Detached Bargain Seekers
- **Behavior (PCs):** −**PC1/PC2**, +**PC3** → low engagement/low spend; occasionally responds to digital promos/deals.
- **Facts:** Income **$29,260** (lowest); Age **48.0** (youngest); Children **0.85**; Family Size **1.45**; **79%** parents; **Spent ≈ $100** (lowest).
- **Use:** Discounts, targeted promos, win-back strategies.

### Cluster 4 — Infrequent, Selective Buyers
- **Behavior (PCs):** −**PC1/PC3**, mildly +**PC2** → irregular purchases, weak attachment; some seasonal/message fit.
- **Facts:** Income **$48,771**; Age **61.7** (oldest); Children **1.53**; Family Size **2.26**; **99%** parents; **Spent ≈ $268**.
- **Use:** Seasonal pushes, curated bundles, reactivation cadence.

   <image src="https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/cluster_bars_income_age_spent.png" width="3000">


## Managerial Insights
- **C1:** Creator/community programs; lifestyle campaigns; multi-channel journeys.
- **C2:** Prioritize retention; premium exclusives; early access; white-glove service.
- **C3:** Price-led offers; cart-abandon triggers; time-boxed deals; win-back.
- **C4:** Seasonality-aligned curation; reminders; frequency incentives.

## Limitations
- **K-Means assumptions** (spherical/even clusters) may oversimplify real behavior.
- **Silhouette 0.359** → moderate separation; behavioral overlap likely.
- **Static snapshot** (no temporal dynamics/seasonality).
- **Potential feature bias** in PCA weighting can influence cluster bounds.

## Tools & Libraries
- **Python**: pandas, numpy, seaborn, matplotlib, scikit-learn
- **Techniques**: PCA, K-Means, Elbow Method, Silhouette Score

The full notebook can be found [here](https://github.com/danfei-byte/Customer-Segmentation-PCA-Clustering/blob/f85ec0465cfcbb1446a08c58c756d0f84a070349/Customer%20Segmentation%20with%20PCA%20%26%20K-Means5.ipynb).

