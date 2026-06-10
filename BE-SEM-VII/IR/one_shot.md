# 📘 Information Retrieval

## Text Classification & Clustering – One-Page Short Notes

---

## 1. Text Classification
    
**Goal:** Assign documents to **predefined classes** (supervised learning).

### Key Steps

1. Text preprocessing (tokenization, stopword removal)
2. Feature extraction (Bag of Words, **TF-IDF**)
3. Apply classifier
4. Evaluate results

---

## 2. Naïve Bayes Classifier

* **Probabilistic classifier**
* Based on **Bayes’ theorem**
* Assumes **conditional independence** of features

[
P(C \mid D) \propto P(C) \prod P(w_i \mid C)
]

### Advantages

* Fast, scalable
* Works well for high-dimensional text

### Applications

* Spam filtering
* Sentiment analysis
* Document classification
* News categorization

---

## 3. k-Nearest Neighbor (KNN)

* **Instance-based, lazy learning**
* Class decided by **majority vote of k nearest neighbors**
* Similarity: **Cosine similarity** (IR)

### Pros / Cons

* ✔ Simple, no training
* ❌ Slow for large datasets

---

## 4. Vector Space Classification

* Documents → vectors in **high-dimensional space**
* Uses **hyperplanes** for separation

### Hypothesis

[
\mathbf{w} \cdot \mathbf{x} + b = 0
]

Used in:

* Rocchio classifier
* SVM

---

## 5. Support Vector Machine (SVM)

* Finds **maximum margin hyperplane**
* Effective in high-dimensional text data
* Can handle non-linear data using **kernels**

---

## 6. Kernel Function

Transforms data into higher-dimensional space

Common kernels:

* Linear
* Polynomial
* Radial Basis Function (RBF)

Used mainly in **SVM**

---

## 7. Spam Filtering

* Binary classification: Spam / Not Spam
* Common classifiers:

  * Naïve Bayes
  * SVM
  * KNN

Features:

* Word frequency
* Email content

---

## 8. Clustering vs Classification

| Classification | Clustering      |
| -------------- | --------------- |
| Supervised     | Unsupervised    |
| Labeled data   | No labels       |
| Fixed classes  | Discover groups |

✔ Clustering can assist classification when labels are scarce.

---

## 9. Clustering Algorithms – Types

1. **Partitioning** – k-means
2. **Hierarchical** – Agglomerative
3. Density-based – DBSCAN
4. Model-based – EM, GMM

---

## 10. k-Means Clustering

* Partitioning method
* Minimizes **within-cluster variance**
* Requires k in advance

### Limitations

* Sensitive to initial centroids
* Assumes spherical clusters

---

## 11. Agglomerative Hierarchical Clustering

* **Bottom-up approach**
* Produces a **dendrogram**

### Linkage Types

* Single
* Complete
* Average
* Centroid
* Ward’s method

---

## 12. Expectation–Maximization (EM)

* Used when **hidden variables** exist
* Iterative algorithm:

  * **E-step:** Estimate hidden variables
  * **M-step:** Update parameters

Used in:

* Soft clustering
* Mixture models

---

## 13. Mixture of Gaussians Model (GMM)

* Probabilistic clustering model
* Each cluster = Gaussian distribution

[
P(x) = \sum \pi_i \mathcal{N}(x \mid \mu_i, \Sigma_i)
]

### Features

* Soft clustering
* Learnt using EM
* More flexible than k-means

Below is a **ONE-PAGE, exam-ready SHORT NOTES** sheet covering **everything from this chapter** (Probabilistic IR, Language Models, NLP basics, DFR, Passage Retrieval).
This is designed for **quick revision before exams**.

---

# 📘 Information Retrieval – Probabilistic Models & Language Models

### **One-Page Revision Notes**

---

## 1. Probabilistic Information Retrieval (PIR)

* Ranks documents by **probability of relevance**
  [
  P(R=1|d,q)
  ]
* Assumes **binary relevance** (relevant / non-relevant)
* Based on **Probability Ranking Principle (PRP)**:

  > Rank documents in decreasing order of probability of relevance
* Foundation for **BIM, BM25, DFR, Language Models**

---

## 2. Binary Independence Model (BIM)

**Assumptions:**

* Terms are **binary** (present/absent)
* Terms are **independent**
* Relevance is binary

**Ranking function:**
[
\sum_{t \in q} \log \frac{p_t(1-q_t)}{q_t(1-p_t)}
]

