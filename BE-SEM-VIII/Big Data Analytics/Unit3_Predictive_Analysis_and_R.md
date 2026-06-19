# Unit III: Predictive Analysis Process and R

---

## Section 1: Introduction to R & Data Import/Export + Dirty Data & EDA

### PYQs Covered:
- **Q1a)** What are the primary methods and functions for importing and exporting data in R and how can they be utilized effectively? [8 Marks]
- **Q1b)** How can data analysts detect and address dirty data using visualizations and statistical techniques in R? [10 Marks]
- **Q2b)** What are the different attribute types in data analysis, and how are they categorized? How does R handle various data types? [10 Marks]

---

### 1.1 Introduction to R

R is a programming language and software environment for statistical computing, data analysis, and graphical representation. It is open-source and widely used in academia and industry.

**Features of R:**
- Open source and free
- Extensive statistical and graphical capabilities
- Supports procedural, functional, and object-oriented programming
- Large community with 18,000+ packages on CRAN
- Platform independent (Windows, Linux, macOS)

**R Graphical User Interfaces (GUIs):**

| GUI | Description |
|-----|-------------|
| RStudio | Most popular IDE; includes editor, console, environment, and plot panes |
| R Commander (Rcmdr) | Menu-driven GUI for basic statistical analysis |
| RGui | Default Windows interface |
| Jupyter Notebook | Web-based interactive computing with R kernel |
| Deducer | Java-based GUI for R |

**RStudio Panes:**
1. **Source Editor** – Write and edit R scripts
2. **Console** – Execute R commands interactively
3. **Environment/History** – View variables, data objects, command history
4. **Files/Plots/Packages/Help** – File management, visualizations, package management

---

### 1.2 Data Types in R (Covers Q2b)

**Basic Data Types:**

| Type | Description | Example |
|------|-------------|---------|
| Numeric | Real numbers (default for numbers) | `x <- 3.14` |
| Integer | Whole numbers (suffix L) | `x <- 5L` |
| Character | Text/strings | `x <- "hello"` |
| Logical | Boolean values | `x <- TRUE` |
| Complex | Complex numbers | `x <- 3+2i` |
| Factor | Categorical data with levels | `x <- factor(c("M","F","M"))` |

**Data Structures in R:**

```r
# Vector - 1D collection of same type
marks <- c(85, 90, 78, 92)

# Matrix - 2D collection of same type
mat <- matrix(1:9, nrow=3, ncol=3)

# Data Frame - 2D collection of different types (like a table)
df <- data.frame(name=c("A","B"), marks=c(85,90))

# List - Collection of different types and sizes
mylist <- list(name="Atharv", marks=c(85,90), passed=TRUE)

# Factor - Categorical variable
gender <- factor(c("Male", "Female", "Male"))
```

**Attribute Types in Data Analysis:**

1. **Nominal** – Categories with no order (e.g., color, gender)
2. **Ordinal** – Categories with meaningful order (e.g., rating: low, medium, high)
3. **Interval** – Numeric with equal intervals, no true zero (e.g., temperature in °C)
4. **Ratio** – Numeric with true zero (e.g., weight, height, income)

**How R Handles These:**
- Nominal → `factor()` without ordering
- Ordinal → `factor(ordered=TRUE, levels=c("low","medium","high"))`
- Interval/Ratio → `numeric` or `integer`
- Character data → `character` type
- Boolean → `logical` type

```r
# Checking data types
class(x)       # Returns class of object
typeof(x)      # Returns internal type
is.numeric(x)  # Returns TRUE/FALSE
str(df)        # Structure of data frame showing all types
```

---

### 1.3 Data Import and Export in R (Covers Q1a)

**Importing Data:**

