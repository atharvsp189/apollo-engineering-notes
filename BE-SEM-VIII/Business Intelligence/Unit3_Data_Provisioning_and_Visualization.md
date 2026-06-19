# Unit III: Data Provisioning and Data Visualization (6 Hours)

## SPPU Business Intelligence - Exam Preparation

---

# PART A: DATA PROVISIONING

---

## 1. Data Warehouse

**Definition:** A Data Warehouse is a centralized repository that stores integrated data from multiple sources, designed for query and analysis rather than transaction processing.

**Key Characteristics:**
- **Subject-Oriented:** Organized around major subjects (customers, sales, products)
- **Integrated:** Data from various sources is made consistent
- **Time-Variant:** Stores historical data (5-10 years)
- **Non-Volatile:** Data is stable; once entered, it is not changed or deleted

**Purpose in BI:** Provides a single source of truth for business reporting, analytics, and decision-making.

---

## 2. Schemas

**Definition:** Schema defines the logical structure/organization of data in a data warehouse.

### Types of Schemas:

| Schema | Structure | Use Case |
|--------|-----------|----------|
| Star Schema | Central fact table + dimension tables | Simple queries, fast performance |
| Snowflake Schema | Normalized dimension tables | Reduced redundancy, complex queries |
| Galaxy/Fact Constellation | Multiple fact tables sharing dimensions | Complex enterprises |

**Star Schema:**
```
        [Time Dim]
            |
[Product Dim] -- [FACT TABLE (Sales)] -- [Customer Dim]
            |
        [Store Dim]
```

**Snowflake Schema:** Dimension tables are further normalized into sub-dimension tables.

---

## 3. Data Quality

**Definition:** Data Quality refers to the condition of data based on factors like accuracy, completeness, consistency, reliability, and timeliness.

**Dimensions of Data Quality:**
1. **Accuracy** – Data correctly represents real-world entities
2. **Completeness** – All required data is present
3. **Consistency** – Data is uniform across systems
4. **Timeliness** – Data is available when needed
5. **Validity** – Data conforms to defined formats/rules
6. **Uniqueness** – No duplicate records

**Impact of Poor Data Quality:**
- Wrong business decisions
- Loss of customer trust
- Increased operational costs
- Regulatory compliance issues

---

## 4. Data Profiling

**Definition:** Data Profiling is the process of examining, analyzing, and reviewing data from existing sources to collect statistics and information about that data.

**Key Activities:**
1. **Column Analysis** – Data type, length, null values, distinct values
2. **Cross-Column Analysis** – Dependencies between columns
3. **Cross-Table Analysis** – Relationships between tables (FK references)

**Purpose:**
- Understand data structure and content
- Identify data quality issues early
- Plan ETL transformations
- Discover patterns and anomalies

**Tools used:** Informatica Data Quality, Talend, IBM InfoSphere

---

## 5. Data Enrichment

**Definition:** Data Enrichment is the process of enhancing, refining, and improving raw data by merging it with additional relevant data from internal or external sources.

**Types:**
1. **Demographic Enrichment** – Adding age, income, location data
2. **Geographic Enrichment** – Adding geo-coordinates, region info
3. **Behavioral Enrichment** – Adding purchase history, browsing patterns

**Example:** A customer database with only names and emails can be enriched with social media profiles, purchase history, and demographic information.

**Benefits:**
- Better customer segmentation
- Improved analytics and predictions
- More personalized marketing
- Enhanced decision-making

---

## 6. Data Duplication

**Definition:** Data Duplication (or data redundancy) occurs when the same piece of data exists in multiple places within a data warehouse or across systems.

### 📝 PYQ: Q1 a) Evaluate the impact of Data Duplication on the overall performance and efficiency of a data warehouse [8 Marks]

**Answer:**

**Introduction:** Data duplication refers to the unnecessary repetition of data across tables, databases, or systems within a data warehouse environment.

**Negative Impacts on Performance and Efficiency:**

1. **Increased Storage Costs:**
   - Duplicate data consumes additional disk space
   - As data grows, storage costs increase exponentially
   - Example: Customer address stored in 5 tables = 5x storage

