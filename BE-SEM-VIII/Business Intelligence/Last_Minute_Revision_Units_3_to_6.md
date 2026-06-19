# BE SEM VIII Business Intelligence
## Last Minute Revision: Units III to VI

Use this as a fast recall sheet before the exam. It keeps each unit compact, high-yield, and focused on the most asked concepts.

---

# Unit III: Data Provisioning and Data Visualization

## Data Provisioning Core

**Data Warehouse:** Central repository for integrated, historical, analysis-oriented data. Key properties: subject-oriented, integrated, time-variant, non-volatile.

**Schemas:**
- Star schema: fact table in center with dimension tables around it; simple and fast.
- Snowflake schema: normalized dimensions; less redundancy but more joins.
- Galaxy schema: multiple fact tables sharing dimensions.

**Data Quality and Profiling:**
- Quality dimensions: accuracy, completeness, consistency, timeliness, validity, uniqueness.
- Profiling checks column patterns, nulls, dependencies, and anomalies before ETL.

**ETL:** Extract data from sources, transform it to warehouse format, then load it.
- Extraction: full or incremental.
- Transformation: cleaning, standardization, deduplication, aggregation, derivation.
- Loading: initial bulk load or incremental load.

**CDC (Change Data Capture):** Captures only inserts, updates, and deletes since last refresh. It reduces load time and supports near real-time provisioning.

**Lookups:** Used in ETL to validate keys, enrich data, map surrogate keys, and maintain referential integrity.

**Staging Area:** Temporary buffer between source and warehouse. It isolates source systems and supports cleansing, recovery, and audit.

**Data Marts:** Small subject-focused subsets of a warehouse. They improve speed, security, and department-level analysis.

**OLAP Cubes:** Multi-dimensional analysis structure with measures and dimensions. Operations: roll-up, drill-down, slice, dice, pivot.

## Data Duplication and Latency

- Uncontrolled duplication increases storage, slows queries, complicates ETL, and causes inconsistent reporting.
- Controlled redundancy can help read performance in star schemas.
- Time lag is the delay between source change and warehouse availability; CDC and micro-batching reduce it.

## Visualization and Reporting

**Business Report:** Formal presentation of data, analysis, and recommendations.

**Reporting System Components:** data source layer, report server, design tool, distribution, security, report types.

**Visualization Basics:**
- Bar chart: compare categories.
- Line chart: trends over time.
- Pie chart: simple composition.
- Scatter plot: relationship between variables.
- Histogram and box plot: distribution and outliers.

**Visual Analytics:** Combines interactive visuals with analytical computation for faster insight and decision-making.

**Performance Dashboards:** Single-screen KPI monitoring for operational, tactical, and strategic use.

**BPM (Business Performance Management):** Strategy, planning, scorecards, monitoring, analysis, and adjustment.

**Tools:** Tableau, Power BI, Dundas BI, Oracle BI, Excel.

## Fast Recall for Exam

| Topic | One-line Recall |
|---|---|
| ETL | Extract, transform, load data into warehouse |
| CDC | Capture only changed records |
| Data Mart | Department-specific warehouse subset |
| OLAP | Roll-up, drill-down, slice, dice, pivot |
| Dashboard | KPI screen for monitoring |
| Data Quality | Accurate, complete, consistent, timely data |

---

# Unit IV: Data Pre-processing Techniques

## Data Validation and Missing Data

**Data Validation:** Checks data before analysis using type, range, format, consistency, completeness, uniqueness, and referential integrity rules.

**Incomplete Data:** Missing values reduce accuracy and can bias outcomes.
- MCAR: missing completely at random.
- MAR: missing depends on observed data.
- MNAR: missing depends on unobserved data.

**Handling Missing Values:** delete, impute with mean/median/mode, regression imputation, KNN imputation, or flag separately.

## Noise

Noise is random error that hides the true signal.
- Affects numerical, categorical, text, and image/signal data.
- Leads to false patterns, unstable statistics, and poor model performance.
- Common fixes: binning, smoothing, regression, filtering, outlier handling.

