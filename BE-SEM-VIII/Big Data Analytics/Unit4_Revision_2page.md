# Unit 4 — Advanced Predictive Analytics (2-page revision)

Purpose: concise explanations of EDA workflow, multivariate analysis, ensemble methods, practical modeling tips and evaluation.

1. EDA & workflow recap
- Steps: Data sourcing → Data cleaning → Univariate analysis → Bivariate/multivariate analysis → Feature engineering → Model selection → Validation.
- Quick checks: `str()`, `summary()`, `pairs()`, correlation matrix, missing-value patterns.

2. Feature engineering essentials
- Impute missing values, create indicator flags for missingness, encode categoricals (one-hot / label encode), bin continuous variables, scale features when needed (`scale()`).
- Interaction terms: `I(x1*x2)` in formulas; polynomial terms: `poly(x,2)`.

3. Multivariate analysis (practical)
- Correlation heatmap to detect multicollinearity; if present consider PCA or drop variables.
- Use pairplots and partial dependence plots (for tree models) to interpret relationships.

4. Ensemble methods — what to memorize
- Bagging: bootstrap AGGregation — trains many base learners on bootstrap samples; reduces variance. Example: Bagging with decision trees.
- Random Forest: bagging + random feature selection at each split; robust, handles mixed data, less overfitting than single trees.
- Boosting: sequential models focusing on previous errors (AdaBoost, Gradient Boosting, XGBoost); reduces bias and can overfit if unchecked.
- AdaBoost: weight adjustment for misclassified examples; final model is weighted sum of weak learners.

5. Practical modeling tips
- Tree-based models: no need to scale numeric features; handle missing values (some implementations) and categorical splits.
- Distance-based models (KNN, k-means): must scale numeric features.
- Regularization: Lasso (L1) for feature selection; Ridge (L2) for multicollinearity.

6. Model evaluation & validation
- Metrics: classification — accuracy, precision, recall, F1, ROC-AUC; regression — RMSE, MAE, R-squared.
- Confusion matrix: interpret TP, FP, FN, TN; choose metric based on problem (precision vs recall tradeoff).
- Cross-validation: `cv` (k-fold, stratified) for robust performance estimates; use grid search for hyperparameters.

7. Overfitting & regularization
- Detect via training vs validation performance gap; fix with simpler model, more data, regularization, early stopping (boosting), or pruning (trees).

8. Quick code hints (Python/ R concepts)
- R: `lm()`, `glm()`, `kmeans()`, `caret` package for modeling and resampling.
- Python/sklearn equivalents: `RandomForestClassifier`, `GridSearchCV`, `KFold`, `AdaBoostClassifier`, `XGBClassifier`.

Exam focus: for 10-mark answers describe algorithm idea, strengths/weaknesses, when to use it, and include a tiny pseudocode or command snippet.
