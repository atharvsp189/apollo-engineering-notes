# Unit IV: Data Pre-processing Techniques (6 Hours)

## SPPU Business Intelligence - Exam Preparation

---

# PART A: DATA VALIDATION

---

## 1. Data Validation

**Definition:** Data Validation is the process of ensuring that data is accurate, complete, and consistent before it is used for analysis or loaded into a data warehouse.

**Purpose:**
- Prevent garbage-in-garbage-out (GIGO)
- Ensure reliable analytics and decision-making
- Maintain data integrity across systems

**Validation Techniques:**
1. **Type Check** – Ensure data matches expected type (integer, string, date)
2. **Range Check** – Values within acceptable range (age: 0-120)
3. **Format Check** – Data follows expected pattern (email: x@y.z)
4. **Consistency Check** – Related fields are logically consistent (start_date < end_date)
5. **Completeness Check** – Required fields are not null/empty
6. **Uniqueness Check** – No unintended duplicates (e.g., primary keys)
7. **Referential Integrity** – Foreign keys reference valid records

---

## 2. Incomplete Data

**Definition:** Incomplete data refers to datasets with missing values in one or more attributes for certain records.

**Causes of Incomplete Data:**
- Data entry errors or omissions
- Equipment malfunction during collection
- Respondent refusal in surveys
- System crashes during data transfer
- Fields not applicable to all records

**Types of Missing Data:**
| Type | Description | Example |
|------|-------------|---------|
| **MCAR** (Missing Completely At Random) | No pattern in missingness | Random sensor failure |
| **MAR** (Missing At Random) | Missingness depends on observed data | Young people skip income field |
| **MNAR** (Missing Not At Random) | Missingness depends on unobserved data | High-income people don't report income |

**Handling Methods:**

1. **Ignore/Delete:**
   - Listwise deletion – Remove entire record
   - Pairwise deletion – Use available data for each analysis
   - Suitable when missing data is small (<5%)

2. **Imputation (Fill with estimated values):**
   - **Mean/Median/Mode** – Replace with central tendency
   - **Regression Imputation** – Predict missing value using other attributes
   - **KNN Imputation** – Use K nearest neighbors' values
   - **Hot-deck** – Replace with value from similar record
   - **Multiple Imputation** – Generate multiple plausible values

3. **Flag and Separate:**
   - Add indicator variable for missingness
   - Analyze complete and incomplete data separately

---

## 3. Data Affected by Noise

### 📝 PYQ: Q3 c) Identify the types of data affected by noise and explain their impact on data quality [4 Marks]

**Answer:**

**Definition:** Noise is random error or variance in a measured variable. It is meaningless or irrelevant data that corrupts actual information.

**Types of Data Affected by Noise:**

**1. Numerical/Continuous Data:**
- Sensor readings with random fluctuations
- Financial data with market micro-fluctuations
- Example: Temperature sensor reads 36.7°C ± 0.5°C randomly
- Impact: Distorts statistical measures, creates false patterns

**2. Categorical Data:**
- Incorrect category assignments due to data entry errors
- Example: "Male" entered as "Mael" or gender coded as "3" instead of "1/2"
- Impact: Skews frequency distributions, affects classification

**3. Textual Data:**
- Typos, abbreviations, inconsistent naming
- Example: "Mumbai", "Bombay", "Mmbai" for same city
- Impact: Fails in grouping, duplicate detection fails

**4. Image/Signal Data:**
- Pixel noise in images, static in audio
- Example: Grainy surveillance footage
- Impact: Reduces pattern recognition accuracy

**Impact on Data Quality:**

1. **Reduced Accuracy** – Analysis results become unreliable
2. **False Patterns** – Noise may appear as trends (overfitting)
3. **Poor Model Performance** – ML models learn noise instead of signal
4. **Increased Variance** – Statistical measures become unstable
5. **Wrong Decisions** – Business decisions based on noisy data are risky

**Noise Handling Methods:**
- **Binning** – Smooth data by replacing with bin means
- **Regression** – Fit data to a function, use fitted values
- **Clustering** – Identify and remove outliers
- **Moving Average** – Smooth time series data
- **Filtering** – Low-pass filters remove high-frequency noise

---

---

# PART B: DATA TRANSFORMATION

---

## 4. Standardization

