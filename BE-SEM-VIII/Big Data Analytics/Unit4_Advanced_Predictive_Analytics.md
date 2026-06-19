# Unit IV: Advanced Predictive Analytics Algorithms and Python

---

## Section 1: Exploratory Data Analytics (EDA) (Covers Q3a, Q3b)

### PYQs Covered:
- **Q3a)** Explain Data Sourcing, Data Cleaning, Univariate analysis, Bivariate/Multivariate analysis [10 Marks]
- **Q3b)** What are the essential steps in data exploration and how do they contribute to uncovering insights? [7 Marks]

---

### 1.1 Definition of EDA

**Exploratory Data Analysis (EDA)** is an approach to analyzing datasets to summarize their main characteristics, often using visual methods, before applying formal modeling or hypothesis testing.

**Purpose:** Understand data structure, detect patterns, spot anomalies, check assumptions, and formulate hypotheses.

**Coined by:** John Tukey (1977)

---

### 1.2 Motivation for EDA

1. **Understand data quality** – Identify missing values, errors, inconsistencies
2. **Discover patterns** – Find trends, relationships, groupings
3. **Detect outliers** – Spot unusual observations that may affect analysis
4. **Validate assumptions** – Check if data meets requirements for planned models
5. **Feature selection** – Identify which variables are most important
6. **Guide modeling** – Inform choice of appropriate algorithms
7. **Communicate findings** – Create visualizations for stakeholders

---

### 1.3 Data Types in EDA

| Category | Type | Description | Example |
|----------|------|-------------|---------|
| **Quantitative** | Discrete | Countable values | No. of students, items sold |
| **Quantitative** | Continuous | Measurable, any value in range | Height, weight, temperature |
| **Qualitative** | Nominal | Categories without order | Color, city, gender |
| **Qualitative** | Ordinal | Categories with meaningful order | Rating (poor/good/excellent) |
| **Qualitative** | Binary | Only two categories | Yes/No, 0/1, True/False |

---

### 1.4 Steps in Data Exploration (Covers Q3b)

```
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│    1.      │   │    2.      │   │    3.      │   │    4.      │   │    5.      │
│   Data     │──▶│   Data     │──▶│ Univariate │──▶│ Bivariate/ │──▶│  Feature   │
│  Sourcing  │   │  Cleaning  │   │  Analysis  │   │Multivariate│   │Engineering │
└────────────┘   └────────────┘   └────────────┘   └────────────┘   └────────────┘
                                                                           │
                                                                           ▼
                                                                    ┌────────────┐
                                                                    │    6.      │
                                                                    │ Insights & │
                                                                    │ Hypothesis │
                                                                    └────────────┘
```

**Step 1: Data Sourcing**
- **Contribution:** Provides the raw material for analysis; quality of source determines quality of insights.

**Step 2: Data Cleaning**
- **Contribution:** Ensures reliability; removes noise that could lead to false patterns.

**Step 3: Univariate Analysis**
- **Contribution:** Understands individual variable distributions; identifies outliers and data issues per variable.

**Step 4: Bivariate/Multivariate Analysis**
- **Contribution:** Reveals relationships between variables; uncovers hidden patterns and correlations.

**Step 5: Feature Engineering & Transformation**
- **Contribution:** Creates meaningful features; improves model performance by extracting relevant information.

**Step 6: Insights & Hypothesis Generation**
- **Contribution:** Formulates testable hypotheses; guides next steps in analysis/modeling.

---

### 1.5 Detailed Explanation of EDA Terms (Covers Q3a)

---

#### i) Data Sourcing

**Definition:** Data sourcing is the process of identifying, collecting, and acquiring data from various internal and external sources for analysis.

**Types of Data Sources:**

| Source Type | Examples |
|-------------|----------|
| **Internal** | Company databases, CRM systems, ERP, transaction logs |
| **External** | Government data, APIs, web scraping, third-party vendors |
| **Primary** | Surveys, experiments, interviews (collected firsthand) |
| **Secondary** | Census data, research papers, public datasets |

