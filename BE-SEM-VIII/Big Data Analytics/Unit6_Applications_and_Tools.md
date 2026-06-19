# Unit VI: Big Data Analytics Applications and Tools

---

## Section 1: Big Data Analytics Applications

---

### 1.1 Retail Analytics

**Definition:** Retail analytics uses big data to optimize business operations, improve customer experience, and increase profitability in the retail sector.

**Applications:**

| Application | Description |
|-------------|-------------|
| **Customer Segmentation** | Grouping customers by behavior, demographics, purchasing patterns |
| **Recommendation Engines** | "Customers who bought X also bought Y" (Amazon, Netflix) |
| **Demand Forecasting** | Predicting future product demand using historical sales data |
| **Price Optimization** | Dynamic pricing based on demand, competition, inventory |
| **Inventory Management** | Predict stock levels, reduce overstocking/understocking |
| **Market Basket Analysis** | Identify products frequently purchased together (Apriori) |
| **Sentiment Analysis** | Analyze customer reviews/social media for brand perception |
| **Store Layout Optimization** | Heatmaps of customer movement to optimize product placement |
| **Churn Prediction** | Identify customers likely to leave; offer retention strategies |
| **Fraud Detection** | Detect unusual transaction patterns in real-time |

**Technologies Used:** Hadoop, Spark, recommendation algorithms (collaborative filtering), NLP for reviews, real-time streaming (Kafka)

**Example:** Walmart processes 2.5 petabytes of data/hour to optimize pricing, inventory, and personalized marketing.

---

### 1.2 Financial Data Analytics (Covers Q7a)

### PYQ Covered:
- **Q7a)** How does financial data analytics leverage big data to drive insights, mitigate risks, and enhance decision-making? [10 Marks]

---

**Definition:** Financial data analytics applies big data technologies to analyze massive financial datasets for better decision-making, risk management, and regulatory compliance.

**Key Areas and Applications:**

**1. Risk Management & Mitigation:**
- **Credit Risk Scoring** – ML models analyze thousands of variables (transaction history, social media, employment) to assess creditworthiness
- **Market Risk Analysis** – Real-time monitoring of market conditions, portfolio risk using Monte Carlo simulations
- **Operational Risk** – Detecting system failures, process breakdowns before they escalate
- **Stress Testing** – Simulating extreme scenarios on large historical datasets

**2. Fraud Detection & Prevention:**
- Real-time transaction monitoring using streaming analytics (Kafka, Spark Streaming)
- Anomaly detection algorithms flag unusual patterns
- Network analysis identifies fraud rings
- Reduces false positives by learning normal behavior patterns

**3. Algorithmic/High-Frequency Trading:**
- Process millions of market data points per second
- ML models predict short-term price movements
- Sentiment analysis of news/social media for trading signals
- Low-latency processing with in-memory computing (Spark)

**4. Customer Analytics:**
- Customer lifetime value prediction
- Personalized product recommendations (loans, investments)
- Churn prediction and retention strategies
- Cross-selling and up-selling optimization

**5. Regulatory Compliance (RegTech):**
- Anti-Money Laundering (AML) – Pattern detection in transactions
- Know Your Customer (KYC) – Automated identity verification
- Automated regulatory reporting
- Audit trail maintenance using distributed ledgers

**6. Decision-Making Enhancement:**
- Real-time dashboards for executives
- Predictive models for loan approval/denial
- Portfolio optimization using big data analytics
- Sentiment-driven investment decisions

**Big Data Technologies Used in Finance:**

| Technology | Use Case |
|-----------|----------|
| Hadoop/HDFS | Store massive historical transaction data |
| Apache Spark | Real-time fraud detection, streaming analytics |
| Apache Kafka | Real-time data ingestion from markets |
| NoSQL (HBase, Cassandra) | Store unstructured financial documents |
| Machine Learning (Scikit-learn, TensorFlow) | Predictive models for risk/fraud |
| NLP | Analyze financial reports, news, social media |
| Graph Databases (Neo4j) | Detect fraud networks, relationship mapping |