**Definition:** Standardization (also called normalization) is the process of rescaling data attributes to a common scale without distorting differences in ranges.

**Why Standardize?**
- Different attributes have different scales (age: 0-100, salary: 10000-1000000)
- Many algorithms (KNN, SVM, PCA) are sensitive to scale
- Ensures equal contribution of all features

**Common Methods:**

**1. Z-Score Standardization (Standard Scaling):**
```
z = (x - μ) / σ

Where:
  x = original value
  μ = mean of the attribute
  σ = standard deviation

Result: Mean = 0, Std Dev = 1
```

**Example:**
- Salary data: μ = 50000, σ = 15000
- Value 65000 → z = (65000 - 50000) / 15000 = 1.0

**2. Min-Max Normalization:**
```
x_norm = (x - x_min) / (x_max - x_min)

Result: Values scaled to [0, 1]
```

**Example:**
- Age range [20, 60], value = 35
- x_norm = (35 - 20) / (60 - 20) = 0.375

**3. Decimal Scaling:**
```
x_new = x / 10^j

Where j = smallest integer such that max(|x_new|) < 1
```

**4. Log Transformation:**
```
x_new = log(x)

Used for: Right-skewed distributions
```

**Comparison:**

| Method | Range | Preserves | Best When |
|--------|-------|-----------|-----------|
| Z-Score | (-∞, +∞) | Distribution shape | Normal distribution assumed |
| Min-Max | [0, 1] | Relative distances | Bounded range needed |
| Decimal | (-1, 1) | Order | Quick scaling |
| Log | Varies | Multiplicative relationships | Skewed data |

---

## 5. Feature Extraction

**Definition:** Feature Extraction is the process of creating new features (variables) from existing raw data that better represent the underlying patterns for analysis or modeling.

**Purpose:**
- Reduce dimensionality while preserving information
- Create more meaningful/informative features
- Improve model performance
- Transform raw data into analyzable form

**Methods:**

**1. Principal Component Analysis (PCA):**
- Creates new uncorrelated features (principal components)
- Each PC captures maximum remaining variance
- Linear transformation of original features

**2. Linear Discriminant Analysis (LDA):**
- Creates features that maximize class separation
- Supervised method (uses class labels)

**3. Domain-Specific Feature Extraction:**
- **Text:** TF-IDF, word embeddings, n-grams
- **Images:** Edge detection, color histograms, SIFT features
- **Time Series:** Fourier transform, wavelets, rolling statistics
- **Dates:** Extract day_of_week, month, is_weekend, quarter

**4. Mathematical Transformations:**
- Polynomial features (x², x₁×x₂)
- Ratio features (revenue/employees = revenue_per_employee)
- Aggregation features (avg_purchase_last_30_days)

**Feature Extraction vs Feature Selection:**

| Aspect | Feature Extraction | Feature Selection |
|--------|-------------------|-------------------|
| Output | New features | Subset of original features |
| Interpretability | Lower (transformed) | Higher (original features) |
| Information | Combines multiple features | Keeps individual features |
| Example | PCA components | Selecting top-5 features |

---

---

# PART C: DATA REDUCTION

---

## 6. Data Reduction - Overview

**Definition:** Data Reduction is the process of reducing the volume of data while maintaining analytical integrity, making analysis more efficient without significantly losing information.

### 📝 PYQ: Q3 b) Describe the significance of Data Reduction in the context of large datasets, and explain the methods of Sampling [5 Marks]

**Answer:**

**Significance of Data Reduction:**

1. **Computational Efficiency** – Reduced data requires less processing power and time
2. **Storage Savings** – Less disk space needed for analysis datasets
3. **Faster Analysis** – Algorithms run faster on smaller datasets
4. **Noise Reduction** – Removing irrelevant data can reduce noise
5. **Visualization** – Easier to visualize lower-dimensional data
6. **Avoid Curse of Dimensionality** – High dimensions cause sparse data, poor model performance

**Categories of Data Reduction:**
- **Dimensionality Reduction** – Reduce number of attributes (PCA, Feature Selection)
- **Numerosity Reduction** – Reduce number of records (Sampling, Clustering)
- **Data Compression** – Encode data more efficiently

**Methods of Sampling:**

**Definition:** Sampling selects a representative subset of data from a larger population.