**Key Considerations:**
- Data relevance to the problem
- Data quality and reliability
- Data freshness (how recent)
- Legal/ethical permissions
- Format compatibility
- Volume and completeness

**Common Data Formats:**
- Structured: CSV, Excel, SQL databases
- Semi-structured: JSON, XML, logs
- Unstructured: Text, images, audio, video

**In Python:**
```python
import pandas as pd

# From CSV
df = pd.read_csv("data.csv")

# From Database
import sqlalchemy
engine = sqlalchemy.create_engine("mysql://user:pass@host/db")
df = pd.read_sql("SELECT * FROM students", engine)

# From API
import requests
response = requests.get("https://api.example.com/data")
data = response.json()
```

---

#### ii) Data Cleaning

**Definition:** Data cleaning (data cleansing/scrubbing) is the process of detecting and correcting (or removing) corrupt, inaccurate, or irrelevant records from a dataset.

**Common Data Quality Issues:**

| Issue | Detection | Treatment |
|-------|-----------|-----------|
| **Missing Values** | `df.isnull().sum()` | Imputation (mean/median/mode), deletion |
| **Duplicates** | `df.duplicated().sum()` | `df.drop_duplicates()` |
| **Outliers** | Box plot, IQR, Z-score | Cap, transform, or remove |
| **Inconsistencies** | Value counts, unique() | Standardize (e.g., "M"→"Male") |
| **Wrong Data Types** | `df.dtypes` | Type casting (`astype()`) |
| **Invalid Values** | Domain knowledge check | Replace or remove |

**Data Cleaning in Python:**
```python
# Check missing values
df.isnull().sum()

# Fill missing values
df['age'].fillna(df['age'].median(), inplace=True)

# Remove duplicates
df.drop_duplicates(inplace=True)

# Fix inconsistencies
df['gender'] = df['gender'].str.lower().replace({'m': 'male', 'f': 'female'})

# Remove outliers (IQR method)
Q1 = df['salary'].quantile(0.25)
Q3 = df['salary'].quantile(0.75)
IQR = Q3 - Q1
df = df[(df['salary'] >= Q1 - 1.5*IQR) & (df['salary'] <= Q3 + 1.5*IQR)]

# Convert data types
df['date'] = pd.to_datetime(df['date'])
df['category'] = df['category'].astype('category')
```

---

#### iii) Univariate Analysis

**Definition:** Univariate analysis examines each variable individually to understand its distribution, central tendency, and spread.

**For Continuous Variables:**

| Measure | Purpose | Python |
|---------|---------|--------|
| Mean | Central tendency | `df['col'].mean()` |
| Median | Central tendency (robust) | `df['col'].median()` |
| Mode | Most frequent value | `df['col'].mode()` |
| Std Dev | Spread/dispersion | `df['col'].std()` |
| Skewness | Asymmetry of distribution | `df['col'].skew()` |
| Kurtosis | Tail heaviness | `df['col'].kurtosis()` |

**Visualizations for Continuous:**
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Histogram - Distribution shape
plt.hist(df['marks'], bins=20, edgecolor='black')
plt.title('Distribution of Marks')
plt.show()

# Box Plot - Median, quartiles, outliers
sns.boxplot(x=df['salary'])

# Density Plot - Smooth distribution
sns.kdeplot(df['age'])
```

**For Categorical Variables:**

| Measure | Purpose | Python |
|---------|---------|--------|
| Frequency | Count per category | `df['col'].value_counts()` |
| Proportion | Percentage per category | `df['col'].value_counts(normalize=True)` |

**Visualizations for Categorical:**
```python
# Bar Chart
df['department'].value_counts().plot(kind='bar')

# Pie Chart
df['gender'].value_counts().plot(kind='pie', autopct='%1.1f%%')
```

**Insights from Univariate:**
- Shape of distribution (normal, skewed, bimodal)
- Presence of outliers
- Missing value patterns
- Range and spread of data

---

#### iv) Bivariate/Multivariate Analysis

**Definition:** Bivariate analysis examines the relationship between two variables. Multivariate analysis examines relationships among three or more variables simultaneously.

**Types of Bivariate Relationships:**

| Variable 1 | Variable 2 | Technique | Visualization |
|-----------|-----------|-----------|---------------|
| Continuous | Continuous | Correlation, Regression | Scatter plot, Heatmap |
| Continuous | Categorical | T-test, ANOVA | Box plot, Violin plot |
| Categorical | Categorical | Chi-square test | Stacked bar, Heatmap |

**Bivariate Analysis in Python:**
```python
# Continuous vs Continuous - Correlation
correlation = df['hours'].corr(df['marks'])
print(f"Correlation: {correlation}")