**Benefits:**
1. Faster and more accurate risk assessment
2. Reduced fraud losses (up to 50% reduction)
3. Better customer targeting and retention
4. Regulatory compliance automation
5. Real-time market insights for competitive advantage
6. Reduced operational costs through automation

**Challenges:**
- Data privacy and security regulations (GDPR, PCI-DSS)
- Legacy system integration
- Data quality and consistency across sources
- Need for skilled data scientists
- Explainability of ML models (black-box problem)

---

### 1.3 Healthcare Analytics

**Definition:** Healthcare analytics uses big data to improve patient outcomes, reduce costs, and enhance operational efficiency in healthcare systems.

**Applications:**

| Application | Description |
|-------------|-------------|
| **Predictive Diagnostics** | ML models predict disease risk based on patient data |
| **Electronic Health Records (EHR)** | Centralized patient data for better treatment decisions |
| **Drug Discovery** | Analyze molecular data to identify potential drug candidates |
| **Hospital Operations** | Optimize bed allocation, staff scheduling, resource utilization |
| **Genomics** | Analyze DNA sequences for personalized medicine |
| **Epidemic Prediction** | Track disease spread using social media, search trends |
| **Medical Imaging** | Deep learning for X-ray, MRI, CT scan analysis |
| **Clinical Trials** | Identify suitable patients, predict trial outcomes |
| **Insurance Claims** | Fraud detection in medical claims |
| **Wearable Data** | Real-time patient monitoring (heart rate, activity) |

**Example:** Google DeepMind's AI detects eye diseases from retinal scans with 94% accuracy.

**Technologies:** Hadoop for genomic data storage, Spark for real-time patient monitoring, NLP for clinical notes, Deep Learning for imaging.

---

### 1.4 Supply Chain Management

**Definition:** Supply chain analytics uses big data to optimize the flow of goods, information, and finances from raw materials to end consumers.

**Applications:**

| Application | Description |
|-------------|-------------|
| **Demand Forecasting** | Predict future demand using historical data, weather, events |
| **Route Optimization** | Find most efficient delivery routes using GPS + traffic data |
| **Inventory Optimization** | Balance stock levels to minimize costs and stockouts |
| **Supplier Risk Assessment** | Analyze supplier reliability, financial health, geopolitical risks |
| **Quality Control** | IoT sensors detect defects in real-time during manufacturing |
| **Warehouse Management** | Optimize storage layout, picking routes |
| **Real-time Tracking** | GPS, RFID, IoT for shipment visibility |
| **Sustainability** | Optimize carbon footprint across supply chain |

**Technologies:** IoT sensors, RFID, GPS tracking, Spark Streaming, predictive models, digital twins.

**Example:** Amazon uses predictive analytics to pre-position inventory in warehouses before customers even order (anticipatory shipping).

---

## Section 2: Types of Big Data Analytics Tools

---

### 2.1 Data Collection Tools

---

#### 2.1.1 Semantria Tool (Covers Q8a)

### PYQ Covered:
- **Q8a)** How does Semantria streamline data collection and what features make it valuable for analyzing unstructured data? [10 Marks]

---

**What is Semantria?**
Semantria (by Lexalytics) is a cloud-based text analytics and Natural Language Processing (NLP) tool that collects, processes, and analyzes large volumes of unstructured text data from various sources.

**How Semantria Streamlines Data Collection:**

1. **Multi-Source Data Ingestion:**
   - Collects data from social media (Twitter, Facebook, Reddit)
   - Processes customer reviews, emails, support tickets
   - Analyzes news articles, blogs, forums
   - Handles documents, PDFs, web pages
   - Supports 20+ languages

2. **API-Based Integration:**
   - RESTful API for seamless integration with existing systems
   - Supports batch processing and real-time streaming
   - Easy to embed in applications (Excel, Python, Java, .NET)
   - Scalable cloud infrastructure handles volume spikes