| Function | File Type | Package |
|----------|-----------|---------|
| `read.csv()` | CSV files | base R |
| `read.table()` | Tab/space delimited | base R |
| `read.xlsx()` | Excel files | openxlsx / readxl |
| `read_json()` | JSON files | jsonlite |
| `dbConnect()` + `dbReadTable()` | Databases | DBI, RMySQL |
| `read.spss()` | SPSS files | foreign |
| `fread()` | Large CSV (fast) | data.table |

**Examples:**

```r
# CSV Import
data <- read.csv("student_data.csv", header=TRUE, sep=",")

# With options
data <- read.csv("file.csv", 
                 na.strings=c("", "NA", "N/A"),  # Handle missing values
                 stringsAsFactors=FALSE)          # Don't auto-convert strings

# Excel Import
library(readxl)
data <- read_excel("data.xlsx", sheet=1)

# Tab-delimited
data <- read.table("data.txt", header=TRUE, sep="\t")

# From URL
data <- read.csv("https://example.com/data.csv")

# Database connection
library(DBI)
con <- dbConnect(RMySQL::MySQL(), dbname="mydb", host="localhost")
data <- dbReadTable(con, "students")
```

**Exporting Data:**

| Function | File Type |
|----------|-----------|
| `write.csv()` | CSV file |
| `write.table()` | Text file |
| `write.xlsx()` | Excel file |
| `saveRDS()` | R binary object |
| `save()` | Multiple R objects |

```r
# Export to CSV
write.csv(data, "output.csv", row.names=FALSE)

# Export to Excel
library(openxlsx)
write.xlsx(data, "output.xlsx")

# Save R object (preserves data types)
saveRDS(model, "my_model.rds")
loaded_model <- readRDS("my_model.rds")
```

**Effective Utilization Tips:**
1. Always set `stringsAsFactors=FALSE` to avoid unwanted conversions
2. Use `na.strings` to handle various missing value representations
3. Use `fread()` from data.table for large files (10x faster)
4. Use `readRDS/saveRDS` to preserve R-specific data structures
5. Always check imported data with `str()`, `head()`, `summary()`

---

### 1.4 Dirty Data – Detection and Treatment (Covers Q1b)

**What is Dirty Data?**
Dirty data refers to data that is inaccurate, incomplete, inconsistent, duplicated, or improperly formatted, leading to unreliable analysis.

**Types of Dirty Data:**

| Type | Example |
|------|---------|
| Missing values | NA, blank cells |
| Duplicates | Same record appearing multiple times |
| Outliers | Extreme values (e.g., age = 999) |
| Inconsistencies | "Male", "M", "male" for same category |
| Invalid data | Negative age, future dates |
| Formatting errors | Mixed date formats |

**Detection Using Visualizations:**

```r
# 1. Box Plot - Detect Outliers
boxplot(data$salary, main="Salary Distribution")
# Points beyond whiskers are potential outliers

# 2. Histogram - Distribution and anomalies
hist(data$age, breaks=20, main="Age Distribution")

# 3. Scatter Plot - Detect unusual relationships
plot(data$height, data$weight, main="Height vs Weight")

# 4. Missing Value Visualization
library(VIM)
aggr(data, col=c('blue','red'), numbers=TRUE)

# 5. Heatmap of correlations (detect data entry errors)
library(corrplot)
corrplot(cor(data), method="circle")
```

**Detection Using Statistical Techniques:**

```r
# 1. Summary Statistics
summary(data)  # Shows min, max, quartiles, NA count

# 2. Identify Missing Values
sum(is.na(data))              # Total NAs
colSums(is.na(data))          # NAs per column
complete.cases(data)           # Rows without any NA

# 3. Detect Outliers using IQR method
Q1 <- quantile(data$col, 0.25)
Q3 <- quantile(data$col, 0.75)
IQR_val <- Q3 - Q1
outliers <- data$col[data$col < (Q1 - 1.5*IQR_val) | 
                     data$col > (Q3 + 1.5*IQR_val)]

# 4. Z-score method for outliers
z_scores <- scale(data$col)
outliers <- which(abs(z_scores) > 3)

# 5. Duplicate Detection
duplicated(data)
sum(duplicated(data))
```