# Scatter plot
plt.scatter(df['hours'], df['marks'])
plt.xlabel('Hours Studied')
plt.ylabel('Marks')
plt.show()

# Correlation Heatmap (multiple variables)
sns.heatmap(df[['hours', 'marks', 'attendance']].corr(), 
            annot=True, cmap='coolwarm')

# Continuous vs Categorical - Box plot
sns.boxplot(x='department', y='salary', data=df)

# Categorical vs Categorical - Cross tabulation
pd.crosstab(df['gender'], df['passed'], normalize='index')
```

**Multivariate Analysis:**
```python
# Pair Plot - All pairwise relationships
sns.pairplot(df[['age', 'income', 'spending']], hue='cluster')

# Parallel Coordinates
from pandas.plotting import parallel_coordinates
parallel_coordinates(df, 'class', cols=['x1','x2','x3','x4'])

# 3D Scatter
from mpl_toolkits.mplot3d import Axes3D
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.scatter(df['x'], df['y'], df['z'], c=df['label'])
```

**Insights from Bivariate/Multivariate:**
- Correlation strength and direction between variables
- Which features influence the target variable
- Interaction effects between variables
- Clusters or groupings in multi-dimensional space
- Multicollinearity among predictors

---

## Section 2: Ensemble Methods (Covers Q4a)

### PYQ Covered:
- **Q4a)** How do ensemble methods (bagging, boosting, AdaBoost, Random Forest) improve classification accuracy? [10 Marks]

---

### 2.1 What are Ensemble Methods?

**Definition:** Ensemble methods combine multiple base/weak learners to create a stronger, more accurate model. The idea is that a group of weak models together can outperform a single strong model.

**Principle:** "Wisdom of the crowd" – multiple opinions reduce individual errors.

**Why Ensemble Methods Improve Accuracy:**
1. **Reduce Variance** – Averaging multiple models smooths out noise (Bagging)
2. **Reduce Bias** – Sequentially correcting errors improves underfitting models (Boosting)
3. **Improve Stability** – Less sensitive to training data variations
4. **Handle Complex Patterns** – Different models capture different aspects of data

**Types of Ensemble Methods:**
```
Ensemble Methods
├── Bagging (Parallel) ──── Random Forest
└── Boosting (Sequential) ── AdaBoost, Gradient Boosting, XGBoost
```

---

### 2.2 Bagging (Bootstrap Aggregating)

**Concept:** Train multiple models independently on different random subsets of training data, then combine predictions by voting (classification) or averaging (regression).

**Algorithm:**
1. From original dataset of size N, create M bootstrap samples (random sampling with replacement, each of size N)
2. Train a base model (e.g., Decision Tree) on each bootstrap sample independently
3. For prediction:
   - **Classification:** Majority voting among all M models
   - **Regression:** Average of all M model predictions

**Diagram:**
```
Original Dataset
       │
   ┌───┼───┬───────┐
   ▼   ▼   ▼       ▼
┌─────┐┌─────┐  ┌─────┐
│Boot ││Boot │  │Boot │
│Samp1││Samp2│..│SampM│  (Bootstrap Sampling with Replacement)
└──┬──┘└──┬──┘  └──┬──┘
   ▼      ▼        ▼
┌─────┐┌─────┐  ┌─────┐
│Model││Model│  │Model│  (Independent Training)
│  1  ││  2  │..│  M  │
└──┬──┘└──┬──┘  └──┬──┘
   │      │        │
   └──────┼────────┘
          ▼
   ┌─────────────┐
   │  Majority   │   (Aggregation)
   │  Voting /   │
   │  Averaging  │
   └─────────────┘
          ▼
     Final Prediction