**1. Simple Random Sampling:**
- Every record has equal probability of selection
- Types: With replacement (SRSWR) / Without replacement (SRSWOR)
- Pros: Unbiased, easy to implement
- Cons: May miss rare subgroups

**2. Stratified Sampling:**
- Divide population into strata (groups)
- Sample proportionally from each stratum
- Example: If 60% male, 40% female → sample maintains this ratio
- Pros: Ensures representation of all groups
- Cons: Requires knowledge of strata

**3. Systematic Sampling:**
- Select every k-th record (k = N/n)
- Example: From 10000 records, want 1000 → pick every 10th
- Pros: Simple, evenly spread
- Cons: Bias if data has periodic patterns

**4. Cluster Sampling:**
- Divide data into clusters (geographic, temporal)
- Randomly select entire clusters
- Pros: Practical for large distributed data
- Cons: Higher sampling error

**5. Reservoir Sampling:**
- For streaming data where total size is unknown
- Maintains uniform probability for all items seen so far
- Used in real-time data processing

**When to Use Sampling:**
- Dataset too large for available memory
- Exploratory analysis before full processing
- Quick estimation of population parameters
- Training/testing split in machine learning

---

## 7. Feature Selection

**Definition:** Feature Selection is the process of identifying and selecting the most relevant attributes (features) from the dataset, discarding irrelevant or redundant ones.

**Why Feature Selection?**
- Reduces overfitting
- Improves model accuracy
- Reduces training time
- Improves interpretability

**Methods:**

**1. Filter Methods (Pre-processing, independent of model):**
- **Correlation-based** – Remove highly correlated features
- **Chi-Square Test** – For categorical target variable
- **Information Gain / Mutual Information** – Measures feature's predictive power
- **Variance Threshold** – Remove low-variance features
- Pros: Fast, scalable
- Cons: Ignores feature interactions

**2. Wrapper Methods (Use model performance):**
- **Forward Selection** – Start empty, add best feature one at a time
- **Backward Elimination** – Start with all, remove worst one at a time
- **Recursive Feature Elimination (RFE)** – Train model, remove least important
- Pros: Considers feature interactions
- Cons: Computationally expensive

**3. Embedded Methods (Built into model training):**
- **LASSO Regression (L1)** – Shrinks coefficients to zero
- **Decision Tree Feature Importance** – Based on split criteria
- **Random Forest Importance** – Average decrease in impurity
- Pros: Balance of filter and wrapper
- Cons: Model-specific

---

## 8. Principal Component Analysis (PCA)

**Definition:** PCA is a dimensionality reduction technique that transforms data into a new coordinate system where the axes (principal components) capture maximum variance.

**How PCA Works:**

```
Step 1: Standardize the data (mean=0, std=1)
Step 2: Compute covariance matrix
Step 3: Calculate eigenvalues and eigenvectors
Step 4: Sort eigenvectors by eigenvalues (descending)
Step 5: Select top-k eigenvectors (principal components)
Step 6: Transform data to new k-dimensional space
```

**Key Concepts:**
- **Principal Components (PCs)** – New axes, linear combinations of original features
- **Eigenvalues** – Indicate variance captured by each PC
- **Explained Variance Ratio** – % of total variance each PC captures
- PCs are orthogonal (uncorrelated) to each other

**Example:**
```
Original: 10 features (Age, Salary, Experience, Education, ...)
After PCA: 3 principal components capture 95% variance

PC1 = 0.4×Age + 0.5×Experience + 0.3×Salary + ...  (captures 60% variance)
PC2 = 0.2×Education + 0.6×Skills + ...             (captures 25% variance)
PC3 = ...                                           (captures 10% variance)
```

**Choosing Number of Components:**
- **Scree Plot** – Plot eigenvalues, find "elbow"
- **Cumulative Variance** – Select k where cumulative reaches 90-95%
- **Kaiser's Rule** – Keep PCs with eigenvalue > 1

---

### 📝 PYQ: Q4 a) Compare and contrast Feature Selection and Principal Component Analysis (PCA) in terms of their applicability and limitations [8 Marks]

**Answer:**

**Feature Selection:**

**Definition:** Choosing a subset of original features based on relevance and redundancy criteria.