2. **Data Inconsistency:**
   - Updates may not propagate to all copies
   - Leads to conflicting information across reports
   - Example: Customer address updated in one table but not others

3. **Slower Query Performance:**
   - Larger tables take more time to scan
   - Index maintenance becomes expensive
   - JOIN operations become slower with redundant data

4. **ETL Processing Overhead:**
   - More data to extract, transform, and load
   - Increased processing time during batch operations
   - Higher resource utilization (CPU, memory, I/O)

5. **Maintenance Complexity:**
   - More effort to maintain data integrity
   - Complex update procedures needed
   - Increased risk of errors during maintenance

6. **Reduced Data Quality:**
   - Difficult to identify "single source of truth"
   - Conflicting versions lead to trust issues
   - Compliance and auditing become difficult

**Positive Aspects (Controlled Duplication):**

7. **Improved Read Performance:**
   - Denormalized tables reduce JOINs
   - Pre-aggregated data speeds up reporting
   - Star schema intentionally uses controlled redundancy

8. **High Availability:**
   - Replication ensures data availability during failures
   - Disaster recovery benefits from controlled duplication

**Mitigation Strategies:**
- Master Data Management (MDM)
- Data deduplication tools
- Proper normalization where appropriate
- Regular data audits
- Clear data governance policies

**Conclusion:** While uncontrolled data duplication severely impacts warehouse performance, efficiency, and data quality, controlled redundancy (as in star schemas) can improve query performance. Organizations must balance between normalization and strategic denormalization.

---

## 7. ETL Architecture and What is ETL

### 📝 PYQ: Q1 b) Explain the concepts of ETL Architecture, Extraction, Transformation, and Loading (ETL) in the context of Business Intelligence [5 Marks]

**Answer:**

**What is ETL?**
ETL stands for **Extract, Transform, Load** – a process that moves data from source systems into a data warehouse.

**ETL Architecture:**

```
[Source Systems] → [Extraction] → [Staging Area] → [Transformation] → [Loading] → [Data Warehouse]
     |                                                                                    |
  - OLTP DBs                                                                        - Fact Tables
  - Flat Files                                                                      - Dimension Tables
  - APIs                                                                            - Data Marts
  - Legacy Systems
```

**Three Phases:**

**1. Extraction:**
- Pulling data from various source systems
- Sources: Databases, flat files, XML, APIs, web services
- Types: Full extraction, Incremental extraction
- Goal: Minimal impact on source systems

**2. Transformation:**
- Converting extracted data into warehouse-compatible format
- Activities: Cleaning, filtering, joining, aggregating, deriving new values
- Business rules are applied here
- Data standardization and formatting

**3. Loading:**
- Inserting transformed data into the target data warehouse
- Types: Initial load (first time), Incremental load (periodic updates)
- Methods: Bulk loading, trickle feeding

**ETL Architecture Components:**
1. **Source Layer** – All data sources
2. **ETL Engine** – Core processing (Informatica, SSIS, Talend)
3. **Staging Area** – Temporary storage for intermediate data
4. **Target Layer** – Data warehouse/data marts
5. **Metadata Repository** – Stores ETL job definitions and mappings
6. **Scheduler** – Manages ETL job execution timing

**Significance in BI:**
- Ensures clean, consistent data for reporting
- Enables historical analysis
- Supports decision-making with reliable data

---

## 8. Extraction Concept and Change Data Capture

### 📝 PYQ: Q1 c) Explain the concept of Change Data Capture and its significance in data provisioning [5 Marks]

**Answer:**

**Extraction Concept:**

Extraction is the first step of ETL where data is read/pulled from source systems.

**Types of Extraction:**
1. **Full Extraction** – Entire data is extracted every time
2. **Incremental Extraction** – Only changed data since last extraction

**Extraction Methods:**
- Direct database queries (SQL)
- Log-based extraction
- API-based extraction
- File-based extraction

---

**Change Data Capture (CDC):**

**Definition:** CDC is a set of techniques used to identify and capture data that has changed (inserted, updated, deleted) in source systems since the last extraction.