* Basis for **BM25**
* Simple but ignores term frequency

---

## 3. Probability Ranking Principle (PRP)

* Optimal ranking if:

  * Relevance is binary
  * Documents are independent
* Interactive IR updates probabilities using **relevance feedback**
* Ensures **maximum retrieval effectiveness**

---

## 4. Bayesian Networks in IR

* **Directed Acyclic Graph (DAG)**
* Nodes → terms, documents, relevance
* Edges → probabilistic dependence

**Types:**

1. Naive Bayes – full independence
2. Tree-structured BN – limited dependencies
3. General BN – complex dependencies

Used to compute:
[
P(R|d,q)
]

---

## 5. Language Modeling Approach to IR

* Each **document = language model**
* Rank by **query likelihood**:
  [
  P(q|d) = \prod_{t \in q} P(t|d)
  ]
* Uses **multinomial distribution**
* Requires **smoothing**:

  * Dirichlet
  * Jelinek-Mercer

---

## 6. Language Models (LM)

**Types:**

* Unigram – independent words
* n-gram – short-range dependencies
* Document LM – query likelihood
* Relevance LM – uses feedback

**Key idea:**

> A relevant document can generate the query with high probability

---

## 7. Multinomial Distribution over Words

* Models **term frequencies**
  [
  P(t|d) = \frac{f_{t,d}}{|d|}
  ]
* Basis for **query likelihood model**
* Smoothing avoids zero probabilities

---

## 8. Ranking with Language Models

* Documents ranked by:
  [
  \sum_{t \in q} \log P(t|d)
  ]
* Handles document length naturally
* Competitive with **BM25**

---

## 9. Divergence from Randomness (DFR)

* Ranks documents by **informativeness of terms**
* Measures how much term occurrence **deviates from randomness**
* Uses:

  * Poisson / Binomial / Geometric models
* Score:
  [
  -\log P(\text{term occurs by chance})
  ]

---

## 10. Passage Retrieval & Ranking

* Retrieves **passages instead of full documents**
* Useful for:

  * Question Answering
  * Snippet generation
* Approaches:

  * Sliding window
  * Sentence-based
  * Topic-based
* Ranking:

  * BM25
  * Language Models
  * DFR

---

## 11. NLP Levels (Quick Recall)

1. Lexical – words
2. Syntactic – grammar
3. Semantic – meaning
4. Pragmatic – context
5. Discourse – sentence relations

---

## 12. NLP Challenges

* Ambiguity
* Context understanding
* Language variability
* Complex grammar
* World knowledge
* Resource limitations


# **UNIT V – WEB RETRIEVAL & WEB CRAWLING (ONE-PAGE SHORT NOTES)**

---

## **1. Web Retrieval**

Web retrieval deals with **searching and retrieving relevant web documents** in response to user queries.

### **Challenges**

* Huge size of the Web
* Dynamic content
* Heterogeneous data
* Spam and duplicate pages
* Low latency requirement

---

## **2. Search Engine Architecture**

**Main Components:**

* Web Crawler
* Indexer
* Document Repository
* Query Processor
* Ranking Module
* User Interface

---

## **3. Parallel Information Retrieval**

Parallel IR improves **speed and scalability**.

### **Parallel Query Processing**

* Query processed simultaneously on multiple servers
* Reduces response time
* Supports millions of queries

**Types:**

* Query parallelism
* Data parallelism
* Hybrid parallelism

---

## **4. Search Engine Ranking**

Ranking determines **order of search results**.

### **Ranking Function Uses:**

* Term Frequency (TF)
* Inverse Document Frequency (IDF)
* Document length normalization
* Link-based signals
* User behavior

**Example:** BM25

---

## **5. Link-Based Ranking**

Uses **link structure of the Web**.

### **Types**

* PageRank
* HITS (Authority & Hub)

---

## **6. PageRank Algorithm**

Measures **importance of webpages**.

### **Factors Affecting PageRank**

* Number of incoming links
* Quality of linking pages
* Outgoing links
* Damping factor (≈ 0.85)
* Internal link structure

---

## **7. Simple Ranking Functions & Evaluation**

* Boolean model
* TF–IDF
* Precision
* Recall
* F-measure

---

## **8. Web Crawler**

A program that **automatically downloads web pages**.

### **Crawler Tasks**

* Fetch pages
* Extract links
* Follow links
* Store content

---

## **9. Web Crawler Structure**

* Seed URLs
* URL Frontier
* Page Fetcher
* Parser
* URL Filter
* Repository

