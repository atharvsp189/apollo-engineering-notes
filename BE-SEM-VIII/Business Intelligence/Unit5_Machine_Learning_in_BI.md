# Unit V: Impact of Machine Learning in BI (6 Hours)

## SPPU Business Intelligence - Exam Preparation

---

# PART A: REGRESSION

---

## 1. Regression Problems

**Definition:** Regression is a supervised machine learning technique used to predict a continuous numerical output variable based on one or more input variables.

**Goal:** Find the relationship between independent variables (X) and a dependent variable (Y) to predict future values.

**Types of Regression Problems in BI:**

| Problem | Input (X) | Output (Y) |
|---------|-----------|-------------|
| Sales Forecasting | Ad spend, season, price | Revenue (₹) |
| Stock Price Prediction | Volume, market index, news | Price (₹) |
| Customer Lifetime Value | Purchase frequency, tenure | CLV (₹) |
| Demand Estimation | Price, day, weather | Units sold |
| Risk Assessment | Income, credit score | Loan default amount |

**Characteristics of Regression Problems:**
- Output is continuous (not categorical)
- Relationship can be linear or non-linear
- Model learns from historical labeled data
- Predicts unseen data points

**Types of Regression:**
1. **Simple Linear Regression** – One independent variable
2. **Multiple Linear Regression** – Multiple independent variables
3. **Polynomial Regression** – Non-linear relationships
4. **Ridge/Lasso Regression** – Regularized regression

---

## 2. Evaluation of Regression Models

### 📝 PYQ: Q5 c) List and explain the key components involved in the evaluation of regression models [4 Marks]

**Answer:**

**Key Evaluation Metrics:**

**1. Mean Absolute Error (MAE):**
```
MAE = (1/n) × Σ|yᵢ - ŷᵢ|

- Average absolute difference between actual and predicted
- Same unit as target variable
- Less sensitive to outliers
- Example: MAE = ₹5000 means predictions off by ₹5000 on average
```

**2. Mean Squared Error (MSE):**
```
MSE = (1/n) × Σ(yᵢ - ŷᵢ)²

- Squares the errors (penalizes large errors more)
- Always positive
- Units are squared (₹²)
- Sensitive to outliers
```

**3. Root Mean Squared Error (RMSE):**
```
RMSE = √MSE = √[(1/n) × Σ(yᵢ - ŷᵢ)²]

- Same unit as target variable
- Most commonly used metric
- Penalizes large errors more than MAE
- Example: RMSE = ₹7000
```

**4. R-Squared (R² - Coefficient of Determination):**
```
R² = 1 - (SS_res / SS_tot)

Where:
SS_res = Σ(yᵢ - ŷᵢ)²  (residual sum of squares)
SS_tot = Σ(yᵢ - ȳ)²   (total sum of squares)

Range: [0, 1] (can be negative for very bad models)
R² = 1: Perfect fit
R² = 0: Model no better than predicting mean
Example: R² = 0.85 means 85% of variance is explained
```

**5. Adjusted R-Squared:**
```
Adj R² = 1 - [(1-R²)(n-1) / (n-k-1)]

Where k = number of predictors
- Penalizes adding irrelevant features
- Better for comparing models with different feature counts
```

**Other Evaluation Components:**
- **Residual Analysis** – Plot residuals to check assumptions (randomness, normality)
- **Cross-Validation** – K-fold CV for robust estimate of performance
- **Training vs Test Error** – Check for overfitting/underfitting

---

## 3. Linear Regression

**Definition:** Linear Regression models the relationship between a dependent variable (Y) and one or more independent variables (X) by fitting a linear equation.

**Simple Linear Regression:**
```
Y = β₀ + β₁X + ε

Where:
Y = Dependent variable (predicted value)
β₀ = Y-intercept (value of Y when X=0)
β₁ = Slope (change in Y per unit change in X)
X = Independent variable
ε = Error term (residual)
```

**Multiple Linear Regression:**
```
Y = β₀ + β₁X₁ + β₂X₂ + ... + βₙXₙ + ε
```