**CDC Methods:**

| Method | Description | Pros | Cons |
|--------|-------------|------|------|
| **Timestamp-based** | Uses last_modified column | Simple to implement | Misses deletes |
| **Trigger-based** | Database triggers log changes | Captures all changes | Performance overhead on source |
| **Log-based** | Reads database transaction logs | No source impact | Complex implementation |
| **Snapshot-based** | Compares full snapshots | Detects all changes | High resource usage |

**Significance in Data Provisioning:**

1. **Reduced Load Time** – Only changed data is processed, not entire dataset
2. **Minimal Source Impact** – Less strain on operational systems
3. **Near Real-Time Updates** – Enables frequent warehouse refreshes
4. **Network Efficiency** – Less data transferred across networks
5. **Accurate History** – Tracks what changed and when
6. **Supports SCD** – Enables Slowly Changing Dimension management

**Example:**
- Source: Sales OLTP with 10 million records
- Daily new/changed records: ~50,000
- CDC extracts only 50,000 records instead of 10 million

---

## 9. Transformation Concept

**Definition:** Transformation is the process of converting extracted raw data into a format suitable for the data warehouse.

**Key Transformation Activities:**

1. **Data Cleansing** – Removing errors, fixing inconsistencies
2. **Data Standardization** – Uniform formats (date: DD-MM-YYYY)
3. **Deduplication** – Removing duplicate records
4. **Filtering** – Removing irrelevant data
5. **Aggregation** – Summarizing data (daily sales → monthly)
6. **Joining/Merging** – Combining data from multiple sources
7. **Deriving** – Calculating new values (profit = revenue - cost)
8. **Surrogate Key Generation** – Creating warehouse-specific keys
9. **Pivoting** – Changing row-column orientation

---

## 10. Lookups

### 📝 PYQ: Q2 a) Analyse the role of Lookups in the transformation process of ETL [8 Marks]

**Answer:**

**Definition:** A Lookup is a transformation operation that searches a reference table/dataset to find matching values, validate data, or retrieve related information during ETL processing.

**Types of Lookups:**

1. **Connected Lookup:**
   - Receives input from pipeline
   - Returns values to the pipeline
   - Used for real-time data enrichment
   - Example: Looking up customer name from customer_id

2. **Unconnected Lookup:**
   - Called by another transformation using :LKP expression
   - Returns one column value
   - Used conditionally
   - Example: Looking up tax rate only for specific products

3. **Cached Lookup:**
   - Lookup data loaded into memory (cache)
   - Faster performance for repeated lookups
   - Best for small-medium reference tables

4. **Uncached Lookup:**
   - Queries database for each row
   - Used when reference data is too large for memory
   - Slower but handles large datasets

**Role in ETL Transformation Process:**

1. **Data Validation:**
   - Verify if incoming data exists in reference tables
   - Example: Check if product_id exists in product master
   - Invalid data can be routed to error handling

2. **Data Enrichment:**
   - Add additional attributes from reference tables
   - Example: Add product_name, category using product_id lookup
   - Enhances data completeness

3. **Surrogate Key Assignment:**
   - Look up dimension tables to find surrogate keys
   - Map natural keys (business keys) to surrogate keys
   - Essential for fact table loading

4. **Slowly Changing Dimensions (SCD):**
   - Determine if record is new or existing
   - Decide insert vs. update action
   - Manage Type 1, Type 2, Type 3 SCD

5. **Data Standardization:**
   - Map source codes to standard values
   - Example: "M" → "Male", "F" → "Female"
   - Ensures consistency across sources

6. **Referential Integrity:**
   - Ensure foreign key relationships are valid
   - Prevent orphan records in fact tables
   - Example: Every order must have valid customer_id

7. **Conditional Processing:**
   - Apply different business rules based on lookup results
   - Route data to different targets based on lookup match/no-match
   - Example: New customers → insert path, existing → update path

**Performance Considerations:**
- Cache size impacts memory usage
- Index on lookup columns improves speed
- Persistent cache reduces repeated database hits
- Partitioning lookups for large reference tables