3. **Automated Processing Pipeline:**
   ```
   Raw Text → Preprocessing → NLP Analysis → Structured Output → Visualization
   ```

**Key Features and Capabilities:**

| Feature | Description |
|---------|-------------|
| **Sentiment Analysis** | Determines positive/negative/neutral sentiment with confidence scores |
| **Entity Extraction** | Identifies people, places, organizations, products, monetary values |
| **Theme/Topic Detection** | Automatically discovers main topics in large text collections |
| **Categorization** | Classifies documents into predefined or auto-detected categories |
| **Summarization** | Generates concise summaries of long documents |
| **Intention Detection** | Identifies customer intent (buy, complain, inquire) |
| **Aspect-Based Sentiment** | Sentiment for specific aspects (e.g., "battery life: positive, price: negative") |
| **Language Detection** | Automatically detects language of input text |
| **Custom Configuration** | Define custom entities, categories, sentiment rules for domain-specific analysis |

**Why Semantria is Valuable for Businesses:**

1. **Handles Unstructured Data at Scale:**
   - 80% of business data is unstructured (text, audio, video)
   - Semantria converts this into structured, analyzable data
   - Processes millions of documents without manual reading

2. **Real-Time Insights:**
   - Stream processing for live social media monitoring
   - Immediate alerts on negative sentiment spikes
   - Real-time brand reputation tracking

3. **Customization:**
   - Industry-specific models (healthcare, finance, retail)
   - Custom dictionaries and sentiment rules
   - Trainable classification models

4. **Actionable Outputs:**
   - Structured JSON/XML output for integration with BI tools
   - Dashboard-ready data for Tableau, Power BI
   - Trend tracking over time

5. **Ease of Use:**
   - No NLP expertise required (pre-built models)
   - Excel add-in for non-technical users
   - Comprehensive API documentation

**Use Cases:**
- Brand monitoring and reputation management
- Voice of Customer (VoC) analysis
- Competitive intelligence
- Market research and trend analysis
- Customer support automation (ticket routing)
- Compliance monitoring (detecting risky language)

---

#### 2.1.2 AS Sentiment Analysis Tool

**What is it?**
AS (Automated Sentiment) Analysis tools are software solutions that automatically determine the emotional tone behind text data — whether opinions expressed are positive, negative, or neutral.

**How it Works:**
```
Input Text → Tokenization → Feature Extraction → Classification → Sentiment Score
```

**Approaches:**
1. **Lexicon-Based** – Uses dictionaries of positive/negative words with scores
2. **Machine Learning** – Trained on labeled data (SVM, Naive Bayes, Neural Networks)
3. **Hybrid** – Combines both approaches

**Features:**
- Real-time sentiment scoring
- Multi-language support
- Aspect-level analysis
- Emotion detection (anger, joy, sadness, fear)
- Sarcasm and irony detection (advanced)

**Applications:** Social media monitoring, product review analysis, election prediction, stock market sentiment

---

### 2.2 Data Storage Tools and Frameworks

---

#### 2.2.1 Apache HBase (Covers Q7b)

### PYQ Covered:
- **Q7b)** How does Apache HBase contribute to efficient data storage and retrieval in big data environments? [7 Marks]

---

**What is Apache HBase?**
Apache HBase is an open-source, distributed, scalable, NoSQL database built on top of Hadoop HDFS. It provides real-time read/write access to large datasets.

**Key Characteristics:**
- Column-oriented (column-family) database
- Built on top of HDFS (Hadoop Distributed File System)
- Modeled after Google's Bigtable
- Supports billions of rows × millions of columns
- Provides random, real-time read/write access

**Architecture:**

