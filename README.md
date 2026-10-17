# Who Reads AI Summaries? Predicting AI-Summary Engagement and Breadth of Generative-AI Use among UK Adults

> Independent research project · ChoHang Huang · Durham University Business School · Oct–Dec 2026
> Status: 🚧 In progress

## Research questions
1. Which demographic, digital-behaviour and AI-attitude factors are associated with how often UK adults read AI summaries in search results?
2. Which factors are associated with the breadth of generative-AI use among AI users?
3. Do machine-learning models predict these outcomes better than conventional statistical models?
4. How much predictive value do digital behaviour and AI attitudes add beyond demographics?

## Data
Ofcom *Adults' Media Literacy Tracker 2025*, respondent-level data (released 28 Jan 2026).
Download from the [Ofcom statistical release calendar 2026](https://www.ofcom.org.uk/about-ofcom/our-research/statistical-release-calendar-2026) and place the CSV in `data/` (raw data is not committed to this repo).

## Repository structure
```
├── PLAN.md          项目规划书 / Project plan (written before modelling)
├── LOG.md           项目日志 / Research log
├── notebooks/       01_cleaning → 02_eda → 03_regression → 04_ml → 05_clustering
├── figures/         Exported charts
├── report/          Final report (PDF)
├── demo/            Interactive demo (Streamlit)
└── data/            (not tracked) raw CSV goes here
```

## Methods (planned)
Ordered logit · Binomial / Poisson GLM · Regularised logistic regression · Random forest · Gradient boosting (XGBoost) · k-means clustering · Permutation importance / SHAP

## Key findings
_To be added after analysis._

## Limitations
Cross-sectional, self-reported survey data — results are associations and predictions, not causal effects.