**Example Scenario:**
```
Sales Record: {order_id: 101, product_code: "P001", customer_code: "C501", amount: 5000}

Lookup 1: Product Dimension → Get product_SK, product_name, category
Lookup 2: Customer Dimension → Get customer_SK, customer_name, region
Lookup 3: Date Dimension → Get date_SK from order_date

Result: Fact record with all surrogate keys ready for loading
```

**Conclusion:** Lookups are fundamental to ETL transformation, enabling data validation, enrichment, key assignment, and ensuring data quality. They bridge the gap between source system codes and warehouse-friendly formats.

---

## 11. Time Lag

**Definition:** Time lag in ETL refers to the delay between when data changes in source systems and when it becomes available in the data warehouse.

**Types:**
1. **Extraction Lag** – Delay in capturing changes from source
2. **Processing Lag** – Time spent in transformation
3. **Loading Lag** – Time to load into warehouse

**Impact:**
- Reports may not reflect current state
- Real-time decisions may be based on stale data

**Mitigation:**
- Near-real-time ETL (micro-batches)
- CDC for faster extraction
- Parallel processing
- ELT approach (load first, transform in warehouse)

---

## 12. Formats

**Definition:** Format handling in ETL involves standardizing diverse data formats from multiple sources into a consistent warehouse format.

**Common Format Issues:**
- Date formats: DD/MM/YYYY vs MM-DD-YYYY vs YYYYMMDD
- Number formats: 1,000.00 vs 1.000,00
- Character encoding: UTF-8 vs ASCII vs Unicode
- Phone numbers: +91-9876543210 vs 09876543210
- Currency: ₹ vs INR vs Rupees

**Resolution:** Transformation rules enforce standard formats during ETL.

---

## 13. Consistency

**Definition:** Data consistency ensures that the same data item has the same value and meaning across all instances in the warehouse.

**Types:**
1. **Format Consistency** – Same representation (Male/Female not M/F mixed)
2. **Semantic Consistency** – Same meaning (revenue means same thing everywhere)
3. **Temporal Consistency** – Data reflects same time point

**Ensuring Consistency:**
- Standardization rules in ETL
- Master Data Management
- Data governance policies
- Validation checks before loading

---

## 14. Loading Concept

**Definition:** Loading is the final phase of ETL where transformed data is written into the target data warehouse.

**Loading Strategies:**

| Strategy | Description | Use Case |
|----------|-------------|----------|
| Full/Bulk Load | Load entire dataset | Initial warehouse setup |
| Incremental Load | Load only changes | Regular updates |
| Trickle Feed | Continuous small loads | Near real-time |

---

## 15. Initial and Incremental Loading

**Initial Loading:**
- First-time full population of the data warehouse
- All historical data is loaded
- Takes significant time (hours/days)
- Done once during warehouse setup

**Incremental Loading:**
- Loading only new or changed data since last load
- Uses CDC, timestamps, or flags to identify changes
- Types:
  - **Streaming/Trickle** – Continuous small batches
  - **Batch** – Scheduled periodic loads (nightly, hourly)
  
**Comparison:**

| Aspect | Initial Load | Incremental Load |
|--------|-------------|-----------------|
| Volume | Full dataset | Only changes |
| Frequency | Once | Periodic |
| Time | Long | Short |
| Complexity | Lower | Higher (CDC needed) |
| Resource Usage | Very high | Moderate |

---

## 16. Late Arriving Facts

**Definition:** Late arriving facts are fact records that arrive in the ETL pipeline after the dimension records they reference have already been processed for a later time period.

**Example:**
- Sales fact for January arrives in March ETL batch
- The dimension context (price, category) may have changed since January

**Handling Strategies:**
1. **Reprocess** – Re-run ETL for the affected period
2. **Special Processing** – Insert with correct historical dimension keys
3. **Default Dimension** – Use placeholder, update later
4. **Allocated Facts** – Distribute across appropriate time periods

**Challenges:**
- Must link to correct historical dimension version
- May require SCD Type 2 dimension lookups
- Can affect aggregate tables and OLAP cubes