```
┌─────────────────────────────────────────────┐
│              Client Application              │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│              ZooKeeper                        │
│   (Coordination, Master election, Config)    │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│            HBase Master                       │
│  (Region assignment, Load balancing, DDL)    │
└──────────────────┬──────────────────────────┘
                   │
    ┌──────────────┼──────────────┐
    ▼              ▼              ▼
┌────────┐   ┌────────┐   ┌────────┐
│Region  │   │Region  │   │Region  │
│Server 1│   │Server 2│   │Server 3│
└───┬────┘   └───┬────┘   └───┬────┘
    │             │             │
    ▼             ▼             ▼
┌─────────────────────────────────────────────┐
│              HDFS (Storage Layer)             │
└─────────────────────────────────────────────┘
```

**Components:**

| Component | Role |
|-----------|------|
| **HBase Master** | Manages RegionServers, handles schema changes, load balancing |
| **Region Server** | Serves read/write requests for assigned regions |
| **Region** | A subset of table's rows (auto-splits when grows) |
| **ZooKeeper** | Coordinates distributed processes, master election |
| **HDFS** | Underlying storage for persistence |
| **MemStore** | In-memory write buffer (fast writes) |
| **HFile** | On-disk storage format (sorted key-value) |
| **WAL (Write-Ahead Log)** | Ensures durability before writes are acknowledged |

**How HBase Contributes to Efficient Storage & Retrieval:**

**1. Efficient Storage:**
- **Column-family storage** – Only stores non-null columns (sparse data friendly)
- **Automatic sharding** – Tables split into regions, distributed across servers
- **Compression** – Supports Snappy, GZip, LZO for reduced storage
- **Versioning** – Stores multiple versions of data with timestamps
- **Horizontal scalability** – Add nodes to increase capacity linearly

**2. Efficient Retrieval:**
- **Row key design** – Data is sorted by row key for fast lookups
- **In-memory caching** – BlockCache and MemStore for frequent reads
- **Bloom filters** – Quickly determine if a row exists without disk I/O
- **Region-level access** – Only scan relevant regions, not entire table
- **Coprocessors** – Server-side computation (like stored procedures)

**3. Real-Time Access:**
- Millisecond-level read/write latency
- Random access to any row by key
- Suitable for real-time applications (unlike batch-only HDFS)

**4. Fault Tolerance:**
- Data replicated across HDFS (default 3 copies)
- WAL ensures no data loss on server crash
- Automatic failover via ZooKeeper

**Data Model:**
```
Table: student_records
┌──────────┬─────────────────────────┬──────────────────────┐
│ Row Key  │ Column Family: personal │ Column Family: marks  │
├──────────┼─────────────────────────┼──────────────────────┤
│ STU001   │ name: "Atharv"          │ BDA: 85              │
│          │ city: "Pune"            │ ML: 90               │
├──────────┼─────────────────────────┼──────────────────────┤
│ STU002   │ name: "Rahul"           │ BDA: 78              │
│          │                         │ BI: 82               │
└──────────┴─────────────────────────┴──────────────────────┘
```

**HBase vs RDBMS:**

| Feature | HBase | RDBMS |
|---------|-------|-------|
| Schema | Flexible, dynamic | Fixed, predefined |
| Scale | Petabytes, distributed | Limited by single server |
| Data Model | Column-family | Relational (tables) |
| Query | Get/Put/Scan by row key | Full SQL with JOINs |
| Transactions | Row-level only | ACID across tables |
| Best For | Sparse, wide, big data | Structured, transactional |

**Use Cases:** Facebook messages (original), time-series data, IoT sensor data, real-time analytics, log storage.

---

#### 2.2.2 CouchDB

**What is CouchDB?**
Apache CouchDB is an open-source NoSQL document-oriented database that uses JSON for storing data, JavaScript for querying (MapReduce), and HTTP/REST API for access.

**Key Features:**

| Feature | Description |
|---------|-------------|
| **Document Store** | Stores data as JSON documents (no fixed schema) |
| **RESTful HTTP API** | All operations via standard HTTP methods (GET, PUT, POST, DELETE) |
| **Multi-Version Concurrency Control (MVCC)** | No locking; readers never block writers |
| **Replication** | Built-in master-master replication for distributed setups |
| **Offline-First** | Works offline, syncs when connected (CouchDB + PouchDB) |
| **MapReduce Views** | JavaScript-based queries for indexing and aggregation |
| **Eventual Consistency** | Prioritizes availability over strict consistency (CAP theorem: AP) |