**Finding Best Fit Line – Ordinary Least Squares (OLS):**
```
Minimize: Σ(yᵢ - ŷᵢ)² = Σ(yᵢ - β₀ - β₁xᵢ)²

Formulas:
β₁ = [nΣxᵢyᵢ - ΣxᵢΣyᵢ] / [nΣxᵢ² - (Σxᵢ)²]
β₀ = ȳ - β₁x̄
```

**Example:**
```
Data: Study hours (X) vs Marks (Y)
X: 2, 3, 5, 7, 9
Y: 45, 55, 65, 80, 90

Σx = 26, Σy = 335, Σxy = 1955, Σx² = 168, n = 5
x̄ = 5.2, ȳ = 67

β₁ = (5×1955 - 26×335) / (5×168 - 26²) = (9775-8710)/(840-676) = 1065/164 = 6.49
β₀ = 67 - 6.49×5.2 = 67 - 33.75 = 33.25

Model: Y = 33.25 + 6.49X
Prediction: For X=6 hours → Y = 33.25 + 6.49(6) = 72.19 marks
```

**Assumptions of Linear Regression:**
1. **Linearity** – Relationship between X and Y is linear
2. **Independence** – Observations are independent
3. **Homoscedasticity** – Constant variance of errors
4. **Normality** – Errors are normally distributed
5. **No Multicollinearity** – Independent variables not highly correlated

**Applications in BI:**
- Sales forecasting based on marketing spend
- Revenue prediction based on economic indicators
- Cost estimation for projects
- Demand planning

---

---

# PART B: CLASSIFICATION

---

## 4. Classification Problems

**Definition:** Classification is a supervised learning technique where the model predicts a categorical/discrete class label for new observations based on training data.

**Goal:** Assign input data to one of predefined categories/classes.

**Examples in BI:**

| Problem | Input Features | Output Classes |
|---------|---------------|----------------|
| Spam Detection | Email content, sender | Spam / Not Spam |
| Customer Churn | Usage, tenure, complaints | Churn / Stay |
| Loan Approval | Income, credit score, age | Approve / Reject |
| Fraud Detection | Transaction amount, location | Fraud / Genuine |
| Sentiment Analysis | Review text | Positive / Negative / Neutral |

**Types:**
- **Binary Classification** – Two classes (Yes/No, 0/1)
- **Multi-class Classification** – More than two classes
- **Multi-label Classification** – Multiple labels per instance

---

## 5. Evaluation of Classification Models

**Confusion Matrix:**
```
                    Predicted
                  Positive  Negative
Actual Positive [   TP    |   FN   ]
Actual Negative [   FP    |   TN   ]

TP = True Positive (correctly predicted positive)
TN = True Negative (correctly predicted negative)
FP = False Positive (Type I error - false alarm)
FN = False Negative (Type II error - missed detection)
```

**Key Metrics:**

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
Precision = TP / (TP + FP)        → "Of predicted positives, how many correct?"
Recall = TP / (TP + FN)           → "Of actual positives, how many found?"
F1-Score = 2 × (Precision × Recall) / (Precision + Recall)
Specificity = TN / (TN + FP)     → "Of actual negatives, how many identified?"
```

**Example:**
```
Email Spam Classifier - 100 emails tested:
TP=40 (spam correctly caught), FP=5 (ham marked spam)
FN=10 (spam missed), TN=45 (ham correctly passed)

Accuracy = (40+45)/100 = 85%
Precision = 40/(40+5) = 88.9%
Recall = 40/(40+10) = 80%
F1 = 2×(0.889×0.8)/(0.889+0.8) = 84.2%
```

**Other Evaluation Methods:**
- **ROC Curve** – Plot TPR vs FPR at different thresholds
- **AUC** – Area Under ROC Curve (0.5=random, 1.0=perfect)
- **K-Fold Cross Validation** – Robust performance estimate
- **Stratified Sampling** – Maintain class distribution in train/test

---

## 6. Bayesian Methods

### 📝 PYQ: Q5 b) Define Bayesian methods and Logistic Regression in the context of classification [5 Marks]

**Answer:**

**Bayesian Methods:**

**Definition:** Bayesian classification methods are based on Bayes' Theorem, which calculates the probability of a class given the observed features.

**Bayes' Theorem:**
```
P(C|X) = [P(X|C) × P(C)] / P(X)