**Applicability:**
1. When interpretability is critical (medical diagnosis – need to know which features matter)
2. When original feature meaning must be preserved
3. When dataset has many irrelevant/redundant features
4. Classification and regression tasks
5. When domain knowledge can guide selection

**Limitations:**
1. May miss feature interactions (filter methods)
2. Computationally expensive for wrapper methods (2^n subsets possible)
3. Model-dependent results (different models prefer different features)
4. Cannot create new combined features
5. Discrete selection – features are either in or out

---

**Principal Component Analysis (PCA):**

**Definition:** Creating new features (principal components) as linear combinations of original features to capture maximum variance.

**Applicability:**
1. When dimensionality is very high (images, genomics)
2. When multicollinearity exists among features
3. Visualization of high-dimensional data (reduce to 2D/3D)
4. Preprocessing for algorithms sensitive to correlated inputs
5. When variance preservation is the goal

**Limitations:**
1. Loss of interpretability – PCs are abstract combinations
2. Assumes linear relationships between features
3. Sensitive to scale – requires standardization
4. Variance ≠ importance – high variance doesn't always mean informative
5. All original features still needed for transformation
6. Cannot handle categorical data directly

---

**Comparison Table:**

| Criteria | Feature Selection | PCA |
|----------|------------------|-----|
| **Output** | Subset of original features | New transformed features |
| **Interpretability** | High (original features retained) | Low (abstract components) |
| **Information Loss** | Discards entire features | Preserves variance but loses detail |
| **Handles Multicollinearity** | Partially (remove correlated) | Fully (PCs are orthogonal) |
| **Computational Cost** | Varies (filter: low, wrapper: high) | Moderate (matrix operations) |
| **Supervised/Unsupervised** | Both available | Unsupervised only |
| **Scalability** | Good for filter methods | Good (linear algebra) |
| **Data Type** | Numerical and categorical | Numerical only |
| **Feature Interactions** | Wrapper methods capture some | Captures through combinations |
| **Domain Knowledge** | Can incorporate | Does not use |
| **Reversibility** | Features can be added back | Transformation is one-way for analysis |

**When to Use What:**
- **Use Feature Selection when:** Interpretability needed, domain features are meaningful, mix of data types
- **Use PCA when:** Very high dimensions, multicollinearity, visualization needed, numeric data only
- **Use Both:** Feature selection first to remove obvious irrelevant features, then PCA on remaining

**Conclusion:** Feature Selection preserves original feature meaning and is preferred when interpretability matters. PCA is superior for handling multicollinearity and extreme dimensionality but sacrifices interpretability. The choice depends on the specific analytical goals, data characteristics, and whether understanding individual feature contributions is necessary.

---

## 9. Data Discretization

### 📝 PYQ: Q3 a) Evaluate the effectiveness of different methods of Data Discretization in improving the interpretability of data [8 Marks]

**Answer:**

**Definition:** Data Discretization is the process of converting continuous numerical attributes into discrete intervals (bins/categories), making data easier to understand and analyze.

**Why Discretize?**
- Some algorithms work only with categorical data (Naive Bayes, Apriori)
- Reduces noise in continuous data
- Improves interpretability for humans
- Simplifies decision rules

**Methods of Discretization:**

### 1. Equal-Width (Equal-Interval) Binning

**Method:** Divide range into N bins of equal width.
```
Width = (Max - Min) / N

Example: Age [20-60], 4 bins
Bin 1: [20-30], Bin 2: (30-40], Bin 3: (40-50], Bin 4: (50-60]
```

**Effectiveness:**
- ✅ Simple to implement and understand
- ✅ Good interpretability (clear boundaries)
- ❌ Sensitive to outliers (one extreme value creates sparse bins)
- ❌ Uneven distribution across bins (some bins may be empty)

**Interpretability Rating: Medium** – Boundaries are uniform but may not align with meaningful categories.

---

### 2. Equal-Frequency (Equal-Depth) Binning

**Method:** Each bin has approximately the same number of records.
```
Example: 100 records, 4 bins → 25 records each
Bin 1: [20-28], Bin 2: [29-35], Bin 3: [36-45], Bin 4: [46-60]
```

**Effectiveness:**
- ✅ Every bin has equal representation
- ✅ Handles skewed distributions well
- ❌ Bin boundaries may split natural groups
- ❌ Identical values may end up in different bins