**Addressing Dirty Data:**

```r
# 1. Handle Missing Values
data$col[is.na(data$col)] <- mean(data$col, na.rm=TRUE)  # Mean imputation
data$col[is.na(data$col)] <- median(data$col, na.rm=TRUE) # Median imputation
data_clean <- na.omit(data)  # Remove rows with NA

# 2. Remove Duplicates
data_clean <- data[!duplicated(data), ]
# OR
data_clean <- unique(data)

# 3. Handle Outliers
# Cap at 1.5*IQR boundaries (Winsorization)
data$col[data$col > upper_bound] <- upper_bound
data$col[data$col < lower_bound] <- lower_bound

# 4. Standardize Categories
data$gender <- tolower(data$gender)
data$gender <- gsub("m", "male", data$gender)

# 5. Type Conversion
data$date <- as.Date(data$date, format="%d/%m/%Y")
data$age <- as.numeric(data$age)
```

**Implications of Anomalies on Decision-Making:**

1. **Biased Results** – Outliers can skew mean, regression coefficients
2. **Incorrect Predictions** – Models trained on dirty data give wrong outputs
3. **Misleading Visualizations** – Charts may hide real patterns
4. **Poor Business Decisions** – Incorrect analysis leads to financial losses
5. **Reduced Model Accuracy** – Noise reduces predictive power
6. **Compliance Issues** – Incorrect data may violate regulations

**Diagram: Dirty Data Impact Flow**
```
Dirty Data → Biased Statistics → Wrong Models → Poor Decisions → Business Loss
     ↓
  Detection (Visual + Statistical)
     ↓
  Cleaning (Imputation, Removal, Transformation)
     ↓
  Clean Data → Accurate Analysis → Better Decisions
```

---

### 1.5 Data Analysis in R

**Exploratory Data Analysis (EDA):**

```r
# Basic EDA Commands
head(data, 10)        # First 10 rows
tail(data, 5)         # Last 5 rows
dim(data)             # Dimensions (rows x columns)
str(data)             # Structure
summary(data)         # Statistical summary
names(data)           # Column names
table(data$category)  # Frequency table

# Descriptive Statistics
mean(data$col)
median(data$col)
sd(data$col)          # Standard deviation
var(data$col)         # Variance
cor(data$x, data$y)   # Correlation

# Data Manipulation
subset(data, age > 25)
aggregate(marks ~ class, data=data, FUN=mean)
```

---

## Section 2: Linear Regression, Clustering & Hypothesis Testing

---

### 2.1 Linear Regression with R

**Simple Linear Regression:**
- Finds relationship between one independent variable (X) and one dependent variable (Y)
- Equation: **Y = β₀ + β₁X + ε**
  - β₀ = intercept, β₁ = slope, ε = error term

```r
# Build model
model <- lm(marks ~ hours_studied, data=student_data)

# View results
summary(model)
# Key outputs: Coefficients, R-squared, p-value

# Prediction
new_data <- data.frame(hours_studied = 8)
predict(model, new_data)

# Visualization
plot(student_data$hours_studied, student_data$marks)
abline(model, col="red")
```

**Multiple Linear Regression:**
- Multiple independent variables: **Y = β₀ + β₁X₁ + β₂X₂ + ... + ε**

```r
model <- lm(marks ~ hours_studied + attendance + assignments, data=data)
summary(model)
```

**Interpreting Output:**
- **R-squared**: Proportion of variance explained (0 to 1, higher = better)
- **p-value**: If < 0.05, relationship is statistically significant
- **Coefficients**: Change in Y for unit change in X

---

### 2.2 Clustering with R

**Clustering** is an unsupervised learning technique that groups similar data points together.

**K-Means Clustering:**