Where:
P(C|X) = Posterior probability (probability of class C given features X)
P(X|C) = Likelihood (probability of features X given class C)
P(C)   = Prior probability (overall probability of class C)
P(X)   = Evidence (probability of features X)
```

**Naive Bayes Classifier:**

"Naive" because it assumes all features are **conditionally independent** given the class.

```
P(C|X₁,X₂,...,Xₙ) ∝ P(C) × P(X₁|C) × P(X₂|C) × ... × P(Xₙ|C)

Predict class with highest posterior probability:
ŷ = argmax P(C) × ∏P(Xᵢ|C)
```

**Example:**
```
Classify: Should we play tennis? (Yes/No)
Features: Outlook=Sunny, Temp=Hot, Humidity=High, Wind=Weak

P(Yes) = 9/14, P(No) = 5/14
P(Sunny|Yes) = 2/9, P(Sunny|No) = 3/5
P(Hot|Yes) = 2/9, P(Hot|No) = 2/5
P(High|Yes) = 3/9, P(High|No) = 4/5
P(Weak|Yes) = 6/9, P(Weak|No) = 2/5

P(Yes|X) ∝ 9/14 × 2/9 × 2/9 × 3/9 × 6/9 = 0.0053
P(No|X)  ∝ 5/14 × 3/5 × 2/5 × 4/5 × 2/5 = 0.0274