**Interpretability Rating: Medium** – Equal representation aids comparison but boundaries may seem arbitrary.

---

### 3. Entropy-Based (Information Gain) Discretization

**Method:** Uses class labels to find splits that maximize information gain (supervised).
```
Choose split point that minimizes entropy:
Entropy(S) = -Σ p_i × log₂(p_i)

Split where info gain is maximum
```

**Effectiveness:**
- ✅ Creates bins aligned with class boundaries
- ✅ Maximizes predictive power
- ✅ Optimal for classification tasks
- ❌ Requires class labels (supervised only)
- ❌ More complex to implement

**Interpretability Rating: High** – Bins correspond to meaningful class separations.

---

### 4. Binning by Natural Boundaries (Domain-Driven)

**Method:** Use domain knowledge to define meaningful categories.
```
Example: Income
Low: < ₹25,000
Medium: ₹25,000 - ₹75,000
High: > ₹75,000
```

**Effectiveness:**
- ✅ Highest interpretability (meaningful to users)
- ✅ Aligns with business rules
- ✅ Stable across datasets
- ❌ Requires domain expertise
- ❌ May not be optimal for modeling

**Interpretability Rating: Very High** – Directly maps to business understanding.

---

### 5. Clustering-Based Discretization

**Method:** Apply clustering (K-means) to find natural groupings, use cluster boundaries as bin edges.

**Effectiveness:**
- ✅ Discovers natural data groupings
- ✅ Adapts to data distribution
- ❌ K must be specified
- ❌ Results vary with initialization
- ❌ Less intuitive boundaries

**Interpretability Rating: Medium-Low** – Boundaries based on data patterns, not human logic.

---

### 6. ChiMerge (Chi-Square Based)

**Method:** Bottom-up approach; start with many intervals, merge adjacent intervals if chi-square test shows they're statistically similar.

**Effectiveness:**
- ✅ Statistically sound
- ✅ Automatic determination of bin count
- ❌ Computationally intensive
- ❌ Complex to explain

**Interpretability Rating: Medium** – Statistical rigor but harder to explain.

---

**Comparative Evaluation:**

| Method | Interpretability | Accuracy | Complexity | Best For |
|--------|-----------------|----------|------------|----------|
| Equal-Width | Medium | Low | Very Low | Quick exploration |
| Equal-Frequency | Medium | Medium | Low | Skewed data |
| Entropy-Based | High | High | Medium | Classification |
| Domain-Driven | Very High | Varies | Low | Business reports |
| Clustering | Medium-Low | High | Medium | Natural groups |
| ChiMerge | Medium | High | High | Statistical rigor |

**Conclusion:** The choice of discretization method depends on the goal:
- For **maximum interpretability** → Domain-driven or Entropy-based
- For **quick analysis** → Equal-width or Equal-frequency
- For **modeling accuracy** → Entropy-based or ChiMerge
- For **discovering patterns** → Clustering-based

Entropy-based methods offer the best balance of interpretability and analytical effectiveness for supervised tasks, while domain-driven methods are most interpretable for business reporting.

---

---

# PART D: DATA EXPLORATION

---

## 10. Data Exploration - Overview

### 📝 PYQ: Q4 c) Discuss the importance of Data Exploration in the data analysis pipeline [4 Marks]

**Answer:**

**Definition:** Data Exploration (Exploratory Data Analysis - EDA) is the initial phase of data analysis where datasets are examined to understand their main characteristics, discover patterns, spot anomalies, and test hypotheses using statistical and visual techniques.

**Importance in Data Analysis Pipeline:**

1. **Understanding Data Structure:**
   - Identify data types, ranges, and distributions
   - Discover relationships between variables
   - Determine data quality issues early

2. **Guiding Analysis Strategy:**
   - Helps choose appropriate algorithms
   - Informs feature engineering decisions
   - Identifies need for transformation or normalization

3. **Detecting Anomalies and Outliers:**
   - Spot data entry errors before they affect analysis
   - Identify genuine outliers requiring special treatment
   - Validate assumptions about data

4. **Hypothesis Generation:**
   - Visual patterns suggest hypotheses to test
   - Unexpected correlations prompt deeper investigation
   - Guides business question formulation