---

## 17. What is Staging

**Definition:** A Staging Area is a temporary/intermediate storage area between source systems and the data warehouse, used during the ETL process.

**Purpose:**
1. **Reduce source system load** – Extract quickly, process later
2. **Data quality checkpoint** – Validate before warehouse loading
3. **Recovery point** – Restart from staging if loading fails
4. **Transformation workspace** – Complex transformations done here
5. **Audit trail** – Track what was extracted and when

**Characteristics:**
- Temporary storage (cleared after successful load)
- Minimal or no indexing (optimized for write speed)
- Flat/denormalized structure
- No end-user access

**Types:**
- **Persistent Staging** – Data retained for a period
- **Non-Persistent Staging** – Cleared after each ETL run

---

## 18. Data Marts

### 📝 PYQ: Q2 b) Create a simplified data mart and explain its purpose in a BI system [5 Marks]

**Answer:**

**Definition:** A Data Mart is a subset of a data warehouse focused on a specific business area, department, or subject.

**Purpose in BI:**
1. **Focused Analysis** – Tailored for specific department needs
2. **Faster Queries** – Smaller dataset = quicker response
3. **Simplified Access** – Users see only relevant data
4. **Reduced Load** – Offloads queries from main warehouse
5. **Security** – Department-level access control

**Types:**
- **Dependent Data Mart** – Sourced from enterprise data warehouse
- **Independent Data Mart** – Sourced directly from operational systems
- **Hybrid Data Mart** – Combination of both approaches

**Simplified Sales Data Mart Example:**

```
                    ┌─────────────────┐
                    │  Date Dimension  │
                    │─────────────────│
                    │ date_key (PK)   │
                    │ date            │
                    │ month           │
                    │ quarter         │
                    │ year            │
                    └────────┬────────┘
                             │
┌─────────────────┐    ┌────┴────────────────┐    ┌─────────────────┐
│ Product Dimension│    │   Sales Fact Table   │    │Customer Dimension│
│─────────────────│    │─────────────────────│    │─────────────────│
│ product_key (PK)│◄───│ product_key (FK)    │───►│ customer_key(PK)│
│ product_name    │    │ customer_key (FK)   │    │ customer_name   │
│ category        │    │ date_key (FK)       │    │ city            │
│ brand           │    │ store_key (FK)      │    │ segment         │
│ price           │    │ quantity_sold       │    │ region          │
└─────────────────┘    │ revenue             │    └─────────────────┘
                       │ discount            │
┌─────────────────┐    │ profit              │
│ Store Dimension  │    └─────────────────────┘
│─────────────────│              │
│ store_key (PK)  │◄─────────────┘
│ store_name      │
│ location        │
│ region          │
└─────────────────┘
```

**Sample Data:**

**Fact Table (Sales_Fact):**
| product_key | customer_key | date_key | store_key | quantity | revenue | profit |
|-------------|-------------|----------|-----------|----------|---------|--------|
| 101 | 501 | 20240115 | 1 | 5 | 25000 | 8000 |
| 102 | 502 | 20240116 | 2 | 3 | 15000 | 5000 |

**Benefits:**
- Marketing team can analyze customer segments
- Sales team can track store-wise performance
- Management can monitor quarterly trends
- Finance can track profitability

---

## 19. Cubes (OLAP Cubes)

**Definition:** A Cube is a multi-dimensional data structure that allows fast analysis of data by multiple dimensions simultaneously.

**Structure:**
- **Dimensions** – Perspectives for analysis (Time, Product, Region)
- **Measures** – Numeric values to analyze (Sales, Profit, Quantity)
- **Cells** – Intersection of dimensions containing measure values

**Example:** 3D Cube with Time × Product × Region
```
              Region →
            North  South  East
Product ↓  ┌──────┬──────┬──────┐
Laptop     │ 500K │ 300K │ 400K │  ← Time: Q1 2024
           ├──────┼──────┼──────┤
Mobile     │ 800K │ 600K │ 700K │
           ├──────┼──────┼──────┤
Tablet     │ 200K │ 150K │ 180K │
           └──────┴──────┴──────┘
```

