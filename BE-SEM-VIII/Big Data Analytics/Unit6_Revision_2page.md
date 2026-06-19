# Unit 6 — Applications & Tools (2-page revision)

Purpose: concise review of major application domains (retail, finance, healthcare, supply chain) and tool summaries (Semantria, sentiment tools, HBase) with exam-focused mappings.

1. Application domains — core use-cases & quick tech mapping
- Retail
  - Use-cases: customer segmentation, recommendation systems, demand forecasting, price optimization, inventory management, market-basket analysis, churn prediction, fraud detection.
  - Tech: Hadoop/Spark for batch, Kafka for streaming, ML libraries (scikit-learn, TensorFlow), graph analysis for co-purchase networks.
- Finance
  - Use-cases: credit scoring, fraud detection (real-time), market risk analysis, algorithmic trading, AML/KYC, regulatory reporting.
  - Tech: low-latency streaming (Kafka), in-memory analytics (Spark), graph DBs (Neo4j) for network analysis, NLP for news sentiment.
- Healthcare
  - Use-cases: predictive diagnostics, EHR analytics, genomics pipelines, medical imaging (deep learning), clinical trial analytics, wearable monitoring.
  - Tech: HDFS/storage for genomics, Spark for processing, TensorFlow/PyTorch for imaging, HIPAA-compliant pipelines.
- Supply Chain
  - Use-cases: demand forecasting, route optimization, inventory optimization, supplier risk scoring, real-time tracking.
  - Tech: IoT sensors, GPS/RFID, streaming analytics, optimization libraries, digital twins.

2. Semantria (text analytics) — quick summary
- Function: cloud-based text analytics and NLP for unstructured data.
- Features: sentiment scoring, entity extraction, theme detection, categorization, summarization, intent/aspect detection, multi-language support, REST API for integration.
- Exam angle: say why Semantria suits VoC analysis and sentiment-driven dashboards.

3. AS sentiment tools — approaches & caveats
- Approaches: lexicon-based (dictionary scoring), supervised ML (SVM, neural nets), hybrid.
- Strengths: fast lexicon-based for rule-driven domains; ML scales with labeled data and handles context.
- Caveats: sarcasm, domain-specific vocabulary, multilingual issues; need domain adaptation or custom dictionaries.

4. Apache HBase — architecture & when to use
- Model: column-family NoSQL store built on HDFS; inspired by Bigtable.
- Components: HBase Master, RegionServers (serve regions), ZooKeeper (coordination), HFiles (on-disk), MemStore (in-memory buffer), WAL (durability).
- Strengths: low-latency random read/write at scale, sparse wide tables, versioning, horizontal scalability.
- Tradeoffs: not optimized for ad-hoc full-table scans (use HDFS/Parquet for analytic scans), operational complexity.

5. Exam hints: map tool → use-case
- Streaming fraud detection → Kafka + Spark Streaming + ML model (low-latency pipeline).
- Text analytics (brand monitoring) → Semantria or NLP pipeline + dashboard.
- Real-time key-value retrieval for user profiles → HBase.

6. Short examples to memorize
- Market-basket analysis: Apriori turns transactions → frequent itemsets → association rules (support, confidence, lift).
- Fraud detection: anomaly scoring per transaction, graph-based link analysis to spot rings.