5. **Data Quality Assessment:**
   - Identifies missing values and their patterns
   - Detects inconsistencies across attributes
   - Validates data against domain expectations

6. **Communication:**
   - Visualizations help communicate findings to stakeholders
   - Summary statistics provide quick data overview
   - Facilitates team alignment on data characteristics

**Position in Pipeline:**
```
Data Collection → [DATA EXPLORATION] → Preprocessing → Modeling → Evaluation → Deployment
                         ↕
                  Informs all subsequent steps
```

**Conclusion:** Data Exploration is not optional – it's essential for preventing downstream errors, guiding methodology choices, and ensuring analysts truly understand the data before building models or making decisions.

---

## 11. Univariate Analysis

**Definition:** Analysis of a single variable at a time to understand its distribution, central tendency, spread, and outliers.

### Graphical Analysis of Categorical Attributes:

| Chart | Use | Example |
|-------|-----|---------|
| **Bar Chart** | Frequency of categories | Product-wise sales count |
| **Pie Chart** | Proportion of categories | Market share % |
| **Pareto Chart** | Ranked categories with cumulative % | Top defect types |
| **Frequency Table** | Count/percentage per category | Gender distribution |

### Graphical Analysis of Numerical Attributes:

| Chart | Use | Example |
|-------|-----|---------|
| **Histogram** | Distribution shape | Age distribution |
| **Box Plot** | Summary stats + outliers | Salary spread |
| **Density Plot** | Smooth distribution curve | Score distribution |
| **Stem-and-Leaf** | Detailed distribution | Small datasets |
| **QQ Plot** | Check normality | Test if data is normally distributed |

### Measures of Central Tendency:

**1. Mean (Average):**
```
Mean = Σxᵢ / n

Example: {10, 20, 30, 40, 50} → Mean = 150/5 = 30
```
- Sensitive to outliers
- Best for symmetric distributions

**2. Median (Middle Value):**
```
Median = Middle value when sorted
Odd n: middle element
Even n: average of two middle elements

Example: {10, 20, 30, 40, 100} → Median = 30
```
- Robust to outliers
- Best for skewed distributions

**3. Mode (Most Frequent):**
```
Mode = Value with highest frequency

Example: {1, 2, 2, 3, 3, 3, 4} → Mode = 3
```
- Works for categorical data
- May not exist or be multiple

### Measures of Dispersion:

**1. Range:**
```
Range = Max - Min
Example: {10, 20, 30, 40, 50} → Range = 50 - 10 = 40
```

**2. Variance (σ²):**
```
Variance = Σ(xᵢ - μ)² / n          (population)
Variance = Σ(xᵢ - x̄)² / (n-1)     (sample)
```

**3. Standard Deviation (σ):**
```
SD = √Variance

Example: {2, 4, 4, 4, 5, 5, 7, 9}
Mean = 5, Variance = 4, SD = 2
```

**4. Interquartile Range (IQR):**
```
IQR = Q3 - Q1
Q1 = 25th percentile
Q3 = 75th percentile
```

**5. Coefficient of Variation (CV):**
```
CV = (SD / Mean) × 100%
Used to compare variability across different scales
```

### Identification of Outliers:

**Methods:**

**1. IQR Method (Box Plot):**
```
Lower Fence = Q1 - 1.5 × IQR
Upper Fence = Q3 + 1.5 × IQR
Values outside fences = Outliers

Example: Q1=25, Q3=75, IQR=50
Lower = 25 - 75 = -50
Upper = 75 + 75 = 150
Any value < -50 or > 150 is outlier
```

**2. Z-Score Method:**
```
If |z| > 3, the value is an outlier
z = (x - μ) / σ
```

**3. Modified Z-Score (MAD-based):**
```
MAD = Median(|xᵢ - Median(x)|)
Modified Z = 0.6745 × (xᵢ - Median) / MAD
If |Modified Z| > 3.5, outlier
```

**Handling Outliers:**
- Remove (if error)
- Cap/Winsorize (replace with boundary values)
- Transform (log, sqrt)
- Keep (if genuine extreme value)
- Separate analysis

---

## 12. Bivariate Analysis

**Definition:** Analysis of two variables simultaneously to understand relationships, correlations, and associations between them.