Since P(No|X) > P(Yes|X) → Predict "No" (Don't play)
```

**Types of Naive Bayes:**
- **Gaussian NB** – For continuous features (assumes normal distribution)
- **Multinomial NB** – For discrete counts (text classification)
- **Bernoulli NB** – For binary features (present/absent)

**Advantages:**
- Simple and fast
- Works well with high-dimensional data
- Good for text classification
- Handles missing data naturally

**Limitations:**
- Independence assumption rarely true
- Cannot capture feature interactions
- Poor probability estimates (but good classifications)

---

## 7. Logistic Regression

### 📝 PYQ: Q6 a) Explain logistic regression with example considering relevant variables and data [9 Marks]

**Answer:**

**Definition:** Logistic Regression is a classification algorithm that models the probability of a binary outcome using the logistic (sigmoid) function. Despite its name, it's used for classification, not regression.

**Logistic/Sigmoid Function:**
```
σ(z) = 1 / (1 + e⁻ᶻ)

Where z = β₀ + β₁X₁ + β₂X₂ + ... + βₙXₙ

Output: Probability between 0 and 1
Decision: If P ≥ 0.5 → Class 1, else → Class 0
```

**Sigmoid Curve:**
```
P(Y=1)
1.0 |                    ___________
    |                   /
0.5 |_ _ _ _ _ _ _ _ _/_ _ _ _ _ _ _ (threshold)
    |                 /
0.0 |________________/
    ──────────────────────────────── z
```

**Key Concepts:**

**Odds and Log-Odds (Logit):**
```
Odds = P(event) / P(no event) = P / (1-P)
Log-odds (Logit) = ln(P / (1-P)) = β₀ + β₁X₁ + β₂X₂ + ...
```

**Detailed Example: Student Pass/Fail Prediction**

**Problem:** Predict whether a student will pass (1) or fail (0) based on study hours and attendance.

**Data:**

| Student | Hours (X₁) | Attendance% (X₂) | Result (Y) |
|---------|------------|-------------------|------------|
| S1 | 2 | 60 | 0 (Fail) |
| S2 | 3 | 70 | 0 (Fail) |
| S3 | 5 | 75 | 1 (Pass) |
| S4 | 6 | 80 | 1 (Pass) |
| S5 | 1 | 50 | 0 (Fail) |
| S6 | 7 | 85 | 1 (Pass) |
| S7 | 4 | 65 | 0 (Fail) |
| S8 | 8 | 90 | 1 (Pass) |
| S9 | 5 | 78 | 1 (Pass) |
| S10 | 3 | 55 | 0 (Fail) |

**Model Building:**

Step 1: Define the model
```
P(Pass) = 1 / (1 + e^-(β₀ + β₁×Hours + β₂×Attendance))
```

Step 2: After training (using Maximum Likelihood Estimation):
```
Suppose we get: β₀ = -8.5, β₁ = 0.6, β₂ = 0.08
```

Step 3: Prediction for new student (Hours=6, Attendance=82%):
```
z = -8.5 + 0.6(6) + 0.08(82)
z = -8.5 + 3.6 + 6.56
z = 1.66

P(Pass) = 1 / (1 + e^-1.66) = 1 / (1 + 0.19) = 0.84

Since 0.84 > 0.5 → Predict: PASS ✓
```

Step 4: Interpretation of coefficients:
```
β₁ = 0.6: Each additional study hour increases log-odds of passing by 0.6
         OR increases odds by e^0.6 = 1.82 times (82% more likely)
β₂ = 0.08: Each 1% increase in attendance increases log-odds by 0.08
```

**Another BI Example: Customer Churn Prediction**

```
Features: tenure(months), monthly_charges(₹), contract_type(0=monthly, 1=yearly)
Model: P(Churn) = σ(-1.2 + 0.05×charges - 0.03×tenure - 1.5×contract)

Customer: tenure=6, charges=₹2000, contract=0(monthly)
z = -1.2 + 0.05(2000) - 0.03(6) - 1.5(0) = -1.2 + 100 - 0.18 - 0 = 98.62
P(Churn) ≈ 1.0 → HIGH CHURN RISK
```

**Assumptions:**
1. Binary dependent variable
2. Linear relationship between log-odds and features
3. No multicollinearity among predictors
4. Large sample size preferred
5. Independent observations

**Advantages:**
- Output is interpretable probability
- Coefficients show feature importance
- No distributional assumptions on X
- Works well in practice for binary classification

**Limitations:**
- Assumes linear decision boundary
- Cannot handle complex non-linear relationships
- Sensitive to outliers
- Requires large samples for stable estimates

**Applications in BI:**
- Customer churn prediction
- Credit risk scoring
- Marketing response modeling
- Employee attrition prediction
- Disease prediction in healthcare

---

---

# PART C: CLUSTERING

---

## 8. Clustering Methods

**Definition:** Clustering is an unsupervised learning technique that groups similar data points together without predefined labels.

**Goal:** Maximize intra-cluster similarity and minimize inter-cluster similarity.

**Applications in BI:**
- Customer segmentation
- Market basket analysis
- Anomaly/fraud detection
- Document categorization
- Image segmentation

**Types of Clustering:**

| Type | Method | Example |
|------|--------|---------|
| **Partitioning** | K-Means, K-Medoids | Customer segments |
| **Hierarchical** | Agglomerative, Divisive | Taxonomy creation |
| **Density-Based** | DBSCAN | Noise handling |
| **Grid-Based** | STING, CLIQUE | Spatial data |
| **Model-Based** | GMM, EM | Soft clustering |

---

## 9. Partition Methods (K-Means)

**Definition:** Partition methods divide data into K non-overlapping clusters where each data point belongs to exactly one cluster.

**K-Means Algorithm:**

```
Input: Dataset D, number of clusters K
Output: K clusters

Step 1: Randomly initialize K centroids
Step 2: REPEAT
    a) Assign each point to nearest centroid (using Euclidean distance)
    b) Recalculate centroids as mean of assigned points
Step 3: UNTIL centroids don't change (convergence)
```

**Distance Measure (Euclidean):**
```
d(p, q) = √[Σ(pᵢ - qᵢ)²]

For 2D: d = √[(x₁-x₂)² + (y₁-y₂)²]
```

**Example:**
```
Data points: A(1,1), B(1.5,2), C(3,4), D(5,7), E(3.5,5), F(4.5,5), G(3.5,4.5)
K = 2

Iteration 1:
- Initial centroids: C₁(1,1), C₂(5,7)
- Assign: Cluster1={A,B,C}, Cluster2={D,E,F,G}
- New centroids: C₁=(1+1.5+3)/3, (1+2+4)/3 = (1.83, 2.33)
                 C₂=(5+3.5+4.5+3.5)/4, (7+5+5+4.5)/4 = (4.125, 5.375)

