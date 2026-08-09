# Employee Attrition Analysis

Statistical analysis of employee attrition using univariate statistical tests and multivariable logistic regression.

This project was conducted as part of the **Statistical Theory Course**. The objective is to investigate which employee and workplace characteristics are associated with employee attrition and to determine which relationships remain significant when multiple characteristics are considered simultaneously.

## Authors

- Matan Rojansky
- Itay Noori

## Research Questions and Hypotheses

The analysis examines three main hypotheses:

1. Selected numerical and ordinal employee characteristics differ between employees who leave the organization and employees who remain.
2. Selected categorical employee and workplace characteristics are associated with attrition status.
3. Selected employee characteristics retain significant associations with attrition after simultaneous adjustment for other predictors in a multivariable logistic regression model.

## Dataset

The analysis uses the **IBM HR Analytics Employee Attrition & Performance** dataset.

The dataset contains **1,470 employee observations** and includes demographic, compensation-related, career, and workplace characteristics.

The outcome variable is:

- `Attrition` — whether an employee left the organization (`Yes`) or remained (`No`).

In the analyzed dataset:

- 237 employees (16.1%) left the organization.
- 1,233 employees (83.9%) remained.

The dataset is downloaded automatically within the notebook using the `kagglehub` package from the Kaggle dataset:

`pavansubhasht/ibm-hr-analytics-attrition-dataset`

The notebook then loads:

`WA_Fn-UseC_-HR-Employee-Attrition.csv`

Therefore, no manual download of the dataset is required when the notebook is executed in an environment with internet access.

## Statistical Analysis

The notebook contains the complete analysis used for the accompanying article.

### Exploratory Analysis

The analysis includes:

- Dataset inspection and data-quality checks
- Descriptive statistics
- Attrition-rate summaries
- Distribution visualizations
- Boxplots for quantitative and ordinal characteristics
- Correlation analysis
- Multicollinearity assessment using Variance Inflation Factors (VIF)

### Univariate Analysis

For numerical and ordinal variables, differences between employees who left and employees who remained are evaluated using the **Mann-Whitney U test**.

Effect sizes are quantified using the **rank-biserial correlation**.

For categorical variables, associations with attrition are evaluated using the **Chi-square test of independence**.

Expected frequencies are examined to assess the assumptions of the Chi-square tests, and association strength is quantified using **Cramer's V**.

To account for multiple hypothesis testing, the **Benjamini-Hochberg False Discovery Rate (FDR) correction** is applied separately to the Mann-Whitney and Chi-square test families.

### Multivariable Analysis

A **binary logistic regression model** is used to evaluate which employee characteristics remain associated with attrition after adjustment for the other predictors.

Model results are evaluated using:

- Logistic regression coefficients
- Odds ratios (OR)
- 95% confidence intervals
- p-values

Generalized likelihood-ratio tests (GLRTs) are used to evaluate model specification, including the contribution of `EducationField` and selected interaction terms.

The following interactions are examined:

- `OverTime × MonthlyIncome`
- `DistanceFromHome × MaritalStatus`

Neither tested interaction significantly improved model fit, and the final model therefore uses an additive specification.

## Train, Validation, and Test Split

The dataset is divided using stratified random splitting into:

- **70% training data**
- **15% validation data**
- **15% test data**

The resulting sample sizes are:

- Training set: 1,029 observations
- Validation set: 220 observations
- Test set: 221 observations

A fixed random state of **42** is used to support reproducibility.

The classification threshold is selected using the **validation set only** and is subsequently applied unchanged to the held-out test set.

## Model Evaluation

Model performance is evaluated using:

- ROC-AUC
- Precision-Recall AUC (PR-AUC)
- Accuracy
- Balanced accuracy
- Precision
- Recall
- F1 score

The final model achieved approximately:

- Validation ROC-AUC: **0.770**
- Test ROC-AUC: **0.797**
- Test PR-AUC: **0.533**
- Test Precision (Attrition): **0.352**
- Test Recall (Attrition): **0.694**
- Test F1 (Attrition): **0.467**

## Main Findings

The analysis indicates that employee attrition is associated with multiple workplace and career-related characteristics.

In the final multivariable analysis, statistically significant associations were identified for characteristics including:

- OverTime
- BusinessTravel
- Selected JobRole categories
- StockOptionLevel
- YearsInCurrentRole
- DistanceFromHome
- NumCompaniesWorked

Some characteristics that showed significant univariate relationships, such as `MonthlyIncome`, did not remain statistically significant after simultaneous adjustment for the other predictors.

The results therefore demonstrate the importance of distinguishing between associations observed when variables are examined individually and associations that remain after adjustment in a multivariable model.

## Repository Contents

```text
Employee-Attrition-Analysis/
│
├── README.md
├── StatisticalTheoryFinalCode (1).ipynb
└── Employee_Attrition_Analysis.pdf
```

- [`StatisticalTheoryFinalCode (1).ipynb`](./StatisticalTheoryFinalCode%20%281%29.ipynb) — complete reproducible statistical analysis and model evaluation.
- [`Employee_Attrition_Analysis.pdf`](./Employee_Attrition_Analysis.pdf) — final research article.

## How to Run the Analysis

### Recommended Method: Google Colab

1. Download `StatisticalTheoryFinalCode (1).ipynb` from this repository.
2. Open Google Colab.
3. Select **File → Upload notebook**.
4. Upload `StatisticalTheoryFinalCode (1).ipynb`.
5. Connect to a Python runtime.
6. Run the notebook from beginning to end using **Runtime → Run all**.
7. Wait until all cells have finished executing.

The notebook should be executed sequentially from the first cell to the last because later stages of the analysis depend on objects created in earlier cells.

The dataset is downloaded automatically by the notebook using `kagglehub`, so an internet connection is required when the dataset is downloaded.

The final notebook was tested in a fresh Google Colab session and successfully executed from beginning to end without runtime errors.

## Python Libraries

The analysis uses the following Python libraries:

- `numpy`
- `pandas`
- `scipy`
- `statsmodels`
- `scikit-learn`
- `matplotlib`
- `seaborn`
- `kagglehub`

The required imports and dataset-loading commands are included directly in the notebook.

## Reproducibility

To reproduce the complete analysis:

1. Start with a fresh Google Colab session.
2. Upload and open the notebook.
3. Run all cells sequentially from beginning to end.
4. Do not modify the random state, data-splitting procedure, predictor definitions, or validation-selected classification threshold.
5. Compare the generated statistical results and model-performance metrics with those reported in the accompanying article.

The notebook defines:

```python
RANDOM_STATE = 42
ALPHA = 0.05
```

The fixed random state is used in the stratified train/validation/test splitting procedure to make the data partitions reproducible.

## Interpretation and Limitations

The dataset is observational. Therefore, the statistical relationships identified in this project should be interpreted as **associations rather than causal effects**.

The model was evaluated using an internal held-out test set. External validation using independent organizational data would be required before conclusions about predictive performance could be generalized to other employee populations.
