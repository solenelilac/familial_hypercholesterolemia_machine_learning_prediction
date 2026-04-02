# Predicting Familial Hypercholesterolemia from Genomic Variant Expression Data

**Author:** Lilac Zihui Zhao  
**Date:** 31st March 2025

---

## Overview

This project evaluates whether genomic variant expression data can be used to predict **Familial Hypercholesterolaemia (FH)** — a genetic condition causing reduced LDL clearance and elevated cardiovascular risk — and identifies which genetic variants and demographic factors are most associated with the disease.

The analysis delivers a complete end-to-end machine learning pipeline addressing three core questions:
1. Can FH be predicted from genetic variant expression data?
2. Which genetic variants are most influential?
3. How do demographic factors correlate with FH status?

---

## Files

| File | Description |
|---|---|
| `fhprediction_lilac.ipynb` | Full analysis notebook (EDA, dimensionality reduction, differential expression, modelling, feature importance) |
| `fhprediction_lilac.pdf` | Slide deck presentation of findings |

---

## Key Findings

| Question | Finding |
|---|---|
| Can FH be predicted from genetic variant data? | Yes — SVM achieves **AUC = 0.999**, 100% FH recall, and 98.2% specificity on held-out test set |
| Which genetic variants are most influential? | **Var_X100248** (most strongly downregulated) and **Var_X100251** (most strongly upregulated) — consistent across Random Forest, LASSO, and differential expression analysis |
| How do demographics correlate with FH? | All four variables significantly associated (p < 0.001) — FH patients are older, more likely male, more sedentary, and report poorer mental wellbeing |

---

## Analysis Structure

| Section | Description |
|---|---|
| 1. Exploratory Data Analysis | Data structure, class distribution, clinical variable associations |
| 2. Dimensionality Reduction | PCA and UMAP visualisations |
| 3. Differential Expression Analysis | Volcano plot and bubble plot |
| 4. Predictive Modelling | LASSO Logistic Regression, Random Forest, SVM |
| 5. Feature Importance | Permutation importance on held-out test set |
| 6. Limitations & Recommendations | Dataset constraints and future directions |

---

## Dataset

- **n = 2,761** individuals; **2,213 FH cases** (80%), **548 No FH** (20%)
- **265** genetic variant expression features (`Var_X100000` – `Var_X100264`)
- UK population prevalence of FH: ~1 in 250 (currently under 8% identified)
- **Known limitations:** class imbalance (80/20), anonymised variant labels, no LDL panel, no longitudinal follow-up

---

## Results

### Demographics — Who Gets FH?

![Demographic distributions](fig_distributions.png)
![Age distribution by FH status](fig_age_distribution_by_fhstatus.png)

FH patients are significantly older, more likely male, more sedentary, and report poorer mental wellbeing (all p < 0.001, logistic regression).

---

### Dimensionality Reduction

![PCA vs UMAP](fig_pca_umap.png)

PCA shows no clear linear cluster separation — consistent with a polygenic architecture where signal is distributed across many variants. UMAP captures the non-linear structure and reveals clearer FH vs No FH grouping.

---

### Differential Expression

![Volcano plot](fig_volcano.png)
![Bubble plot](fig_bubble.png)

No single variant exceeds ±1.0 log₂FC, confirming a polygenic signal. Key variants:
- **Var_X100248** — most strongly downregulated (log₂FC = −0.630, ~54% lower expression in FH)
- **Var_X100251** — most statistically significant upregulated variant
- **Var_X100238** — largest upregulated fold change (log₂FC = +0.342)

---

### Predictive Modelling

![ROC curves](fig_roc_curves.png)
![Confusion matrices](fig_confusion_matrices.png)

| Model | AUC | FH Recall | Specificity | Precision |
|---|---|---|---|---|
| Logistic (LASSO) | 0.996 | 97.1% | 95.5% | 98.9% |
| Random Forest | 0.994 | 99.8% | 64.5% | 91.9% |
| **SVM (RBF) ✓** | **0.999** | **100.0%** | **98.2%** | **99.6%** |

SVM is recommended: it achieves perfect FH recall with near-perfect specificity.

---

### Feature Importance

![Feature importance](fig_feature_importance.png)

**Var_X100248** and **Var_X100251** are the top two features across both Random Forest and LASSO — consistent with the differential expression results.

---

## Methods

A strict leakage-free pipeline was implemented throughout:
- All preprocessing inside `sklearn` `Pipeline` with `ColumnTransformer`
- Hyperparameter tuning via `GridSearchCV` on training set only
- Held-out test set evaluated exactly once

---

## Dependencies

```bash
pip install numpy pandas matplotlib seaborn scipy statsmodels scikit-learn umap-learn
```

---