```

**How Bagging Improves Accuracy:**
- Reduces **overfitting** by averaging out noise from individual models
- Reduces **variance** without increasing bias
- Each model sees different data → different errors → errors cancel out
- Particularly effective with high-variance models (Decision Trees)

**In Python:**
```python
from sklearn.ensemble import BaggingClassifier
from sklearn.tree import DecisionTreeClassifier

model = BaggingClassifier(
    estimator=DecisionTreeClassifier(),
    n_estimators=100,
    max_samples=0.8,
    bootstrap=True
)
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)
```

---

### 2.3 Boosting

**Concept:** Train models sequentially, where each new model focuses on correcting the errors made by previous models. Data points misclassified earlier get higher weights.

**Algorithm:**
1. Train first model on original data
2. Identify misclassified examples
3. Increase weight of misclassified examples
4. Train next model on reweighted data (focuses on hard cases)
5. Repeat for M iterations
6. Final prediction: Weighted combination of all models

**Diagram:**
```
┌─────────┐    ┌─────────┐    ┌─────────┐
│ Model 1 │───▶│ Model 2 │───▶│ Model 3 │───▶ ...
└────┬────┘    └────┬────┘    └────┬────┘
     │              │              │
     ▼              ▼              ▼
 Errors from    Errors from    Errors from
 Model 1 get   Model 2 get    Model 3 get
 higher weight  higher weight   higher weight
     │              │              │
     └──────────────┼──────────────┘
                    ▼
          Weighted Combination
                    ▼
            Final Prediction
```

**How Boosting Improves Accuracy:**
- Reduces **bias** by focusing on hard-to-classify examples
- Converts weak learners (slightly better than random) into strong learner
- Each iteration specifically targets previous mistakes
- Achieves low training error with simple base models

---

### 2.4 AdaBoost (Adaptive Boosting)

**Concept:** A specific boosting algorithm that adaptively adjusts weights of training instances and combines weak classifiers with weighted voting.

**Algorithm (Detailed):**

1. **Initialize** weights: wᵢ = 1/N for all N training samples
2. **For each iteration t = 1 to T:**
   - Train weak classifier hₜ on weighted data
   - Calculate weighted error: εₜ = Σ wᵢ × I(hₜ(xᵢ) ≠ yᵢ) (sum over misclassified)
   - Calculate classifier weight: αₜ = ½ × ln((1 - εₜ) / εₜ)
   - Update sample weights:
     - Misclassified: wᵢ = wᵢ × e^(αₜ) (increase)
     - Correctly classified: wᵢ = wᵢ × e^(-αₜ) (decrease)
   - Normalize weights so they sum to 1
3. **Final prediction:** H(x) = sign(Σ αₜ × hₜ(x))

**Key Insight:** 
- αₜ is higher for more accurate classifiers → better models get more say
- Misclassified samples get exponentially higher weights → next model focuses on them

**Example:**
```
Iteration 1: Simple stump classifies 80% correctly
  → Weight α₁ = 0.69 (good, gets decent weight)
  → Misclassified 20% get higher weights

Iteration 2: New stump focuses on previously misclassified
  → Gets those right but misses others
  → Weight α₂ = 0.55

Final: α₁×h₁(x) + α₂×h₂(x) + ... → Majority wins
```

**In Python:**
```python
from sklearn.ensemble import AdaBoostClassifier
from sklearn.tree import DecisionTreeClassifier

model = AdaBoostClassifier(
    estimator=DecisionTreeClassifier(max_depth=1),  # Weak learner (stump)
    n_estimators=50,
    learning_rate=1.0
)
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)
```

---

### 2.5 Random Forest

**Concept:** An ensemble of Decision Trees trained using both bagging (bootstrap samples) AND random feature selection at each split, making trees more diverse.

**Algorithm:**
1. Create M bootstrap samples from training data
2. For each sample, grow a Decision Tree:
   - At each node, randomly select k features (k << total features)
   - Find best split among those k features only
   - Grow tree fully (no pruning)
3. For prediction:
   - Each tree votes
   - Final answer = majority vote (classification) or average (regression)

**Key Difference from Bagging:**
- Bagging: Uses all features at each split
- Random Forest: Uses random subset of features at each split → more diverse trees

**Diagram:**
```
Original Dataset (N samples, P features)
       │
   ┌───┼───┬───────┐
   ▼   ▼   ▼       ▼