Iteration 2:
- Reassign based on new centroids
- Continue until stable...
```

**Advantages:**
- Simple and efficient: O(nKt) where t=iterations
- Works well for spherical clusters
- Scales to large datasets

**Limitations:**
- Must specify K in advance
- Sensitive to initial centroid selection
- Assumes spherical clusters of similar size
- Sensitive to outliers

**Choosing K:**
- **Elbow Method** – Plot WSS vs K, find elbow
- **Silhouette Score** – Measure cluster quality
- **Domain Knowledge** – Business requirements

---

## 10. Hierarchical Methods

### 📝 PYQ: Q6 c) Define Hierarchical methods in clustering and describe their characteristics [4 Marks]

**Answer:**

**Definition:** Hierarchical clustering builds a tree-like structure (dendrogram) of clusters by either successively merging (agglomerative) or splitting (divisive) clusters.

**Types:**

**1. Agglomerative (Bottom-Up):**
```
Step 1: Start with each point as its own cluster (n clusters)
Step 2: Find two closest clusters
Step 3: Merge them into one cluster
Step 4: Repeat Steps 2-3 until one cluster remains (or desired K reached)
```

**2. Divisive (Top-Down):**
```
Step 1: Start with all points in one cluster
Step 2: Find most dissimilar cluster
Step 3: Split it into two sub-clusters
Step 4: Repeat until each point is its own cluster (or desired K reached)
```

**Linkage Criteria (How to measure distance between clusters):**

| Linkage | Definition | Characteristic |
|---------|-----------|----------------|
| **Single** | Minimum distance between points | Tends to chain (elongated clusters) |
| **Complete** | Maximum distance between points | Compact, spherical clusters |
| **Average** | Average of all pairwise distances | Compromise between single/complete |
| **Ward's** | Minimizes within-cluster variance | Tends to create equal-sized clusters |

**Dendrogram:**
```
Height
  │
5 ├─────────────────────────┐
  │                         │
4 ├───────────────┐         │
  │               │         │
3 ├─────┐         │         │
  │     │         │         │
2 ├──┐  │    ┌────┤         │
  │  │  │    │    │         │
1 ├──┤  │    │    │    ┌────┤
  │  A  B    C    D    E    F
  └──────────────────────────────
```
Cut at desired height to get K clusters.

**Characteristics:**
1. **No need to specify K** – Choose by cutting dendrogram at appropriate level
2. **Deterministic** – Same input always gives same result
3. **Dendrogram provides visualization** – Shows cluster hierarchy
4. **Captures nested structures** – Multi-level groupings
5. **No reassignment** – Once merged/split, cannot undo (greedy)
6. **Computational cost** – O(n³) time, O(n²) space

**Comparison: Partition vs Hierarchical:**

| Aspect | K-Means (Partition) | Hierarchical |
|--------|--------------------:|:-------------|
| K required | Yes (upfront) | No (choose later) |
| Complexity | O(nKt) | O(n²) to O(n³) |
| Scalability | Large datasets | Small-medium datasets |
| Result | Flat clusters | Nested hierarchy |
| Reassignment | Yes (iterative) | No (greedy) |

---

## 11. Evaluation of Clustering Models

**Challenge:** No ground truth labels available (unsupervised), so evaluation is complex.

**Internal Evaluation (No labels needed):**

**1. Silhouette Coefficient:**
```
s(i) = [b(i) - a(i)] / max(a(i), b(i))

a(i) = avg distance to points in same cluster
b(i) = avg distance to points in nearest other cluster

Range: [-1, +1]
+1 = Well clustered
 0 = On cluster boundary
-1 = Misclassified
```

**2. Within-Cluster Sum of Squares (WSS/Inertia):**
```
WSS = ΣΣ ||xᵢ - cⱼ||²

Lower WSS = tighter clusters
Used in Elbow method
```

**3. Dunn Index:**
```
DI = min(inter-cluster distance) / max(intra-cluster diameter)

Higher = better (compact and well-separated)
```

**External Evaluation (When labels available):**
- **Rand Index** – Proportion of correct decisions
- **Adjusted Rand Index** – Corrected for chance
- **Normalized Mutual Information (NMI)**

---

---

# PART D: ASSOCIATION RULES

---

## 12. Structure of Association Rule

**Definition:** Association Rule Mining discovers interesting relationships (associations) between variables in large datasets, commonly used in market basket analysis.

**Structure:**
```
IF {Antecedent} THEN {Consequent}
X → Y