```r
# Prepare data (scale for equal weight)
data_scaled <- scale(data[, c("col1", "col2")])

# Apply K-Means
set.seed(42)
km_result <- kmeans(data_scaled, centers=3, nstart=25)

# Results
km_result$cluster     # Cluster assignments
km_result$centers     # Cluster centroids
km_result$size        # Size of each cluster

# Visualization
plot(data_scaled, col=km_result$cluster, pch=20)
points(km_result$centers, col=1:3, pch=8, cex=2)
```

**Choosing Optimal K (Elbow Method):**

```r
wss <- sapply(1:10, function(k){
  kmeans(data_scaled, k, nstart=25)$tot.withinss
})
plot(1:10, wss, type="b", xlab="Number of Clusters K",
     ylab="Within-cluster Sum of Squares")
# Choose K at the "elbow" point
```

**Hierarchical Clustering:**

```r
# Distance matrix
dist_matrix <- dist(data_scaled, method="euclidean")

# Hierarchical clustering
hc <- hclust(dist_matrix, method="ward.D2")

# Dendrogram
plot(hc)
rect.hclust(hc, k=3)  # Cut into 3 clusters

# Get cluster assignments
clusters <- cutree(hc, k=3)
```

---

### 2.3 Hypothesis Testing in R

**Concept:** Hypothesis testing determines whether there is enough statistical evidence to support a claim about a population parameter.

- **H₀ (Null Hypothesis)**: No effect / no difference
- **H₁ (Alternative Hypothesis)**: There is an effect / difference
- **Significance Level (α)**: Usually 0.05
- **Decision Rule**: If p-value < α → Reject H₀

**Common Tests in R:**

```r
# 1. One-sample t-test (Is mean different from a value?)
t.test(data$marks, mu=60)

# 2. Two-sample t-test (Are two group means different?)
t.test(group_A$marks, group_B$marks)

# 3. Paired t-test (Before vs After on same subjects)
t.test(before, after, paired=TRUE)

# 4. Chi-square test (Association between categorical variables)
chisq.test(table(data$gender, data$passed))

# 5. ANOVA (Compare means of 3+ groups)
model <- aov(marks ~ class, data=data)
summary(model)

# 6. Correlation test
cor.test(data$x, data$y)
```

**Interpreting Results:**
- p-value < 0.05 → Reject H₀ (significant result)
- p-value ≥ 0.05 → Fail to reject H₀ (not significant)

---

## Section 3: Data Cleaning & Validation Tools – MapReduce

---

### 3.1 MapReduce

**MapReduce** is a programming model for processing large datasets in parallel across a distributed cluster.

**Two Phases:**

```
Input Data → SPLIT → MAP → SHUFFLE & SORT → REDUCE → Output
```

**Map Phase:**
- Takes input as key-value pairs
- Processes each record independently
- Outputs intermediate key-value pairs

**Reduce Phase:**
- Takes intermediate key-value pairs (grouped by key)
- Aggregates/summarizes values for each key
- Outputs final results

**Example: Word Count**
```
Input: "big data is big"

MAP:
  (big, 1), (data, 1), (is, 1), (big, 1)

SHUFFLE & SORT:
  big → [1, 1]
  data → [1]
  is → [1]

REDUCE:
  (big, 2), (data, 1), (is, 1)
```

**MapReduce for Data Cleaning:**
- **Map**: Identify dirty records (missing values, duplicates, format errors)
- **Reduce**: Aggregate cleaning actions, apply corrections, validate

**Advantages:**
1. Parallel processing of massive datasets
2. Fault tolerance (automatic re-execution on failure)
3. Scalability (add more nodes)
4. Data locality (process data where it resides)

**MapReduce in R (using rmr2 package):**
```r
library(rmr2)

# Map function
mapper <- function(key, value) {
  # Clean and validate each record
  keyval(value$category, value$amount)
}

# Reduce function
reducer <- function(key, values) {
  keyval(key, sum(values))
}

# Execute
result <- mapreduce(input="/data/sales",
                    map=mapper, reduce=reducer)
```