┌─────┐┌─────┐  ┌─────┐
│Boot ││Boot │  │Boot │   Bootstrap Samples
│Samp1││Samp2│..│SampM│
└──┬──┘└──┬──┘  └──┬──┘
   ▼      ▼        ▼
┌─────┐┌─────┐  ┌─────┐
│Tree1││Tree2│  │TreeM│   Each tree uses random √P features
│√P   ││√P   │..│√P   │   at each split
│feat ││feat │  │feat │
└──┬──┘└──┬──┘  └──┬──┘
   │      │        │
   └──────┼────────┘
          ▼
   Majority Voting
          ▼
   Final Prediction
```

**How Random Forest Improves Accuracy:**
1. **Decorrelates trees** – Random feature selection ensures trees are different
2. **Reduces overfitting** – Averaging many trees reduces variance
3. **Handles high-dimensional data** – Feature subsampling works well with many features
4. **Robust to outliers** – Majority voting reduces impact of anomalies
5. **No pruning needed** – Overfitting handled by ensemble averaging

**Hyperparameters:**
- `n_estimators` – Number of trees (more = better, but diminishing returns)
- `max_features` – Features per split (√P for classification, P/3 for regression)
- `max_depth` – Tree depth (None = fully grown)

**In Python:**
```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(
    n_estimators=100,
    max_features='sqrt',
    random_state=42
)
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)

# Feature Importance
importances = model.feature_importances_
```

---

### 2.6 Comparison of Ensemble Methods

| Feature | Bagging | Boosting/AdaBoost | Random Forest |
|---------|---------|-------------------|---------------|
| Training | Parallel | Sequential | Parallel |
| Focus | Reduce variance | Reduce bias | Reduce variance |
| Base Learner | Any (usually trees) | Weak learners (stumps) | Decision Trees |
| Data Sampling | Bootstrap (rows) | Reweighting | Bootstrap (rows) |
| Feature Sampling | All features | All features | Random subset |
| Overfitting Risk | Low | Medium (can overfit noise) | Very Low |
| Sensitivity to Outliers | Low | High | Low |
| Typical Accuracy | Good | Very Good | Very Good |

---

## Section 3: Model Evaluation and Selection (Covers Q4b)

### PYQ Covered:
- **Q4b)** How does the confusion matrix aid in model evaluation and selection? [7 Marks]

---

### 3.1 Confusion Matrix

**Definition:** A confusion matrix is a table that summarizes the performance of a classification model by comparing predicted labels against actual labels.

**For Binary Classification:**

```
                      PREDICTED
                 Positive    Negative
              ┌───────────┬───────────┐
   Positive   │    TP     │    FN     │
ACTUAL        │ (True     │ (False    │
              │ Positive) │ Negative) │
              ├───────────┼───────────┤
   Negative   │    FP     │    TN     │
              │ (False    │ (True     │
              │ Positive) │ Negative) │
              └───────────┴───────────┘