**OLAP Operations:**
1. **Roll-Up** – Aggregate (City → State → Country)
2. **Drill-Down** – Disaggregate (Year → Quarter → Month)
3. **Slice** – Fix one dimension (only Q1 data)
4. **Dice** – Select sub-cube (Q1 + North + Laptop)
5. **Pivot/Rotate** – Change dimension orientation

**Types of OLAP:**
- **MOLAP** – Multidimensional storage (pre-computed cubes)
- **ROLAP** – Relational storage (SQL queries on star schema)
- **HOLAP** – Hybrid (aggregates in MOLAP, detail in ROLAP)

---

## 20. Data Provisioning Components (Combined)

### 📝 PYQ: Q2 c) Describe the key components of Data Provisioning, including Data Quality, Data Profiling, and Data Enrichment [5 Marks]

**Answer:**

**Data Provisioning** is the process of making data available for use in a data warehouse/BI system. It encompasses all activities from data sourcing to loading.

**Key Components:**

**1. Data Quality:**
- Ensures data is accurate, complete, consistent, and timely
- Activities: Validation, cleansing, standardization
- Example: Removing invalid email addresses, fixing misspelled names
- Tools: Informatica Data Quality, Talend Data Quality
- Dimensions: Accuracy, Completeness, Consistency, Timeliness, Validity

**2. Data Profiling:**
- Examines source data to understand structure, content, and quality
- Activities: Column analysis, dependency analysis, pattern discovery
- Example: Finding that 15% of phone numbers have invalid format
- Output: Data quality report with statistics and anomalies
- Helps plan transformation rules

**3. Data Enrichment:**
- Enhancing existing data with additional information from external/internal sources
- Activities: Appending, linking, deriving new attributes
- Example: Adding demographic data to customer records from census data
- Types: Geographic enrichment, demographic enrichment, behavioral enrichment
- Improves analytics accuracy and completeness

**How They Work Together:**
```
Source Data → [Data Profiling] → Understand data issues
                                        ↓
                              [Data Quality] → Fix identified issues
                                        ↓
                              [Data Enrichment] → Enhance clean data
                                        ↓
                              Ready for Data Warehouse
```

**Significance:**
- Data Profiling identifies WHAT is wrong
- Data Quality fixes WHAT is wrong
- Data Enrichment adds WHAT is missing
- Together they ensure the warehouse has reliable, complete data for BI

---

---

# PART B: DATA VISUALIZATION

---

## 21. What Is a Business Report

**Definition:** A Business Report is a formal document that presents data, analysis, and insights about business performance to support decision-making.

**Types:**
1. **Informational Reports** – Present facts without analysis
2. **Analytical Reports** – Include analysis and recommendations
3. **Operational Reports** – Day-to-day business metrics
4. **Strategic Reports** – Long-term trends and planning

**Characteristics:**
- Structured format
- Data-driven content
- Audience-specific
- Actionable insights
- Regular or ad-hoc generation

---

## 22. Components of Business Reporting Systems

**Key Components:**

1. **Data Source Layer:**
   - Data warehouse, data marts, OLAP cubes
   - Operational databases, external data

2. **Report Server:**
   - Processes report requests
   - Executes queries against data sources
   - Renders reports in requested format

3. **Report Design Tools:**
   - GUI for creating report layouts
   - Define data sources, queries, visualizations
   - Example: Crystal Reports, SSRS

4. **Report Distribution:**
   - Scheduling and delivery mechanisms
   - Email, portal, file share, subscriptions
   - Push vs. pull delivery

5. **Security Layer:**
   - User authentication and authorization
   - Row-level and column-level security
   - Role-based access control

6. **Report Types:**
   - Static/Canned reports
   - Parameterized reports
   - Ad-hoc/self-service reports
   - Interactive/drill-down reports

---

## 23. Data and Information Visualization

**Definition:** Data Visualization is the graphical representation of data and information using visual elements like charts, graphs, and maps.

**Purpose:**
- Make complex data understandable
- Identify patterns, trends, and outliers
- Communicate insights effectively
- Support faster decision-making