---

## Section 4: Data Analytics Lifecycle (Covers Q2a)

### PYQ Covered:
- **Q2a)** With the help of neat diagram explain the phases of Data Analytics life cycle. [8 Marks]

---

### 4.1 Data Analytics Lifecycle Phases

**Diagram:**

```
┌─────────────────────────────────────────────────────────────┐
│                  DATA ANALYTICS LIFECYCLE                   │
│                                                             │
│    ┌──────────┐    ┌──────────────┐    ┌─────────────┐      │
│    │   1.     │    │     2.       │    │    3.       │      │
│    │Discovery │───▶│    Data      │───▶│   Model     │      │
│    │          │    │ Preparation  │    │  Planning   │      │
│    └──────────┘    └──────────────┘    └─────────────┘      │
│         ▲                                     │             │
│         │                                     ▼             │
│    ┌──────────┐    ┌──────────────┐    ┌─────────────┐      │
│    │   6.     │    │     5.       │    │    4.       │      │
│    │Operatio- │◀───│ Communicate  │◀───│   Model     │      │
│    │ nalize   │    │   Results    │    │  Building   │      │
│    └──────────┘    └──────────────┘    └─────────────┘      │
│                                                             │
│         (Iterative - can go back to any previous phase)     │
└─────────────────────────────────────────────────────────────┘
```

---

### Phase 1: Discovery

**Objective:** Understand the business problem and plan the project.

**Activities:**
- Define business objectives and success criteria
- Identify key stakeholders
- Assess available resources (data, technology, people)
- Frame the analytics problem
- Develop initial hypotheses
- Create project plan with timeline

**Key Questions:**
- What problem are we trying to solve?
- What data is available?
- What are the constraints (time, budget, technology)?

**Deliverables:** Project charter, initial hypotheses, resource assessment

---

### Phase 2: Data Preparation

**Objective:** Collect, clean, and transform data for analysis.

**Activities:**
- Data collection from multiple sources
- Data cleaning (handle missing values, outliers, duplicates)
- Data integration (merge datasets)
- Data transformation (normalization, encoding)
- Exploratory Data Analysis (EDA)
- Feature engineering (create new variables)

**Tools:** R (dplyr, tidyr), SQL, ETL tools, Excel

**ETLT Process:**
```
Extract → Transform → Load → Transform
(from sources)  (clean)  (to warehouse)  (for analysis)
```

**Deliverables:** Clean dataset, data quality report, EDA findings

---

### Phase 3: Model Planning

**Objective:** Determine methods, techniques, and workflow for model building.

**Activities:**
- Select analytical techniques (regression, classification, clustering)
- Identify key variables/features
- Define relationships between variables
- Choose tools and algorithms
- Plan model validation approach

**Common Techniques by Problem Type:**

| Problem Type | Techniques |
|-------------|------------|
| Prediction (continuous) | Linear Regression, Random Forest |
| Classification | Logistic Regression, SVM, Decision Trees |
| Clustering | K-Means, Hierarchical |
| Association | Apriori, FP-Growth |

**Deliverables:** Modeling approach document, variable selection, tool selection

---

### Phase 4: Model Building

**Objective:** Build, train, and validate analytical models.

**Activities:**
- Split data into training (70%) and testing (30%) sets
- Build models using selected techniques
- Tune hyperparameters
- Validate model performance
- Compare multiple models
- Handle overfitting/underfitting

**Model Evaluation Metrics:**

| Metric | Use Case |
|--------|----------|
| R-squared | Regression |
| RMSE | Regression |
| Accuracy | Classification |
| Precision/Recall | Classification |
| F1-Score | Classification |
| Silhouette Score | Clustering |