```

| Term | Meaning | Example (Spam Detection) |
|------|---------|--------------------------|
| **TP (True Positive)** | Correctly predicted positive | Spam correctly identified as spam |
| **TN (True Negative)** | Correctly predicted negative | Ham correctly identified as ham |
| **FP (False Positive)** | Incorrectly predicted positive (Type I Error) | Ham wrongly marked as spam |
| **FN (False Negative)** | Incorrectly predicted negative (Type II Error) | Spam wrongly marked as ham |

---

**Metrics Derived from Confusion Matrix:**

| Metric | Formula | Interpretation |
|--------|---------|---------------|
| **Accuracy** | (TP+TN) / (TP+TN+FP+FN) | Overall correctness |
| **Precision** | TP / (TP+FP) | Of predicted positives, how many are correct? |
| **Recall (Sensitivity/TPR)** | TP / (TP+FN) | Of actual positives, how many were found? |
| **Specificity (TNR)** | TN / (TN+FP) | Of actual negatives, how many correctly identified? |
| **F1-Score** | 2×(Precision×Recall)/(Precision+Recall) | Harmonic mean of precision & recall |
| **FPR (False Positive Rate)** | FP / (FP+TN) | Rate of false alarms |

---

**How Confusion Matrix Aids in Model Evaluation and Selection:**

**1. Beyond Accuracy:**
- Accuracy alone is misleading for imbalanced datasets
- Example: 95% non-fraud, 5% fraud → model predicting all "non-fraud" gets 95% accuracy but catches 0 fraud
- Confusion matrix reveals this: TP=0, FN=all frauds

**2. Error Type Analysis:**
- Identifies which types of errors the model makes
- In medical diagnosis: FN (missing disease) is worse than FP (false alarm)
- In spam: FP (blocking legitimate email) is worse than FN (letting spam through)
- Choose model based on which error type is more costly

**3. Model Comparison:**
- Compare multiple models side-by-side using confusion matrices
- Select model with best trade-off for the specific use case

**4. Threshold Tuning:**
- Adjusting classification threshold changes TP/FP trade-off
- ROC curve plots TPR vs FPR at different thresholds
- AUC (Area Under Curve) summarizes overall performance

**5. Class-wise Performance:**
- For multiclass: shows which classes are confused with each other
- Helps identify where model needs improvement

**Example:**

```python
from sklearn.metrics import confusion_matrix, classification_report

y_pred = model.predict(X_test)

# Confusion Matrix
cm = confusion_matrix(y_test, y_pred)
print(cm)
# [[85, 5],    → TN=85, FP=5
#  [10, 50]]   → FN=10, TP=50

# Detailed Report
print(classification_report(y_test, y_pred))
# Precision, Recall, F1 for each class

# Visualization
import seaborn as sns
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Negative','Positive'],
            yticklabels=['Negative','Positive'])
plt.xlabel('Predicted')
plt.ylabel('Actual')
plt.show()
```

**Numerical Example:**
```
Model A:              Model B:
     Pred              Pred
     P    N            P    N
A P [45   5]     A P [48   2]
  N [15  35]       N [20  30]

Model A: Accuracy = 80/100 = 80%, Precision = 45/60 = 75%, Recall = 45/50 = 90%
Model B: Accuracy = 78/100 = 78%, Precision = 48/68 = 71%, Recall = 48/50 = 96%

→ If recall is priority (e.g., disease detection): Choose Model B
→ If precision is priority (e.g., spam): Choose Model A
```

---

### 3.2 Dataset Partitioning Methods

---

#### 3.2.1 Holdout Method

**Concept:** Split dataset into two parts – training set and testing set (typically 70-30 or 80-20).

```
┌──────────────────────────────────────────────────┐
│              Original Dataset (100%)              │
├─────────────────────────────────┬────────────────┤
│     Training Set (70%)          │  Test Set (30%)│
│     (Build Model)               │  (Evaluate)    │
└─────────────────────────────────┴────────────────┘
```

**Process:**
1. Randomly split data into training (70%) and testing (30%)
2. Train model on training set
3. Evaluate on test set (never seen during training)

**Advantages:**
- Simple and fast
- Easy to implement

**Disadvantages:**
- Results depend on random split
- May not represent data well if small dataset
- Wastes data (30% not used for training)

**In Python:**
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)
```

---

#### 3.2.2 Random Subsampling

**Concept:** Repeat the holdout method multiple times with different random splits and average the results.

```
Split 1: Train(70%) | Test(30%) → Accuracy₁ = 82%
Split 2: Train(70%) | Test(30%) → Accuracy₂ = 85%
Split 3: Train(70%) | Test(30%) → Accuracy₃ = 80%
...
Split K: Train(70%) | Test(30%) → Accuracyₖ = 83%

Final Accuracy = Average(Accuracy₁, ..., Accuracyₖ) = 82.5%
```

**Advantages:**
- More reliable than single holdout
- Reduces impact of lucky/unlucky splits

