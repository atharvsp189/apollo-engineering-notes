# Unit 3 — Predictive Analysis Process & R (2-page revision)

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

Exam tips (write 10-mark answers): define, list steps, show commands, and provide a short code snippet or output interpretation.