**Principles of Effective Visualization:**
1. **Clarity** – Clear message without clutter
2. **Accuracy** – Truthful representation of data
3. **Efficiency** – Maximum insight with minimum effort
4. **Aesthetics** – Visually appealing design

**Data vs Information Visualization:**

| Aspect | Data Visualization | Information Visualization |
|--------|-------------------|--------------------------|
| Focus | Raw numbers/metrics | Concepts and relationships |
| Example | Bar chart of sales | Org chart, mind map |
| Audience | Analysts | Broader audience |
| Complexity | Quantitative | Qualitative + Quantitative |

---

## 24. Types of Charts and Graphs

| Chart Type | Best For | Example Use |
|-----------|----------|-------------|
| **Bar Chart** | Comparing categories | Sales by product |
| **Line Chart** | Trends over time | Monthly revenue |
| **Pie Chart** | Parts of whole (≤5 parts) | Market share |
| **Scatter Plot** | Correlation between variables | Price vs. demand |
| **Histogram** | Distribution of values | Customer age distribution |
| **Area Chart** | Cumulative trends | Cumulative sales |
| **Heat Map** | Density/intensity patterns | Website click patterns |
| **Treemap** | Hierarchical proportions | Department budget allocation |
| **Bubble Chart** | 3 variables comparison | Revenue × Profit × Market Size |
| **Gantt Chart** | Project timelines | Project scheduling |
| **Funnel Chart** | Process/stage conversion | Sales pipeline |
| **Waterfall Chart** | Incremental changes | Profit breakdown |
| **Box Plot** | Statistical distribution | Salary range by department |
| **Gauge/Meter** | Single KPI vs target | Customer satisfaction score |

**Choosing the Right Chart:**
- Comparison → Bar/Column chart
- Trend → Line chart
- Composition → Pie/Stacked bar
- Distribution → Histogram/Box plot
- Relationship → Scatter plot

---

## 25. Visual Analytics

**Definition:** Visual Analytics combines automated analysis techniques with interactive visualizations for effective understanding, reasoning, and decision-making.

**Key Aspects:**
1. **Interactive Exploration** – Users manipulate visuals to discover insights
2. **Analytical Reasoning** – Combining human judgment with machine computation
3. **Data Transformation** – Real-time filtering, aggregating, and pivoting
4. **Collaboration** – Sharing visual insights across teams

**Process:**
```
Data → Automated Analysis → Visualization → Human Interaction → Insight → Decision
         ↑                                         |
         └─────────── Feedback Loop ───────────────┘
```

**Capabilities:**
- Drill-down and drill-through
- Dynamic filtering and highlighting
- Predictive visual models
- Anomaly detection and highlighting
- What-if analysis

---

## 26. Performance Dashboards

**Definition:** A Performance Dashboard is a visual display of the most important information needed to achieve one or more objectives, consolidated on a single screen for at-a-glance monitoring.

**Types:**
1. **Operational Dashboard** – Real-time monitoring (call center metrics)
2. **Tactical Dashboard** – Short-term analysis (weekly sales)
3. **Strategic Dashboard** – Long-term KPIs (annual goals)

**Key Design Principles:**
- Single screen display
- Minimal clutter
- KPIs with targets
- Traffic light indicators (Red/Yellow/Green)
- Drill-down capability
- Real-time or near-real-time data

**Components:**
- KPI cards/scorecards
- Sparklines and trend indicators
- Gauges and meters
- Charts and graphs
- Alert notifications
- Filters and time selectors

**Example Dashboard Layout:**
```
┌──────────────────────────────────────────────────┐
│  SALES DASHBOARD - June 2024                      │
├────────────┬────────────┬────────────┬───────────┤
│ Revenue    │ Orders     │ Customers  │ Avg Order │
│ ₹52L  ↑12%│ 1,240 ↑8% │ 890  ↑5%  │ ₹4,200 ↑3%│
├────────────┴────────────┴────────────┴───────────┤
│ [Monthly Revenue Trend - Line Chart]              │
├──────────────────────┬───────────────────────────┤
│ [Top Products - Bar] │ [Region Map - Heat Map]   │
├──────────────────────┴───────────────────────────┤
│ [Recent Orders Table]                             │
└──────────────────────────────────────────────────┘
```

