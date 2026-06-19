# Last Minute Revision — Big Data Analytics (Units 3–6)

Quick, high-yield notes for rapid revision before exams. Read each unit's bullets; focus on the formulas, commands, and exam question cues.

---
**Unit 3 — Predictive Analysis Process & R (Quick Notes)**
- R essentials: `read.csv()`, `read_excel()` (`readxl`), `fread()` (data.table), `write.csv()`, `saveRDS()`/`readRDS()`.
- Data types: numeric, integer, character, logical, factor (ordered vs nominal).
- Dirty data detection: `summary()`, `str()`, `colSums(is.na())`, `duplicated()`, IQR and Z-score for outliers.
- Cleaning quick fixes: `na.omit()`, `df[!duplicated(df),]`, `as.Date()`, `tolower()/gsub()` for category standardization, winsorize via IQR caps.
- EDA commands: `head()`, `tail()`, `dim()`, `table()`, `cor()`, `aggregate()`.
- Linear regression: `model <- lm(y ~ x1 + x2, data=df)`; check `summary(model)` for R-squared, p-values; predict with `predict()`.
- Clustering: scale data (`scale()`), `kmeans(..., centers=k, nstart=25)`, elbow method (plot tot.withinss vs k); hierarchical: `hclust(dist(...), method="ward.D2")`.
- Hypothesis tests: `t.test()`, `chisq.test()`, `aov()` for ANOVA, `cor.test()`.
- MapReduce idea: Map → Shuffle/Sort → Reduce; used for parallel cleaning/aggregation on large data.

---
**Unit 4 — Advanced Predictive Analytics (Quick Notes)**
- EDA workflow: Data Sourcing → Cleaning → Univariate → Bivariate/Multivariate → Feature Engineering → Modeling.
- Univariate checks: histogram, boxplot, skewness, kurtosis.
- Bivariate: scatter + correlation for continuous; boxplots/ANOVA for continuous vs categorical; chi-square for categorical vs categorical.
- Ensemble methods:
  - Bagging: bootstrap samples, average/vote; reduces variance (e.g., BaggingClassifier).
  - Random Forest: bagging + random feature selection at splits; strong for tabular data.
  - Boosting: sequential learners focusing on previous errors (AdaBoost, Gradient Boosting, XGBoost); reduces bias.
  - AdaBoost: weight update, alpha for classifier weight; final prediction weighted sum of weak learners.
- Practical tips: scale features when methods require distance (k-means, KNN); do not scale tree-based models necessarily.
- Model evaluation: confusion matrix, accuracy, precision, recall, F1, ROC-AUC; use cross-validation for robust estimates.

---
**Unit 5 — Big Data Visualization (Quick Notes)**
- Challenges: volume, velocity, variety, visual clutter, scalability, data quality, real-time needs.
- Mitigations: aggregation, sampling, filtering, drill-down, dimensionality reduction (PCA/t-SNE), interactive dashboards.
- Tableau highlights: drag-and-drop, live connections, calculated fields, parameters, LOD expressions, forecasting, maps.
- Google Charts: load library, prepare DataTable, set `options`, draw chart. Good for quick web visualizations.
- Visualization types (pick by goal):
  - Temporal: line, area, sparkline
  - Hierarchical: treemap, sunburst
  - Network: node-link, force-directed
  - Multidimensional: scatter matrix, parallel coordinates
  - Geospatial: choropleth, symbol maps
  - Distribution: histogram, box plot, violin
  - Text: word cloud, topic map
- Exam focus: tradeoffs (detail vs performance), when to aggregate/sample, which chart fits which data/problem.

---
**Unit 6 — Applications & Tools (Quick Notes)**
- Retail: customer segmentation, recommendation engines, demand forecasting, market-basket (Apriori), churn prediction, fraud detection. Tech: Hadoop, Spark, Kafka, NLP.
- Finance: credit risk scoring, fraud detection (streaming), HFT & sentiment signals, RegTech (AML, KYC), graph DBs for fraud networks.
- Healthcare: predictive diagnostics, EHR analytics, genomics, medical imaging (deep learning), wearable data streaming.
- Supply Chain: demand forecasting, route optimization, inventory optimization, IoT sensors, digital twins.
- Tools covered briefly:
  - Semantria (text analytics): multi-source ingestion, sentiment, entity extraction, aspect-based sentiment, summarization, REST API.
  - AS sentiment tools: lexicon vs ML vs hybrid approaches; aspect-level sentiment; sarcasm detection (advanced).
  - Apache HBase: column-family store on HDFS, low-latency random access, RegionServers + HBase Master + ZooKeeper, MemStore, HFile, WAL; use when wide, sparse tables and real-time reads are needed.
- Exam cues: map tools to use-cases (which tech for streaming vs batch, why HBase vs HDFS), list benefits and challenges.

---
**High-priority quick revision checklist (do these in last 30–60 minutes):**
- Memorize core commands for R (`read.csv`, `fread`, `lm`, `kmeans`, `t.test`).
- Know ensemble differences: bagging vs boosting vs random forest vs AdaBoost (one-line summary each).
- Be able to state 3–4 applications each for Retail, Finance, Healthcare, Supply Chain with the key technology used.
- For visualization, list 4 techniques and when to use them; mention Tableau features and Google Charts basic workflow.
- For HBase, be able to sketch architecture (Master, RegionServers, HDFS, ZooKeeper) and state 3 strengths (random access, scalability, versioning).

---
Good luck — review this file once, then practice sketching diagrams and writing quick answers for the PYQs listed in each unit.

---
Detailed 2-page revision notes (expanded):

- Unit 3: [Unit3_Revision_2page.md](Unit3_Revision_2page.md)
- Unit 4: [Unit4_Revision_2page.md](Unit4_Revision_2page.md)
- Unit 5: [Unit5_Revision_2page.md](Unit5_Revision_2page.md)
- Unit 6: [Unit6_Revision_2page.md](Unit6_Revision_2page.md)