Example: {Bread, Butter} → {Milk}
"Customers who buy Bread and Butter also buy Milk"
```

**Key Measures:**

**1. Support:**
```
Support(X→Y) = Count(X ∪ Y) / Total Transactions
             = P(X ∩ Y)

"How frequently do X and Y appear together?"
```

**2. Confidence:**
```
Confidence(X→Y) = Support(X ∪ Y) / Support(X)
                = P(Y|X)

"Given X is bought, how likely is Y also bought?"
```

**3. Lift:**
```
Lift(X→Y) = Confidence(X→Y) / Support(Y)
           = P(X ∩ Y) / [P(X) × P(Y)]

Lift > 1: Positive correlation (X promotes Y)
Lift = 1: Independent (no relationship)
Lift < 1: Negative correlation (X discourages Y)
```

**Example:**
```
1000 transactions total
- 200 contain Bread
- 300 contain Milk
- 150 contain both Bread and Milk

Support(Bread→Milk) = 150/1000 = 15%
Confidence(Bread→Milk) = 150/200 = 75%
Lift(Bread→Milk) = 0.75/0.30 = 2.5 (strong positive association)
```

---

## 13. Apriori Algorithm

### 📝 PYQ: Q6 b) Discuss the principles underlying the Apriori Algorithm for association rule mining [5 Marks]

**Answer:**

**Definition:** Apriori is an algorithm for mining frequent itemsets and generating association rules. It uses a "bottom-up" approach where frequent subsets are extended one item at a time (candidate generation).

**Apriori Principle (Anti-Monotone Property):**
> "If an itemset is infrequent, then all its supersets must also be infrequent."

Conversely: "All subsets of a frequent itemset must also be frequent."

**This principle enables pruning** – if {A,B} is infrequent, we don't need to check {A,B,C}, {A,B,D}, etc.

**Algorithm Steps:**

```
Input: Transaction database D, minimum support threshold (min_sup)
Output: All frequent itemsets

Step 1: Scan DB → Find all frequent 1-itemsets (L₁)
Step 2: k = 2
Step 3: REPEAT
    a) Generate candidate k-itemsets (Cₖ) from Lₖ₋₁ (JOIN step)
    b) Prune candidates using Apriori principle (PRUNE step)
    c) Scan DB → Count support of each candidate
    d) Lₖ = candidates with support ≥ min_sup
    e) k = k + 1