**Disadvantages:**
- Some samples may never be in test set
- Some samples may appear in test set multiple times
- No guarantee every sample is tested

---

#### 3.2.3 Cross-Validation (K-Fold)

**Concept:** Divide data into K equal parts (folds). Use K-1 folds for training and 1 fold for testing. Repeat K times, each time using a different fold as test set.

**K-Fold Cross-Validation (K=5):**

```
Fold 1: [TEST] [Train] [Train] [Train] [Train] → Acc₁
Fold 2: [Train] [TEST] [Train] [Train] [Train] → Acc₂
Fold 3: [Train] [Train] [TEST] [Train] [Train] → Acc₃
Fold 4: [Train] [Train] [Train] [TEST] [Train] → Acc₄
Fold 5: [Train] [Train] [Train] [Train] [TEST] → Acc₅

Final Accuracy = (Acc₁ + Acc₂ + Acc₃ + Acc₄ + Acc₅) / 5
```

**Types:**
- **K-Fold CV** – Standard (K=5 or K=10 typical)
- **Stratified K-Fold** – Maintains class distribution in each fold
- **Leave-One-Out CV (LOOCV)** – K = N (each sample is test once); expensive but maximum data usage

**Advantages:**
- Every sample used for both training and testing
- More reliable estimate than holdout
- Reduced bias and variance in evaluation
- Best use of limited data

**Disadvantages:**
- Computationally expensive (train K models)
- Slower than simple holdout

**In Python:**
```python
from sklearn.model_selection import cross_val_score, KFold

# K-Fold Cross Validation
kfold = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=kfold, scoring='accuracy')

print(f"Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
# Example output: Accuracy: 0.847 ± 0.023

# Stratified K-Fold (for imbalanced data)
from sklearn.model_selection import StratifiedKFold
skfold = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=skfold)
```

---

### 3.3 Comparison of Partitioning Methods

| Method | Reliability | Speed | Data Usage | Best For |
|--------|-------------|-------|-----------|----------|
| Holdout | Low | Fast | Wastes 30% | Large datasets, quick evaluation |
| Random Subsampling | Medium | Medium | Variable | Medium datasets |
| K-Fold CV | High | Slow | All data used | Small-medium datasets, model selection |
| LOOCV | Highest | Very Slow | Maximum | Very small datasets |

---

## Quick Revision Table

| Topic | Key Points |
|-------|-----------|
| EDA Definition | Summarize data characteristics using visuals before modeling |
| Data Sourcing | Identify, collect from internal/external/primary/secondary sources |
| Data Cleaning | Handle missing, duplicates, outliers, inconsistencies, wrong types |
| Univariate | One variable: mean, median, std, histogram, boxplot |
| Bivariate | Two variables: correlation, scatter plot, crosstab, chi-square |
| Ensemble Methods | Combine weak learners → strong learner |
| Bagging | Parallel, bootstrap samples, majority vote, reduces variance |
| Boosting | Sequential, reweight errors, reduces bias |
| AdaBoost | Weighted classifiers, αₜ = ½ln((1-ε)/ε), exponential weight update |
| Random Forest | Bagging + random feature subset at each split, very robust |
| Confusion Matrix | TP, TN, FP, FN → Accuracy, Precision, Recall, F1 |
| Holdout | Simple 70-30 split, fast but unreliable |
| K-Fold CV | K splits, each fold tested once, reliable estimate |

---

## Important Formulas

1. **Accuracy** = (TP + TN) / (TP + TN + FP + FN)
2. **Precision** = TP / (TP + FP)
3. **Recall (Sensitivity)** = TP / (TP + FN)
4. **Specificity** = TN / (TN + FP)
5. **F1-Score** = 2 × (Precision × Recall) / (Precision + Recall)
6. **AdaBoost α** = ½ × ln((1 - ε) / ε)
7. **Weight Update** = wᵢ × e^(±αₜ)
8. **K-Fold Accuracy** = (1/K) × Σ Accuracyₖ

---

*Prepared for SPPU End Semester Examination – Unit IV: Advanced Predictive Analytics Algorithms*