### 📝 PYQ: Q4 b) Differentiate between Univariate, Bivariate and Multivariate analysis [5 Marks]

**Answer:**

| Aspect | Univariate | Bivariate | Multivariate |
|--------|-----------|-----------|--------------|
| **Variables** | Single variable | Two variables | Three or more variables |
| **Purpose** | Describe distribution | Find relationships | Find complex patterns |
| **Graphs** | Histogram, Box plot, Bar chart | Scatter plot, Cross-tab, Grouped bar | 3D plots, Heatmaps, Parallel coordinates |
| **Statistics** | Mean, Median, SD, Skewness | Correlation, Chi-square, ANOVA | Multiple regression, PCA, Factor analysis |
| **Example** | "What is average salary?" | "Does experience affect salary?" | "How do age, education & experience together affect salary?" |
| **Complexity** | Low | Medium | High |
| **Output** | Distribution summary | Relationship strength & direction | Interaction effects, dimensionality patterns |

**Detailed Differentiation:**

**Univariate Analysis:**
- Examines ONE variable in isolation
- Answers: "What does this variable look like?"
- Techniques: Frequency distribution, central tendency, dispersion
- Visualizations: Histogram, box plot, pie chart
- Example: Distribution of customer ages

**Bivariate Analysis:**
- Examines relationship between TWO variables
- Answers: "Are these two variables related? How?"
- Techniques: Correlation, regression, chi-square test
- Visualizations: Scatter plot, cross-tabulation, line chart
- Example: Relationship between advertising spend and sales

**Multivariate Analysis:**
- Examines THREE or more variables simultaneously
- Answers: "What are the combined effects and hidden patterns?"
- Techniques: Multiple regression, PCA, cluster analysis, MANOVA
- Visualizations: 3D scatter, heatmap, parallel coordinates
- Example: Predicting house price from size, location, age, and amenities

---

### Graphical Analysis (Bivariate):

| Variables | Chart Type | Example |
|-----------|-----------|---------|
| Numerical × Numerical | Scatter plot | Height vs Weight |
| Numerical × Categorical | Box plot (grouped), Violin plot | Salary by Department |
| Categorical × Categorical | Stacked bar, Grouped bar, Mosaic plot | Gender × Product preference |

### Measures of Correlation for Numerical Attributes:

**1. Pearson Correlation Coefficient (r):**
```
r = Σ[(xᵢ - x̄)(yᵢ - ȳ)] / √[Σ(xᵢ - x̄)² × Σ(yᵢ - ȳ)²]

Range: [-1, +1]
+1 = Perfect positive linear correlation
 0 = No linear correlation
-1 = Perfect negative linear correlation
```

**Interpretation:**
| r Value | Strength |
|---------|----------|
| 0.0 - 0.3 | Weak |
| 0.3 - 0.7 | Moderate |
| 0.7 - 1.0 | Strong |