```r
# Example: Build and evaluate
library(caret)
set.seed(42)
trainIndex <- createDataPartition(data$target, p=0.7, list=FALSE)
train <- data[trainIndex, ]
test <- data[-trainIndex, ]

model <- lm(target ~ ., data=train)
predictions <- predict(model, test)
RMSE <- sqrt(mean((test$target - predictions)^2))
```

**Deliverables:** Trained models, performance comparison, best model selection

---

### Phase 5: Communicate Results

**Objective:** Present findings to stakeholders in an understandable format.

**Activities:**
- Create visualizations and dashboards
- Summarize key findings
- Quantify business impact
- Present recommendations
- Address limitations and assumptions
- Tell a data story

**Visualization Tools:** ggplot2 (R), Tableau, Power BI, Shiny (R)

**Best Practices:**
- Use appropriate chart types for the data
- Keep visualizations simple and clear
- Focus on actionable insights
- Tailor communication to audience (technical vs business)

**Deliverables:** Final report, presentation, dashboard

---

### Phase 6: Operationalize

**Objective:** Deploy the model into production and monitor performance.

**Activities:**
- Deploy model into production environment
- Integrate with existing systems
- Set up monitoring and alerting
- Create automated pipelines
- Plan for model retraining
- Document the entire process

**Key Considerations:**
- Model drift (performance degrades over time)
- Scalability
- Real-time vs batch processing
- A/B testing for model comparison

**Deliverables:** Deployed model, monitoring dashboard, documentation, maintenance plan

---

### 4.2 Building a Predictive Model (End-to-End Example)

```r
# Step 1: Load Data
data <- read.csv("sales_data.csv")

# Step 2: EDA
summary(data)
str(data)
pairs(data[, c("price", "advertising", "sales")])

# Step 3: Data Preparation
data <- na.omit(data)
data_scaled <- scale(data[, -1])

# Step 4: Split Data
set.seed(123)
n <- nrow(data)
train_idx <- sample(1:n, 0.7*n)
train <- data[train_idx, ]
test <- data[-train_idx, ]

# Step 5: Build Model
model <- lm(sales ~ price + advertising + season, data=train)
summary(model)

# Step 6: Predict
predictions <- predict(model, newdata=test)

# Step 7: Evaluate
rmse <- sqrt(mean((test$sales - predictions)^2))
r_squared <- summary(model)$r.squared
cat("RMSE:", rmse, "\nR-squared:", r_squared)

# Step 8: Visualize Results
plot(test$sales, predictions, xlab="Actual", ylab="Predicted")
abline(0, 1, col="red")
```

---

## Quick Revision Table

| Topic | Key Points to Remember |
|-------|----------------------|
| R Data Types | numeric, integer, character, logical, complex, factor |
| Import Functions | read.csv(), read.table(), read_excel(), fread() |
| Export Functions | write.csv(), write.xlsx(), saveRDS() |
| Dirty Data Types | Missing, Duplicate, Outlier, Inconsistent, Invalid |
| Outlier Detection | IQR method, Z-score, Boxplot |
| Linear Regression | lm(), R-squared, p-value < 0.05 |
| K-Means | kmeans(), Elbow method, scale data first |
| Hypothesis Test | t.test(), chisq.test(), p-value vs α |
| MapReduce | Map (transform) → Shuffle → Reduce (aggregate) |
| Lifecycle Phases | Discovery → Preparation → Planning → Building → Communicate → Operationalize |

---

## Important Formulas

1. **Linear Regression**: Y = β₀ + β₁X + ε
2. **R-squared**: R² = 1 - (SS_res / SS_tot)
3. **IQR**: IQR = Q3 - Q1; Outlier if X < Q1-1.5×IQR or X > Q3+1.5×IQR
4. **Z-score**: Z = (X - μ) / σ
5. **RMSE**: √(Σ(actual - predicted)² / n)
6. **Euclidean Distance** (for clustering): √(Σ(xi - yi)²)

---

*Prepared for SPPU BDA End Semester Examination - Unit III*
