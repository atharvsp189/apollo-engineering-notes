# Last Minute Combined Revision — Big Data Analytics (Units 3–6)

Fast consolidated revision for Units 3, 4, 5 and 6. Use this single document for last-minute study: core commands, formulas, workflows, exam cues and short examples.

---
**Unit 3 — Predictive Analysis Process & R**

Purpose: focused, exam-oriented explanation of core R commands, data cleaning, EDA, regression, clustering, and hypothesis testing.

1. R Essentials (quick commands)
- Read/write: `read.csv("file.csv")`, `read.table()`, `read_excel("file.xlsx")` (readxl), `fread()` (data.table for very large CSVs).
- Save/load: `write.csv(df, "out.csv", row.names=FALSE)`, `saveRDS(obj, "obj.rds")`, `readRDS("obj.rds")`.
- Inspect: `str(df)`, `head(df)`, `summary(df)`, `dim(df)`, `names(df)`, `table(df$col)`.

2. Data types & conversions
- Core types: numeric, integer, character, logical, factor (ordered/nominal), Date.
- Convert: `as.numeric()`, `as.integer()`, `as.character()`, `as.factor()`, `as.Date(x, "%d/%m/%Y")`.

3. Dirty data detection & fixes
- Detect: `colSums(is.na(df))`, `sum(is.na(df))`, `which(duplicated(df))`, `boxplot(df$col)` (outliers), `scale()` for z-scores.
- Handle missing: drop (`na.omit(df)`), impute mean/median (`df$col[is.na(df$col)] <- median(df$col, na.rm=TRUE)`), or model-based imputation.
- Duplicates: `df <- df[!duplicated(df), ]`.
- Standardize categories: `df$gender <- tolower(df$gender); df$gender <- gsub("^m$", "male", df$gender)`.

4. EDA — what to run fast
- Univariate: `hist(df$col)`, `boxplot(df$col)`, `table(df$cat)`.
- Bivariate: `plot(x,y)`, `cor(df$var1, df$var2)`, `boxplot(value ~ group, data=df)`.
- Multivariate: `pairs(df[c("x","y","z")])`, `heatmap(cor(df_numeric))`.

5. Regression (linear)
- Fit: `model <- lm(y ~ x1 + x2, data=df)`.
- Check: `summary(model)` → coefficients, `R-squared`, `Adj R2`, `p-values`.
- Diagnostics: `plot(model)` (residuals, leverage), check multicollinearity with `vif()` (car package).
- Predict: `predict(model, newdata=data.frame(x1=..., x2=...))`.

6. Clustering (practical)
- K-means: scale numeric features `X <- scale(df[c(...)]); km <- kmeans(X, centers=3, nstart=25)`.
- Choose k: elbow (plot within-cluster sum of squares vs k) and silhouette score.
- Hierarchical: `d <- dist(X); hc <- hclust(d, method="ward.D2")`; plot dendrogram and `cutree(hc, k)`.

7. Hypothesis testing quick reference
- One-sample t-test: `t.test(x, mu=val)`.
- Two-sample (independent): `t.test(x, y)`.
- Paired: `t.test(before, after, paired=TRUE)`.
- Chi-square for categorical association: `chisq.test(table(a,b))`.
- ANOVA: `aov(y ~ group, data=df)`; check `TukeyHSD()` for post-hoc.

8. MapReduce (concept)
- Map: parallel record-level mapping; Shuffle/Sort: group by key; Reduce: aggregate results. Use for large-scale counting, cleaning, and summarization.

Exam tips: define, list steps, show commands, and provide a short code snippet or output interpretation.

---
**Unit 4 — Advanced Predictive Analytics**

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
- Bagging: bootstrap AGGregation — trains many base learners on bootstrap samples; reduces variance.
- Random Forest: bagging + random feature selection at each split; robust, handles mixed data.
- Boosting: sequential models focusing on previous errors (AdaBoost, Gradient Boosting, XGBoost); reduces bias.
- AdaBoost: weight adjustment for misclassified examples; final model is weighted sum of weak learners.

5. Practical modeling tips
- Tree-based models: no need to scale numeric features; handle missing values (some implementations).
- Distance-based models (KNN, k-means): must scale numeric features.
- Regularization: Lasso (L1) for feature selection; Ridge (L2) for multicollinearity.

6. Model evaluation & validation
- Metrics: classification — accuracy, precision, recall, F1, ROC-AUC; regression — RMSE, MAE, R-squared.
- Confusion matrix: interpret TP, FP, FN, TN; choose metric based on problem (precision vs recall tradeoff).
- Cross-validation: k-fold, stratified; use grid search for hyperparameters.

7. Overfitting & regularization
- Detect via training vs validation performance gap; fix with simpler model, more data, regularization, early stopping, or pruning.