**2. Spearman Rank Correlation (ρ):**
```
ρ = 1 - (6 × Σdᵢ²) / (n(n²-1))

Where dᵢ = difference in ranks
```
- Non-parametric (doesn't assume normality)
- Measures monotonic relationship (not just linear)
- Robust to outliers

**3. Covariance:**
```
Cov(X,Y) = Σ[(xᵢ - x̄)(yᵢ - ȳ)] / (n-1)

Positive: Variables move together
Negative: Variables move opposite
Zero: No linear relationship
```
- Not standardized (depends on scale)
- Correlation = standardized covariance

### Contingency Tables for Categorical Attributes:

**Definition:** A contingency table (cross-tabulation) shows frequency distribution of two categorical variables.

**Example:**
| | Buy | Not Buy | Total |
|---|---|---|---|
| Male | 60 | 40 | 100 |
| Female | 45 | 55 | 100 |
| Total | 105 | 95 | 200 |

**Chi-Square Test for Independence:**
```
χ² = Σ [(Observed - Expected)² / Expected]

Expected = (Row Total × Column Total) / Grand Total

If χ² > critical value → Variables are associated
```

**Cramér's V (Strength of Association):**
```
V = √(χ² / (n × (min(r,c) - 1)))

Range: [0, 1]
0 = No association
1 = Perfect association
```

---

## 13. Multivariate Analysis

**Definition:** Simultaneous analysis of three or more variables to understand complex interactions and patterns that cannot be observed in univariate or bivariate analysis.

### Graphical Analysis (Multivariate):

| Technique | Description | Use Case |
|-----------|-------------|----------|
| **3D Scatter Plot** | Three variables on x, y, z axes | Exploring 3-way relationships |
| **Pair Plot (Scatter Matrix)** | Grid of all pairwise scatter plots | Overview of all relationships |
| **Heatmap (Correlation Matrix)** | Color-coded correlation values | Identify correlated features |
| **Parallel Coordinates** | Each axis = one variable, lines connect values | Comparing patterns across many variables |
| **Bubble Chart** | Scatter + size dimension | 3 variables in 2D |
| **Andrews Curves** | Map multivariate data to curves | Cluster identification |

### Measures of Correlation for Numerical Attributes (Multivariate):

**1. Correlation Matrix:**
```
         Age    Salary   Experience
Age      1.00   0.65     0.85
Salary   0.65   1.00     0.78
Exp      0.85   0.78     1.00
```
- Shows all pairwise correlations
- Symmetric matrix (diagonal = 1)
- Visualized as heatmap

**2. Partial Correlation:**
```
r_XY.Z = Correlation between X and Y, controlling for Z

Removes confounding effect of Z
```
- Example: Correlation between ice cream sales and drowning, controlling for temperature
- Reveals true relationships

**3. Multiple Correlation Coefficient (R):**
```
R = Correlation between Y and its predicted value from multiple X's
R² = Proportion of Y's variance explained by all X's combined

Example: R² = 0.85 means 85% of salary variance explained by age, education, and experience together
```

**4. Variance Inflation Factor (VIF):**
```
VIF = 1 / (1 - R²ᵢ)

Where R²ᵢ = R² of feature i regressed on all other features
VIF > 5-10 indicates multicollinearity
```

**Multivariate Techniques:**

| Technique | Purpose | Output |
|-----------|---------|--------|
| **Multiple Regression** | Predict Y from multiple X's | Coefficients, R² |
| **PCA** | Reduce dimensions | Principal components |
| **Factor Analysis** | Find latent variables | Factors and loadings |
| **Cluster Analysis** | Group similar observations | Clusters |
| **MANOVA** | Compare groups on multiple outcomes | F-statistics |
| **Discriminant Analysis** | Classify into groups | Decision boundaries |

---

---

# QUICK REVISION - Key Points for Exam

| Topic | Key Point to Remember |
|-------|----------------------|
| Incomplete Data | MCAR/MAR/MNAR; Handle by deletion or imputation |
| Noise | Random error; Handle by binning, regression, smoothing |
| Standardization | Z-score: (x-μ)/σ; Min-Max: (x-min)/(max-min) |
| Feature Extraction | Creates NEW features; PCA most common method |
| Feature Selection | Selects SUBSET; Filter/Wrapper/Embedded methods |
| PCA | Eigenvalues + Eigenvectors; Captures maximum variance |
| Sampling | Simple Random, Stratified, Systematic, Cluster |
| Discretization | Equal-width, Equal-frequency, Entropy-based |
| Univariate | One variable: Mean, Median, Mode, SD, IQR |
| Bivariate | Two variables: Pearson r, Spearman ρ, Chi-square |
| Multivariate | 3+ variables: Correlation matrix, PCA, Multiple regression |
| Outliers | IQR method: < Q1-1.5×IQR or > Q3+1.5×IQR |
| Pearson r | Linear correlation, range [-1,+1], sensitive to outliers |
| Chi-Square | Tests independence of two categorical variables |

---

# FORMULAS TO REMEMBER

```
Mean = Σx / n
Variance = Σ(x-μ)² / n
SD = √Variance
Z-score = (x - μ) / σ
Min-Max = (x - min) / (max - min)
IQR = Q3 - Q1
Outlier if: x < Q1-1.5×IQR or x > Q3+1.5×IQR
Pearson r = Σ[(x-x̄)(y-ȳ)] / √[Σ(x-x̄)² × Σ(y-ȳ)²]
Spearman ρ = 1 - 6Σd² / n(n²-1)
χ² = Σ[(O-E)² / E]
CV = (SD/Mean) × 100%
```

---

*Prepared for SPPU BE IT/CS - Business Intelligence End Semester Examination*