Step 4: UNTIL no new frequent itemsets found
Step 5: Generate rules from frequent itemsets with confidence ≥ min_conf
```

**Key Principles:**

1. **Downward Closure Property:** Subsets of frequent sets are frequent
2. **Candidate Generation:** Only combine itemsets that share (k-1) items
3. **Pruning:** Eliminate candidates with infrequent subsets before counting
4. **Level-wise Search:** Explore k-itemsets only after finding all (k-1)-itemsets
5. **Multiple DB Scans:** One scan per level (k)

---

### 📝 PYQ: Q5 a) Apriori Algorithm Numerical Problem [9 Marks]

**Problem:** Find frequent itemsets and generate association rules.
- Minimum support count = 2
- Minimum confidence = 60%

**Transaction Database:**

| TID | Items |
|-----|-------|
| T1 | I1, I2, I5 |
| T2 | I2, I4 |
| T3 | I2, I3 |
| T4 | I1, I2, I4 |
| T5 | I1, I3 |
| T6 | I2, I3 |
| T7 | I1, I3 |
| T8 | I1, I2, I3, I5 |
| T9 | I1, I2, I3 |

**Total Transactions = 9, Min Support Count = 2**

---

**Step 1: Find Frequent 1-Itemsets (L₁)**

Scan database and count each item:

| Itemset | Support Count | Frequent? (≥2) |
|---------|:------------:|:---:|
| {I1} | 6 | ✓ |
| {I2} | 7 | ✓ |
| {I3} | 6 | ✓ |
| {I4} | 2 | ✓ |
| {I5} | 2 | ✓ |

**L₁ = {{I1}, {I2}, {I3}, {I4}, {I5}}** — All items are frequent.

---

**Step 2: Generate Candidate 2-Itemsets (C₂) and Find L₂**

Generate all pairs from L₁ and count support:

| Itemset | Transactions | Support Count | Frequent? |
|---------|-------------|:---:|:---:|
| {I1, I2} | T1, T4, T8, T9 | 4 | ✓ |
| {I1, I3} | T5, T7, T8, T9 | 4 | ✓ |
| {I1, I4} | T4 | 1 | ✗ |
| {I1, I5} | T1, T8 | 2 | ✓ |
| {I2, I3} | T3, T6, T8, T9 | 4 | ✓ |
| {I2, I4} | T2, T4 | 2 | ✓ |
| {I2, I5} | T1, T8 | 2 | ✓ |
| {I3, I4} | — | 0 | ✗ |
| {I3, I5} | T8 | 1 | ✗ |
| {I4, I5} | — | 0 | ✗ |

**L₂ = {{I1,I2}, {I1,I3}, {I1,I5}, {I2,I3}, {I2,I4}, {I2,I5}}**

---

**Step 3: Generate Candidate 3-Itemsets (C₃) and Find L₃**

Join L₂ with itself (pairs sharing first item):
- {I1,I2} ∪ {I1,I3} → {I1,I2,I3} — Check subsets: {I1,I2}✓, {I1,I3}✓, {I2,I3}✓ → Keep
- {I1,I2} ∪ {I1,I5} → {I1,I2,I5} — Check subsets: {I1,I2}✓, {I1,I5}✓, {I2,I5}✓ → Keep
- {I1,I3} ∪ {I1,I5} → {I1,I3,I5} — Check subsets: {I3,I5}✗ → **PRUNE**
- {I2,I3} ∪ {I2,I4} → {I2,I3,I4} — Check subsets: {I3,I4}✗ → **PRUNE**
- {I2,I3} ∪ {I2,I5} → {I2,I3,I5} — Check subsets: {I3,I5}✗ → **PRUNE**
- {I2,I4} ∪ {I2,I5} → {I2,I4,I5} — Check subsets: {I4,I5}✗ → **PRUNE**

**C₃ = {{I1,I2,I3}, {I1,I2,I5}}**

Count support:

| Itemset | Transactions | Support Count | Frequent? |
|---------|-------------|:---:|:---:|
| {I1, I2, I3} | T8, T9 | 2 | ✓ |
| {I1, I2, I5} | T1, T8 | 2 | ✓ |

**L₃ = {{I1,I2,I3}, {I1,I2,I5}}**

---

**Step 4: Generate Candidate 4-Itemsets (C₄)**

Join: {I1,I2,I3} ∪ {I1,I2,I5} → {I1,I2,I3,I5}
Check subset {I2,I3,I5}: Not in L₃ → **PRUNE**

**C₄ = ∅ → STOP**

---

**Step 5: Generate Association Rules (Min Confidence = 60%)**

From frequent itemsets, generate rules:

**From {I1, I2} (support = 4):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I1→I2 | 4/6 = 66.7% | ✓ |
| I2→I1 | 4/7 = 57.1% | ✗ |

**From {I1, I3} (support = 4):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I1→I3 | 4/6 = 66.7% | ✓ |
| I3→I1 | 4/6 = 66.7% | ✓ |

**From {I1, I5} (support = 2):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I1→I5 | 2/6 = 33.3% | ✗ |
| I5→I1 | 2/2 = 100% | ✓ |

**From {I2, I3} (support = 4):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I2→I3 | 4/7 = 57.1% | ✗ |
| I3→I2 | 4/6 = 66.7% | ✓ |

**From {I2, I4} (support = 2):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I2→I4 | 2/7 = 28.6% | ✗ |
| I4→I2 | 2/2 = 100% | ✓ |

**From {I2, I5} (support = 2):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| I2→I5 | 2/7 = 28.6% | ✗ |
| I5→I2 | 2/2 = 100% | ✓ |

**From {I1, I2, I3} (support = 2):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| {I1,I2}→I3 | 2/4 = 50% | ✗ |
| {I1,I3}→I2 | 2/4 = 50% | ✗ |
| {I2,I3}→I1 | 2/4 = 50% | ✗ |
| I1→{I2,I3} | 2/6 = 33.3% | ✗ |
| I2→{I1,I3} | 2/7 = 28.6% | ✗ |
| I3→{I1,I2} | 2/6 = 33.3% | ✗ |

**From {I1, I2, I5} (support = 2):**

| Rule | Confidence | Valid? |
|------|:---:|:---:|
| {I1,I2}→I5 | 2/4 = 50% | ✗ |
| {I1,I5}→I2 | 2/2 = 100% | ✓ |
| {I2,I5}→I1 | 2/2 = 100% | ✓ |
| I1→{I2,I5} | 2/6 = 33.3% | ✗ |
| I2→{I1,I5} | 2/7 = 28.6% | ✗ |
| I5→{I1,I2} | 2/2 = 100% | ✓ |

---

**Final Strong Association Rules (Confidence ≥ 60%):**

| # | Rule | Support | Confidence |
|---|------|:---:|:---:|
| 1 | I1 → I2 | 4/9 | 66.7% |
| 2 | I1 → I3 | 4/9 | 66.7% |
| 3 | I3 → I1 | 4/9 | 66.7% |
| 4 | I5 → I1 | 2/9 | 100% |
| 5 | I3 → I2 | 4/9 | 66.7% |
| 6 | I4 → I2 | 2/9 | 100% |
| 7 | I5 → I2 | 2/9 | 100% |
| 8 | {I1,I5} → I2 | 2/9 | 100% |
| 9 | {I2,I5} → I1 | 2/9 | 100% |
| 10 | I5 → {I1,I2} | 2/9 | 100% |

**Business Interpretation:**
- Customers who buy I5 ALWAYS buy I1 and I2 (100% confidence)
- Customers who buy I4 ALWAYS buy I2 (100% confidence)
- I1 and I3 are frequently bought together (mutual 66.7% confidence)

---

---

# QUICK REVISION - Key Points for Exam

| Topic | Key Point to Remember |
|-------|----------------------|
| Linear Regression | Y = β₀ + β₁X, minimizes sum of squared errors |
| R² | 1 - (SS_res/SS_tot), proportion of variance explained |
| Logistic Regression | σ(z) = 1/(1+e⁻ᶻ), predicts probability [0,1] |
| Naive Bayes | P(C|X) ∝ P(C)×∏P(Xᵢ|C), assumes independence |
| K-Means | Assign to nearest centroid, recalculate, repeat |
| Hierarchical | Agglomerative (bottom-up), Divisive (top-down) |
| Silhouette | (b-a)/max(a,b), range [-1,+1] |
| Support | P(X∩Y) = count(X∪Y)/total transactions |
| Confidence | P(Y|X) = support(X∪Y)/support(X) |
| Lift | confidence/support(Y), >1 means positive association |
| Apriori Principle | Subsets of frequent sets are frequent |
| Confusion Matrix | TP, TN, FP, FN → Accuracy, Precision, Recall, F1 |

---

# FORMULAS TO REMEMBER

```
Linear Regression:
  Y = β₀ + β₁X
  β₁ = [nΣxy - ΣxΣy] / [nΣx² - (Σx)²]
  β₀ = ȳ - β₁x̄

Evaluation:
  MAE = (1/n)Σ|y - ŷ|
  MSE = (1/n)Σ(y - ŷ)²
  RMSE = √MSE
  R² = 1 - (SS_res/SS_tot)

Logistic Regression:
  P = 1/(1 + e^-z)
  z = β₀ + β₁X₁ + β₂X₂ + ...

Classification:
  Accuracy = (TP+TN)/(TP+TN+FP+FN)
  Precision = TP/(TP+FP)
  Recall = TP/(TP+FN)
  F1 = 2×P×R/(P+R)

Bayes:
  P(C|X) = P(X|C)×P(C) / P(X)

Association Rules:
  Support(X→Y) = count(X∪Y) / N
  Confidence(X→Y) = support(X∪Y) / support(X)
  Lift(X→Y) = confidence(X→Y) / support(Y)

Clustering:
  Silhouette = (b-a)/max(a,b)
  Euclidean distance = √[Σ(pᵢ-qᵢ)²]
```

---

*Prepared for SPPU BE IT/CS - Business Intelligence End Semester Examination*