**Data Model:**
```json
{
  "_id": "student001",
  "_rev": "1-abc123",
  "name": "Atharv",
  "course": "BDA",
  "marks": {
    "unit1": 85,
    "unit2": 90
  },
  "tags": ["big-data", "analytics"]
}
```

**CouchDB vs HBase:**

| Feature | CouchDB | HBase |
|---------|---------|-------|
| Data Model | JSON Documents | Column-family |
| Query | MapReduce (JS) | Get/Scan by row key |
| API | HTTP/REST | Java API, Thrift |
| Replication | Master-Master | Master-Slave (via HDFS) |
| Best For | Web apps, mobile sync | Large-scale analytics |
| CAP | AP (Availability + Partition) | CP (Consistency + Partition) |

**Use Cases:** Web applications, mobile apps (offline sync), content management, semi-structured data storage.

---

### 2.3 Data Filtering and Extraction Tools

---

#### 2.3.1 Scraper

**What is Scraper?**
A web scraper is a tool that automatically extracts data from websites by parsing HTML content and converting it into structured formats (CSV, JSON, database).

**How Web Scraping Works:**
```
Target URL → HTTP Request → HTML Response → Parse DOM → Extract Data → Store
```

**Types of Scrapers:**

| Type | Description | Example |
|------|-------------|---------|
| Browser Extension | Simple point-and-click extraction | Chrome Scraper extension |
| Desktop Software | GUI-based, no coding needed | Octoparse, ParseHub |
| Programming Libraries | Code-based, highly customizable | BeautifulSoup (Python), Scrapy |
| Cloud-Based | Scalable, managed infrastructure | Mozenda, Import.io |

**Features:**
- Automated data extraction from web pages
- Scheduled/recurring scraping jobs
- Handle pagination and infinite scroll
- Export to CSV, Excel, JSON, database
- Handle JavaScript-rendered content (headless browsers)
- IP rotation to avoid blocking

**Challenges:**
- Website structure changes break scrapers
- Anti-scraping measures (CAPTCHA, rate limiting)
- Legal/ethical concerns (robots.txt, ToS)
- Dynamic content loaded via JavaScript

---

#### 2.3.2 Mozenda (Covers Q8b)

### PYQ Covered:
- **Q8b)** How does Mozenda serve as an effective data filtering and extraction tool? [7 Marks]

---

**What is Mozenda?**
Mozenda is a cloud-based web data extraction platform that enables businesses to collect, filter, and organize data from websites without programming knowledge.

**How Mozenda Serves as an Effective Data Filtering and Extraction Tool:**

**1. Point-and-Click Interface:**
- Visual agent builder – no coding required
- Select data fields by clicking on web page elements
- Automatically detects repeating patterns (lists, tables)
- WYSIWYG (What You See Is What You Get) approach

**2. Intelligent Data Extraction:**
- **Agent Technology** – Create reusable "agents" that navigate websites and extract data
- **Pattern Recognition** – Automatically identifies data structures across pages
- **Multi-page Navigation** – Handles pagination, "Next" buttons, infinite scroll
- **Form Interaction** – Fills forms, dropdown selections, search queries automatically
- **JavaScript Rendering** – Handles dynamic/AJAX-loaded content

**3. Data Filtering Capabilities:**
- **Built-in Filters:** Remove HTML tags, trim whitespace, format dates
- **Conditional Logic:** Extract only if criteria met (e.g., price < ₹1000)
- **Regular Expressions:** Pattern-based extraction and filtering
- **Deduplication:** Automatically removes duplicate records
- **Data Validation:** Check extracted data against rules

**4. Scheduling and Automation:**
- Schedule agents to run at specific intervals (hourly, daily, weekly)
- Automatic re-runs when source data changes
- Email notifications on completion or errors
- Queue management for large-scale extraction