## Data Transformation

**Standardization:**
- Z-score: (x - μ) / σ
- Min-max: (x - min) / (max - min)
- Use when features have different scales.

**Feature Extraction:** Creates new features from raw data.
- PCA, LDA, TF-IDF, embeddings, derived date features, ratios.

## Data Reduction

**Why reduce data?** Less storage, faster processing, lower noise, easier visualization.

**Sampling Methods:**
- Simple random: equal chance for every record.
- Stratified: preserve subgroup proportions.
- Systematic: every k-th record.
- Cluster: sample whole groups.
- Reservoir: useful for streams.

**Feature Selection:** Keeps the best original features.
- Filter: correlation, chi-square, information gain, variance threshold.
- Wrapper: forward selection, backward elimination, RFE.
- Embedded: LASSO, trees, random forest importance.

**PCA:** Converts correlated variables into orthogonal principal components that capture maximum variance. Use standardization first; choose components by scree plot or explained variance.

**Discretization:** Converts continuous values into bins.
- Equal-width: simple, but sensitive to outliers.
- Equal-frequency: balanced bins, but boundaries may look artificial.
- Entropy-based: best for classification because it uses class labels.
- Domain-driven: best interpretability.

## Data Exploration

EDA helps understand structure, patterns, anomalies, and data quality before modeling.

**Univariate:** one variable. Use histogram, box plot, mean, median, mode, SD, IQR, outlier rules.

**Bivariate:** two variables. Use scatter plot, correlation, chi-square, contingency table.

**Multivariate:** three or more variables. Use correlation matrix, heatmap, pair plot, PCA, multiple regression.

## Fast Recall for Exam

| Topic | One-line Recall |
|---|---|
| Missing Data | MCAR, MAR, MNAR |
| Noise | Random error; smooth or filter it |
| Standardization | Z-score and min-max scaling |
| PCA | Dimensionality reduction using variance |
| Sampling | Random, stratified, systematic, cluster, reservoir |
| Discretization | Convert numeric data into bins |
| EDA | Find patterns, outliers, and relationships |

---

# Unit V: Impact of Machine Learning in BI

## Regression

**Regression problem:** Predict a continuous value such as sales, price, demand, or CLV.

**Linear Regression:**
- Model: Y = β0 + β1X + ε
- Multiple regression adds more predictors.
- OLS minimizes squared error.

**Evaluation:**
- MAE: average absolute error.
- MSE: squares errors, punishes large misses.
- RMSE: square root of MSE, same unit as target.
- R²: variance explained by the model.

## Classification

**Classification problem:** Predict a class label such as spam/not spam, churn/stay, approve/reject.

**Confusion Matrix:** TP, TN, FP, FN.
- Accuracy: overall correctness.
- Precision: how many predicted positives are correct.
- Recall: how many actual positives are found.
- F1: balance of precision and recall.

**Bayesian Methods:** Use Bayes theorem.
- P(C|X) ∝ P(C) × P(X|C)
- Naive Bayes assumes feature independence.

**Logistic Regression:**
- Predicts class probability using sigmoid.
- P = 1 / (1 + e^-z)
- z = β0 + β1X1 + β2X2 + ...
- Best for binary classification and interpretable coefficients.

## Clustering

**Clustering:** Unsupervised grouping of similar records.

**K-Means:** Assign points to nearest centroid, recompute centroids, repeat until stable.
- Simple and fast.
- Needs K in advance.
- Sensitive to outliers and initial centroids.

**Hierarchical Clustering:** Builds a dendrogram.
- Agglomerative: bottom-up merge.
- Divisive: top-down split.
- Linkage types: single, complete, average, Ward.

**Clustering Evaluation:**
- Silhouette coefficient: compares cohesion and separation.
- WSS/Inertia: lower is better.
- Dunn index: higher is better.

## Association Rules

**Rule form:** IF X THEN Y.