8. Quick code hints
- R: `lm()`, `glm()`, `kmeans()`, `caret` package for modeling and resampling.
- Python/sklearn equivalents: `RandomForestClassifier`, `GridSearchCV`, `KFold`, `AdaBoostClassifier`, `XGBClassifier`.

Exam focus: describe algorithm idea, strengths/weaknesses, when to use it, and include a tiny pseudocode or command snippet.

---
**Unit 5 — Big Data Visualization**

Purpose: cover challenges, key techniques, tool highlights, chart selection guidance, and quick how-tos for Tableau and Google Charts.

1. Challenges & how to mitigate
- Challenges: volume, velocity, variety, visual clutter, scalability, data quality, and human perception limits.
- Mitigations: aggregation (group by time windows), sampling (random/stratified), incremental rendering, progressive disclosure, dimensionality reduction (PCA/t-SNE), and interactive filtering.

2. Choosing the right visualization
- Temporal data: line charts, area charts, sparklines; use aggregation for large series.
- Comparison: bar/column charts, grouped bars; normalize when scales differ.
- Distribution: histograms, box plots, violin plots; show outliers separately when needed.
- Relationships: scatter plots, bubble charts, correlation heatmaps; add regression line when helpful.
- Hierarchical & part-to-whole: treemap, sunburst; avoid too many leaf nodes.
- Geospatial: choropleth for density, symbol maps for precise locations; use map tiling for performance.

3. Tableau quick commands & features
- Workflow: Connect Data -> Prepare (clean/blend) -> Create Worksheets -> Build Dashboard -> Publish.
- Key features: calculated fields, parameters, LOD expressions, filters/actions, show/hide containers, map layers, forecasting, clustering.

4. Google Charts — quick how-to
- Steps: load library via `https://www.gstatic.com/charts/loader.js`, prepare `DataTable`, set `options`, create chart object (e.g., `new google.visualization.LineChart(...)`) and `draw()`.

5. Dashboard design principles
- Keep top-level summary visible, place most important KPI top-left, use filters for interactivity, minimize color palette, use tooltips for details.

6. Quick exam answers (structure)
- Define the concept, list challenges, mitigation techniques, and give a short example or tool mapping for marks.

---
**Unit 6 — Applications & Tools**

Purpose: concise review of major application domains (retail, finance, healthcare, supply chain) and tool summaries (Semantria, sentiment tools, HBase) with exam-focused mappings.

1. Application domains — core use-cases & quick tech mapping
- Retail: customer segmentation, recommendation systems, demand forecasting, price optimization, inventory management, market-basket analysis, churn prediction, fraud detection. Tech: Hadoop/Spark, Kafka, ML libraries, graph analysis.
- Finance: credit scoring, fraud detection (real-time), market risk analysis, algorithmic trading, AML/KYC, regulatory reporting. Tech: Kafka, Spark, Neo4j, NLP.
- Healthcare: predictive diagnostics, EHR analytics, genomics pipelines, medical imaging (deep learning), wearable monitoring. Tech: HDFS, Spark, TensorFlow/PyTorch.
- Supply Chain: demand forecasting, route optimization, inventory optimization, supplier risk scoring, real-time tracking. Tech: IoT, GPS/RFID, streaming analytics.

2. Semantria (text analytics) — quick summary
- Function: cloud-based text analytics and NLP for unstructured data.
- Features: sentiment scoring, entity extraction, theme detection, categorization, summarization, intent/aspect detection, multi-language support, REST API.

3. AS sentiment tools — approaches & caveats
- Lexicon-based vs ML-based vs hybrid; watch for sarcasm, domain vocabulary, and multilingual challenges.

4. Apache HBase — architecture & when to use
- Column-family NoSQL store built on HDFS; components: HBase Master, RegionServers, ZooKeeper, HFiles, MemStore, WAL.
- Strengths: low-latency random read/write, sparse wide tables, versioning, horizontal scalability. Tradeoffs: not for large full-table analytics, operational complexity.

5. Exam hints: map tool → use-case
- Streaming fraud detection → Kafka + Spark Streaming + ML model.
- Text analytics → Semantria or NLP pipeline + dashboard.
- Real-time key-value retrieval for user profiles → HBase.

6. Short examples to memorize
- Market-basket: Apriori → frequent itemsets → association rules (support, confidence, lift).
- Fraud detection: anomaly scoring per transaction + graph link analysis.

---
**High-priority quick revision checklist (30–60 minutes)**
- Memorize core R commands (`read.csv`, `fread`, `lm`, `kmeans`, `t.test`).
- Know ensemble differences: bagging vs boosting vs random forest vs AdaBoost (one-line summary each).
- State 3–4 applications each for Retail, Finance, Healthcare, Supply Chain with key tech.
- For visualization, list 4 techniques and when to use them; mention Tableau features and Google Charts workflow.
- For HBase, be able to sketch architecture and state 3 strengths (random access, scalability, versioning).

Good luck — use this single file to review and then practice writing short answers for the PYQs.