**5. Data Export and Integration:**
- Export to CSV, TSV, XML, JSON
- Direct integration with databases (SQL Server, MySQL)
- API access for programmatic control
- Integration with BI tools (Tableau, Power BI)
- Cloud storage export (AWS S3, Azure Blob)

**6. Scalability and Reliability:**
- Cloud-based infrastructure handles millions of pages
- Distributed extraction across multiple servers
- Error handling and automatic retries
- Handles website changes with adaptive extraction

**Mozenda Workflow:**
```
┌────────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────────┐
│ Build Agent│───▶│ Run/Schedule │───▶│ Filter Data │───▶│  Export   │
│ (Point &   │    │  Extraction  │    │ (Clean,     │    │ (CSV, DB,│
│  Click)    │    │              │    │  Validate)  │    │  API)    │
└────────────┘    └──────────────┘    └─────────────┘    └──────────┘
```

**Key Features Summary:**

| Feature | Benefit |
|---------|---------|
| No-code agent builder | Non-technical users can extract data |
| Cloud-based | No infrastructure management |
| Scheduling | Automated recurring extraction |
| Data filtering | Clean data at source |
| Multi-format export | Integrates with existing workflows |
| Error handling | Reliable large-scale extraction |
| API access | Programmatic control for developers |

**Use Cases:**
- Price monitoring and competitive intelligence
- Lead generation (extracting contact information)
- Market research (product reviews, ratings)
- Real estate listings aggregation
- Job posting aggregation
- News and content monitoring
- Academic research data collection

**Mozenda vs Manual Data Collection:**

| Aspect | Manual | Mozenda |
|--------|--------|---------|
| Speed | Hours for few pages | Minutes for thousands |
| Accuracy | Human error prone | Consistent extraction |
| Scale | Limited | Millions of pages |
| Cost | High labor cost | Fixed subscription |
| Updates | Manual re-collection | Automated scheduling |

---

## Section 3: Comparison of All Tool Categories

| Category | Tool | Type | Key Strength |
|----------|------|------|-------------|
| **Data Collection** | Semantria | NLP/Text Analytics | Unstructured text analysis |
| **Data Collection** | AS Sentiment | Sentiment Analysis | Emotion detection |
| **Data Storage** | Apache HBase | Column-family NoSQL | Real-time big data access |
| **Data Storage** | CouchDB | Document NoSQL | Offline sync, REST API |
| **Data Extraction** | Scraper | Web Scraping | Custom data extraction |
| **Data Extraction** | Mozenda | Web Data Platform | No-code, enterprise scale |

---

## Quick Revision Table

| Topic | Key Points |
|-------|-----------|
| Retail Analytics | Recommendation, demand forecasting, market basket, churn |
| Financial Analytics | Fraud detection, risk scoring, algo trading, compliance |
| Healthcare Analytics | EHR, drug discovery, predictive diagnostics, genomics |
| Supply Chain | Demand forecast, route optimization, IoT tracking |
| Semantria | NLP, sentiment, entity extraction, multi-source, API-based |
| HBase | Column-family, HDFS-based, ZooKeeper, RegionServer, real-time |
| CouchDB | JSON documents, HTTP API, master-master replication, offline-first |
| Mozenda | Point-click extraction, scheduling, filtering, cloud-based, no-code |
| Scraper | Web data extraction, DOM parsing, handles pagination |

---

## Important Points for Exam

1. **HBase sits on HDFS** – provides random real-time access that HDFS alone cannot
2. **Semantria processes unstructured data** – converts text to structured insights
3. **Mozenda is no-code** – differentiator from programming-based scrapers
4. **Financial analytics uses real-time streaming** – Kafka + Spark for fraud detection
5. **CouchDB uses eventual consistency** – AP in CAP theorem
6. **HBase uses strong consistency** – CP in CAP theorem

---

*Prepared for SPPU End Semester Examination – Unit VI: Big Data Analytics Applications and Tools*