**Measures:**
- Support: frequency of X and Y together.
- Confidence: P(Y|X).
- Lift: confidence divided by support(Y).

**Apriori Algorithm:**
- Uses the principle that all subsets of a frequent itemset must be frequent.
- Bottom-up level-wise search.
- Join, prune, count support, repeat.

## Fast Recall for Exam

| Topic | One-line Recall |
|---|---|
| Regression | Predict continuous values |
| R² | Proportion of variance explained |
| Confusion Matrix | TP, TN, FP, FN |
| Logistic Regression | Sigmoid probability model |
| K-Means | Nearest centroid partitioning |
| Hierarchical | Agglomerative or divisive tree |
| Support | Frequency of itemset |
| Confidence | Conditional probability |
| Lift | Strength of association |
| Apriori | Frequent subsets imply frequent supersets |

---

# Unit VI: BI Applications, Emerging Trends and Future Impacts

## BI Applications by Sector

**Higher Education:** student performance analytics, enrollment forecasting, curriculum planning, research analytics, placements, alumni analysis.

**Healthcare:** patient monitoring dashboards, clinical decision support, disease surveillance, resource planning, cost analysis, compliance tracking.

**Logistics and Supply Chain:** demand forecasting, inventory optimization, route planning, supplier scorecards, warehouse analytics.

**CRM:** segmentation, customer lifetime value, churn prediction, cross-selling, campaign analysis, 360-degree customer view.

**Banking:** credit scoring, fraud detection, AML, customer profitability, branch performance, regulatory reporting.

**Telecom:** network monitoring, churn analysis, revenue assurance, capacity planning, campaign targeting, service quality.

**Manufacturing:** predictive maintenance, quality control, production optimization, supply chain visibility, energy management, workforce analytics.

## Emerging Trends

**Location-Based Analytics:** uses GIS, GPS, and spatial data for site selection, geo-marketing, route optimization, and asset tracking.

**Mobile BI:** dashboards on phones and tablets with responsive design, touch interaction, alerts, offline access, and location awareness.

**Web 2.0 and Social BI:** integrates social media data and collaboration features like comments, sharing, @mentions, sentiment analysis, and community insights.

**Cloud BI:** BI services delivered via cloud infrastructure.
- Benefits: lower cost, scalability, fast deployment, accessibility, managed updates, advanced analytics.
- Challenges: security, privacy, latency, vendor lock-in, hidden costs, internet dependency, governance.

## Issues Related to Analytics

- Data quality problems: missing, inconsistent, duplicate, inaccurate data.
- Privacy and ethics: consent, bias, regulatory compliance.
- Skills gap: shortage of data and BI talent.
- Data silos: fragmented data and multiple versions of truth.
- Scalability: data growth, performance, legacy limits.
- Change management: user resistance and low adoption.
- Real-time analytics: higher infrastructure complexity.
- Security: breaches, insider threats, ransomware.
- ROI measurement: difficult to quantify benefits directly.
- Tool proliferation: too many disconnected BI tools.

## Fast Recall for Exam

| Topic | One-line Recall |
|---|---|
| Higher Education BI | Student and placement analytics |
| Healthcare BI | Monitoring, diagnosis support, surveillance |
| CRM + BI | Segmentation, CLV, churn, cross-sell |
| Manufacturing BI | Predictive maintenance and quality control |
| Location Analytics | GIS-based decision support |
| Mobile BI | Anywhere, anytime BI access |
| Social BI | Web 2.0 data + collaboration |
| Cloud BI | Scalable, low-cost, but security-sensitive |
| Analytics Issues | Quality, privacy, skills, silos, governance |

---

# Ultra-Fast Final Recall

| Unit | Must-Remember Keywords |
|---|---|
| III | Warehouse, ETL, CDC, staging, data mart, OLAP, dashboard |
| IV | Missing data, noise, scaling, PCA, sampling, EDA, discretization |
| V | Regression, classification, clustering, support-confidence-lift, Apriori |
| VI | Sector BI uses, mobile BI, social BI, cloud BI, analytics issues |