---

## 27. Business Performance Management (BPM)

**Definition:** BPM (also called Corporate Performance Management/Enterprise Performance Management) is a set of processes that help organizations optimize business performance through planning, monitoring, and analysis.

**Key Components:**
1. **Strategy Management** – Define goals, KPIs, and targets
2. **Budgeting & Planning** – Financial planning and forecasting
3. **Scorecards** – Balanced Scorecard approach (Financial, Customer, Process, Learning)
4. **Reporting & Analysis** – Monitor actual vs. planned performance
5. **Dashboards** – Visual performance monitoring

**Balanced Scorecard Perspectives:**
- **Financial** – Revenue, profit, ROI
- **Customer** – Satisfaction, retention, acquisition
- **Internal Process** – Efficiency, quality, cycle time
- **Learning & Growth** – Training, innovation, culture

**BPM Cycle:**
```
Strategize → Plan → Monitor → Analyze → Adjust → (repeat)
```

---

## 28. BI Tools Overview

### Tableau
- **Type:** Visual analytics platform
- **Strengths:** Drag-and-drop, beautiful visualizations, fast
- **Best For:** Interactive dashboards, data exploration
- **Data Sources:** 70+ connectors
- **Editions:** Public (free), Desktop, Server, Online

### Power BI (Microsoft)
- **Type:** Business analytics service
- **Strengths:** Excel integration, affordable, DAX language
- **Best For:** Microsoft ecosystem users, self-service BI
- **Components:** Desktop (free), Service (cloud), Mobile
- **Key Feature:** Natural language Q&A

### Dundas BI
- **Type:** Enterprise BI platform
- **Strengths:** Highly customizable, embedded analytics
- **Best For:** White-label solutions, custom dashboards
- **Key Feature:** Open API, full customization

### Oracle BI (OBIEE)
- **Type:** Enterprise BI suite
- **Strengths:** Scalability, Oracle DB integration
- **Best For:** Large enterprises with Oracle stack
- **Components:** Answers, Dashboards, Publisher, Delivers

### MS Excel
- **Type:** Spreadsheet with BI capabilities
- **Strengths:** Universally known, pivot tables, Power Query
- **Best For:** Ad-hoc analysis, small datasets
- **BI Features:** Pivot Tables, Power Pivot, Power Query, Charts

**Comparison:**

| Feature | Tableau | Power BI | Dundas BI | Oracle BI | Excel |
|---------|---------|----------|-----------|-----------|-------|
| Cost | High | Low-Mid | Mid-High | High | Low |
| Ease of Use | High | High | Medium | Low | High |
| Scalability | High | High | High | Very High | Low |
| Visualization | Excellent | Very Good | Good | Good | Basic |
| Data Limit | Large | 1GB (free) | Large | Very Large | 1M rows |
| Best For | Analytics | Self-service | Custom/Embed | Enterprise | Ad-hoc |

---

# QUICK REVISION - Key Points for Exam

| Topic | Key Point to Remember |
|-------|----------------------|
| Data Warehouse | Subject-oriented, Integrated, Time-variant, Non-volatile |
| Star Schema | Fact table at center, dimension tables around |
| ETL | Extract → Transform → Load |
| CDC | Captures only changed data (Timestamp/Trigger/Log/Snapshot) |
| Lookup | Searches reference tables during transformation |
| Data Mart | Subset of warehouse for specific department |
| OLAP Cube | Multi-dimensional analysis (Roll-up, Drill-down, Slice, Dice) |
| Staging | Temporary area between source and warehouse |
| Late Arriving Facts | Facts that arrive after their time period has passed |
| Dashboard | Single-screen KPI monitoring |
| BPM | Strategize → Plan → Monitor → Analyze → Adjust |
| Visual Analytics | Human interaction + automated analysis |

---

*Prepared for SPPU BE IT/CS - Business Intelligence End Semester Examination*