---

## **10. Focused Web Crawler**

Crawls pages related to **specific topics only**.

### **Components**

* Seed URLs
* Relevance classifier
* Priority queue
* Link extractor
* Content analyzer

---

## **11. Web Crawler Libraries (Python)**

### **Scrapy**

* Full crawler framework
* Large-scale crawling
* Scheduling + parallelism

### **Beautiful Soup**

* HTML parsing library
* Data extraction
* Small-scale scraping

---

## **12. Applications**

* Search engines
* Web indexing
* News aggregation
* Market analysis
* Research & ML datasets

---

## **13. Main Challenges Posed by Web**

* Massive size
* Rapid changes
* Unstructured data
* Spam
* Distributed nature

# **UNIT VI – INFORMATION RETRIEVAL APPLICATIONS (Quick Revision Notes)**

---

## **1. Multimedia Information Retrieval (MIR)**

**Definition:**
Retrieval of multimedia data such as **images, audio, video** using content, metadata, or semantics.

**Key Challenges**

* Semantic gap
* High dimensionality
* Large storage & computation
* Multimodal data (text + audio + video)

**MIR Pipeline**
Data collection → Feature extraction → Indexing → Query → Similarity matching → Ranking

---

## **2. Multimedia IR Models**

1. **Text-based Model**

   * Uses captions, tags
   * Simple but annotation dependent

2. **Content-Based Model (CBIR/CBVR)**

   * Uses intrinsic features (color, texture, MFCC, motion)
   * No manual annotation

3. **Semantic-Based Model**

   * Uses ML, DL, ontologies
   * Reduces semantic gap

4. **Multimodal Fusion Model**

   * Combines text + visual + audio

---

## **3. Audio Retrieval**

### **Spoken Language Audio Retrieval**

* Speech → Text (ASR)
* Index text & retrieve audio segments
* Used in lectures, podcasts, call centers

### **Non-Speech Audio Retrieval**

* Uses acoustic features (MFCC, ZCR)
* Query-by-example common
* Used in music, surveillance, wildlife monitoring

---

## **4. Image (Imagery) Retrieval**

**CBIR Features**

* Color (histogram)
* Texture (GLCM)
* Shape (edges)

**Query Types**

* Keyword
* Query-by-image
* Sketch

**Key Issue:** Semantic gap

---

## **5. Video Retrieval**

**Components**

* Keyframes
* Shot detection
* Motion + audio features

**Query Types**

* Keyword
* Query-by-example

**Applications**

* YouTube
* Surveillance
* Sports analysis

---

## **6. Graph Retrieval**

* Data represented as **nodes + edges**
* Uses structure + relationships
* Algorithms: PageRank, HITS

**Applications**

* Web search
* Social networks
* Knowledge graphs

---

## **7. Recommender Systems (RS)**

**Goal:** Suggest items without explicit queries

### **Difference: IR vs RS**

* IR → Query-based
* RS → Preference-based

---

## **8. Types of Recommender Systems**

### **1. Collaborative Filtering**

* User-based / Item-based
* Uses user behavior
* Problem: Cold start, sparsity

### **2. Content-Based**

* Uses item features
* Personalized
* Problem: Over-specialization

### **3. Knowledge-Based**

* Uses rules & constraints
* No ratings needed
* Used for cars, real estate

### **4. Hybrid**

* Combines multiple approaches

---

## **9. Four Phases of Recommender System**

1. Data collection
2. User & item profiling
3. Recommendation generation
4. Evaluation & feedback

---

## **10. Evaluation Metrics (Very Important)**

**Accuracy**

* Precision
* Recall
* F1-Score

**Prediction**

* MAE
* RMSE

**Ranking**

* MAP
* NDCG

**User-centric**

* Coverage
* Diversity
* Novelty

---

## **11. Information Extraction & Integration**

* Extract structured data from text
* Techniques:

  * NER
  * Relation extraction
  * Sentiment analysis

**Used for**

* Better recommendations
* Knowledge graph creation

---

## **12. Semantic Web**

**Goal:** Make data machine-understandable

**Core Technologies**

* RDF (triples)
* RDFS
* OWL
* SPARQL

**Benefits**

* Semantic search
* Data integration
* Knowledge-based RS

---

## **13. Collecting Specialized Web Information**

**Steps**

* Focused crawling
* Information extraction
* Data cleaning
* Semantic integration

**Applications**

* Medical IR
* Legal IR
* Scientific databases