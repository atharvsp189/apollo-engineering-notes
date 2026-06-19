# Unit V: Computational Intelligence and NLP - SPPU Exam Preparation

## 📝 Answer Writing Strategy

| Marks | Structure |
|-------|-----------|
| **[5 marks]** | Definition (2 lines) + Key Points/Steps + Diagram if possible |
| **[8 marks]** | Definition + Detailed explanation of each sub-point + Examples |
| **[9 marks]** | Definition + Architecture/Working + Diagram + Example + Advantages/Comparison |

---

## 1. Introduction

**Natural Language Processing (NLP)** is a branch of AI and computational intelligence that deals with the interaction between computers and human language. It enables machines to understand, interpret, and generate human language.

**Key Challenge:** Computers work with numbers, but language is symbolic and ambiguous. We need techniques to convert text into numerical representations that capture meaning.

**Word Embeddings** are the foundation of modern NLP — they convert words into dense numerical vectors that capture semantic relationships.

---

## 2. Word Embedding Techniques

### What are Word Embeddings?
Word embeddings are numerical vector representations of words where semantically similar words are mapped to nearby points in vector space.

```
"king" → [0.2, 0.5, -0.1, 0.8, ...]
"queen" → [0.21, 0.48, -0.12, 0.79, ...]   (similar vectors!)
"car"  → [-0.5, 0.1, 0.9, -0.3, ...]       (different vector)
```

---

### 2.1 Bag of Words (BoW)

**Definition:** Bag of Words is the simplest text representation technique that represents a document as a vector of word counts (or presence), ignoring word order and grammar.

**How it works:**
1. Create a vocabulary of all unique words in the corpus
2. For each document, create a vector of length = vocabulary size
3. Each position counts how many times that word appears in the document

**Example:**
```
Corpus:
Doc1: "I love machine learning"
Doc2: "I love deep learning"
Doc3: "machine learning is great"

Vocabulary: [I, love, machine, learning, deep, is, great]

BoW Vectors:
Doc1: [1, 1, 1, 1, 0, 0, 0]
Doc2: [1, 1, 0, 1, 1, 0, 0]
Doc3: [0, 0, 1, 1, 0, 1, 1]
```

**Advantages:**
- Simple to understand and implement
- Works well for document classification
- Fast computation

**Disadvantages:**
- Ignores word order ("dog bites man" = "man bites dog")
- High dimensionality (vocabulary size can be huge)
- Sparse vectors (most entries are 0)
- No semantic meaning captured (similar words treated as unrelated)
- No context awareness

---

### 2.2 TF-IDF (Term Frequency - Inverse Document Frequency)

**Definition:** TF-IDF is a statistical measure that evaluates how important a word is to a document within a collection (corpus). It assigns higher weights to words that are frequent in a document but rare across the corpus.

**Formula:**

```
TF-IDF(t, d) = TF(t, d) × IDF(t)

Where:
  TF(t, d) = (Number of times term t appears in document d) / (Total terms in d)
  
  IDF(t) = log(N / df(t))
    N = Total number of documents in corpus
    df(t) = Number of documents containing term t
```

**Intuition:**
- **TF (Term Frequency):** Words that appear more in a document are more relevant to that document
- **IDF (Inverse Document Frequency):** Words that appear in many documents (like "the", "is") are less informative → penalized

**Example:**
```
Corpus: 3 documents
Doc1: "machine learning is great"
Doc2: "deep learning is powerful"
Doc3: "machine learning and deep learning"

For word "machine" in Doc1:
  TF("machine", Doc1) = 1/4 = 0.25
  IDF("machine") = log(3/2) = 0.176
  TF-IDF = 0.25 × 0.176 = 0.044

For word "is" in Doc1:
  TF("is", Doc1) = 1/4 = 0.25
  IDF("is") = log(3/2) = 0.176
  TF-IDF = 0.25 × 0.176 = 0.044

For word "learning" in Doc1:
  TF("learning", Doc1) = 1/4 = 0.25
  IDF("learning") = log(3/3) = 0   ← appears in ALL docs, so IDF = 0!
  TF-IDF = 0.25 × 0 = 0
```

**Significance in Text Representation:**
1. Highlights discriminative words (important for a specific document)
2. Downweights common/stop words automatically
3. Better than raw BoW for information retrieval and classification
4. Used in search engines for document ranking

**Advantages:**
- Simple and effective
- Captures word importance relative to corpus
- Reduces impact of common words

**Disadvantages:**
- Still ignores word order
- Sparse, high-dimensional vectors
- No semantic similarity captured
- Cannot handle polysemy (word with multiple meanings)

---

### 2.3 Word2Vec

**Definition:** Word2Vec is a neural network-based technique developed by Google (Mikolov et al., 2013) that learns dense, low-dimensional vector representations of words by training on large text corpora. It captures semantic relationships between words.

**Key Idea:** "You shall know a word by the company it keeps" — words that appear in similar contexts get similar vectors.

**Two Architectures:**

#### a) CBOW (Continuous Bag of Words)
- **Input:** Context words (surrounding words)
- **Output:** Predict the target/center word
- Faster to train, works well with frequent words

```
Context: "The cat ___ on the mat"
Predict: "sat"

Input: [The, cat, on, the, mat] → Model → Output: "sat"
```

#### b) Skip-gram
- **Input:** Target/center word
- **Output:** Predict context/surrounding words
- Works well with rare words, slower training

```
Input: "sat" → Model → Output: [The, cat, on, the, mat]
```

**Architecture Diagram:**
```
CBOW:                          Skip-gram:
Context words → [Hidden] → Target word     Target word → [Hidden] → Context words
  w(t-2) ──┐                                              ┌── w(t-2)
  w(t-1) ──┼→ [Projection] → w(t)        w(t) → [Projection] ──┼── w(t-1)
  w(t+1) ──┤     Layer                         Layer       ├── w(t+1)
  w(t+2) ──┘                                              └── w(t+2)
```

**Training Process:**
1. Slide a window over text corpus
2. For each position, create (input, output) training pairs
3. Train a shallow neural network (1 hidden layer)
4. After training, the hidden layer weights ARE the word embeddings

**Properties of Word2Vec:**
- Captures semantic relationships: king - man + woman ≈ queen
- Captures syntactic relationships: walking - walk + swim ≈ swimming
- Dense vectors (typically 100-300 dimensions)
- Similar words have similar vectors (cosine similarity)

**Advantages:**
- Captures semantic meaning
- Dense, low-dimensional representations
- Handles synonyms and analogies
- Computationally efficient training

**Disadvantages:**
- Requires large training corpus
- One vector per word (no polysemy handling — "bank" has one vector regardless of context)
- Out-of-vocabulary (OOV) problem for unseen words
- Context window is fixed

---

### 2.4 GloVe (Global Vectors for Word Representation)

**Definition:** GloVe is a word embedding technique developed by Stanford (Pennington et al., 2014) that combines the advantages of global matrix factorization (like LSA) and local context window methods (like Word2Vec). It learns vectors by factorizing the word-word co-occurrence matrix.

**Key Idea:** The ratio of co-occurrence probabilities encodes meaning. GloVe directly optimizes vectors to capture these ratios.

**How it works:**

1. **Build Co-occurrence Matrix (X):**
   - X[i][j] = number of times word j appears in the context of word i
   - Context defined by a sliding window over the entire corpus

2. **Define Objective Function:**
   ```
   J = Σᵢ Σⱼ f(Xᵢⱼ) × (wᵢᵀ × w̃ⱼ + bᵢ + b̃ⱼ - log Xᵢⱼ)²
   
   Where:
     wᵢ = word vector for word i
     w̃ⱼ = context vector for word j
     bᵢ, b̃ⱼ = bias terms
     f(Xᵢⱼ) = weighting function (caps influence of very frequent pairs)
   ```

3. **Train:** Minimize J using gradient descent → produces word vectors

**Example of Co-occurrence Insight:**
```
P(ice | solid) / P(ice | gas) = HIGH    → "solid" relates more to "ice"
P(steam | solid) / P(steam | gas) = LOW → "gas" relates more to "steam"
P(water | solid) / P(water | gas) ≈ 1   → "water" is neutral
```

**Advantages:**
- Uses global statistical information (entire corpus co-occurrences)
- Efficient training on co-occurrence matrix
- Produces high-quality embeddings
- Captures both semantic and syntactic patterns
- Works well even with smaller corpora than Word2Vec

**Disadvantages:**
- Memory-intensive (co-occurrence matrix can be huge)
- Still produces one vector per word (no polysemy)
- Cannot handle OOV words
- Pre-trained on static corpus (not adaptable to new domains easily)

---

## 3. Neural Word Embedding

**Definition:** Neural word embeddings are dense vector representations of words learned through neural network training. Unlike traditional methods (BoW, TF-IDF), they capture semantic meaning in continuous vector spaces.

**Key Characteristics:**
- Learned from large corpora using neural networks
- Dense, low-dimensional vectors (50-300 dimensions)
- Capture semantic and syntactic relationships
- Distributed representations (meaning spread across all dimensions)

**Types:**
1. **Word2Vec** (Google, 2013) — CBOW and Skip-gram
2. **GloVe** (Stanford, 2014) — Co-occurrence matrix factorization
3. **FastText** (Facebook, 2016) — Subword-level embeddings
4. **ELMo** (Allen AI, 2018) — Contextualized embeddings
5. **BERT** (Google, 2018) — Deep bidirectional contextualized embeddings

**Evolution:**
```
BoW/TF-IDF → Word2Vec/GloVe → ELMo → BERT/GPT
(sparse,       (dense, static,   (contextualized,  (deep contextualized,
 no semantics)  one per word)     bidirectional)     pre-trained)
```

---

## 4. Neural Machine Translation (NMT)

**Definition:** Neural Machine Translation is an approach to machine translation that uses a large neural network to predict the likelihood of a sequence of words in the target language given a source sentence.

**Key Features:**
- End-to-end learning (single model, no pipeline)
- Handles variable-length input and output
- Captures long-range dependencies
- Produces more fluent translations than statistical methods

**Architecture:** Encoder-Decoder (Seq2Seq) framework

```
Source: "Je suis étudiant"
         ↓
    [Encoder Neural Network]
         ↓
    Context Vector (fixed-size representation of source)
         ↓
    [Decoder Neural Network]
         ↓
Target: "I am a student"
```

**Components:**
1. **Encoder:** Reads source sentence → produces context representation
2. **Decoder:** Generates target sentence word-by-word using context
3. **Attention Mechanism** (modern): Allows decoder to focus on relevant parts of input at each step

---

## 5. Seq2Seq (Sequence-to-Sequence) Model

**Definition:** Seq2Seq is a neural network architecture that maps an input sequence of variable length to an output sequence of variable length. It consists of two RNNs (or LSTMs/GRUs): an Encoder and a Decoder.

### Architecture:

```
┌─────────────── ENCODER ───────────────┐   ┌──────────── DECODER ────────────────┐
│                                       │   │                                      │
│  x₁    x₂    x₃    x₄   <EOS>       │   │  <SOS>   y₁    y₂    y₃   <EOS>     │
│   ↓     ↓     ↓     ↓     ↓          │   │    ↓      ↓     ↓     ↓     ↓       │
│ [h₁]→[h₂]→[h₃]→[h₄]→[h₅]──────────────→ [s₁]→ [s₂]→[s₃]→[s₄]→[s₅]       │
│                                       │   │    ↓      ↓     ↓     ↓     ↓       │
│                                       │   │   y₁     y₂    y₃   <EOS>  STOP     │
└───────────────────────────────────────┘   └──────────────────────────────────────┘
         Context Vector (h₅) ──────────────────→ Initial state of Decoder
```

### Working:

**Encoder:**
1. Takes input sequence one token at a time: x₁, x₂, ..., xₙ
2. Each input token is converted to an embedding
3. RNN/LSTM processes embeddings sequentially
4. Final hidden state = **Context Vector** (summarizes entire input)

**Decoder:**
1. Initialized with context vector from encoder
2. Starts with special <SOS> (Start of Sentence) token
3. At each step, predicts next output token
4. Uses its own previous output as next input (autoregressive)
5. Stops when it generates <EOS> (End of Sentence) token

### Role in NMT:

1. **Encoder** reads source language sentence → compresses into context vector
2. **Context vector** captures the meaning of the source sentence
3. **Decoder** generates target language sentence from context vector

**Example:**
```
Source (French): "Je suis étudiant"
Encoder: "Je"→h₁, "suis"→h₂, "étudiant"→h₃ → Context = h₃
Decoder: Context→"I", "I"→"am", "am"→"a", "a"→"student", "student"→<EOS>
Output (English): "I am a student"
```

### Limitations of Basic Seq2Seq:
- **Information bottleneck:** Entire source compressed into single fixed-size vector
- Struggles with long sentences
- Solution: **Attention Mechanism**

### Attention Mechanism:
- Decoder can look at ALL encoder hidden states (not just last one)
- At each decoding step, computes attention weights over encoder states
- Creates a weighted combination (context) specific to each output token
- Dramatically improves translation quality for long sentences

```
Attention at step t:
  score(sₜ, hᵢ) = alignment score between decoder state sₜ and encoder state hᵢ
  αₜᵢ = softmax(scores)    ← attention weights
  cₜ = Σ αₜᵢ × hᵢ         ← context vector for step t
```

---

## 6. BERT (Bidirectional Encoder Representations from Transformers)

**Definition:** BERT is a pre-trained deep bidirectional language model developed by Google (2018) that learns contextualized word representations by jointly considering both left and right context in all layers. It is based on the Transformer architecture.

### Key Innovation: Bidirectional Context

```
Traditional (left-to-right):  "The bank of the ___" → predicts next word
BERT (bidirectional):         "The [MASK] of the river" → predicts masked word using BOTH sides
```

### Architecture:
- Based on **Transformer Encoder** (multi-head self-attention)
- BERT-Base: 12 layers, 768 hidden size, 12 attention heads, 110M parameters
- BERT-Large: 24 layers, 1024 hidden size, 16 attention heads, 340M parameters

```
Input: [CLS] The cat sat on the mat [SEP]
         ↓    ↓    ↓   ↓   ↓   ↓    ↓
    [Token Embeddings + Segment Embeddings + Position Embeddings]
         ↓
    [Transformer Layer 1 (Self-Attention + FFN)]
         ↓
    [Transformer Layer 2]
         ↓
         ... (12 or 24 layers)
         ↓
    [Contextualized Output Vectors]
```

### Pre-training Tasks:

**1. Masked Language Model (MLM):**
- Randomly mask 15% of input tokens
- Train model to predict the masked tokens
- Forces bidirectional understanding

```
Input:  "The [MASK] sat on the [MASK]"
Output: "The cat sat on the mat"
```

**2. Next Sentence Prediction (NSP):**
- Given two sentences, predict if sentence B follows sentence A
- Helps understand sentence-level relationships

```
Input: [CLS] I went to the store [SEP] I bought milk [SEP] → IsNext
Input: [CLS] I went to the store [SEP] Penguins are birds [SEP] → NotNext
```

### Fine-tuning:
After pre-training, BERT is fine-tuned on specific downstream tasks by adding a simple output layer:
- Text Classification
- Question Answering
- Named Entity Recognition
- Sentiment Analysis

### Advantages over Traditional Language Models:

| Aspect | Traditional Models (Word2Vec, LSTM LM) | BERT |
|--------|---------------------------------------|------|
| Context | Unidirectional (left-to-right or right-to-left) | Truly bidirectional |
| Word Representation | Static (same vector regardless of context) | Contextualized (different vector in different contexts) |
| Pre-training | Often task-specific or shallow | Deep pre-training on large corpus, then fine-tune |
| Transfer Learning | Limited | Excellent (one model, many tasks) |
| Polysemy | Cannot distinguish (one vector per word) | Handles naturally (context-dependent) |
| Performance | Good | State-of-the-art on many NLP benchmarks |

### Applications of BERT:
1. Question Answering (SQuAD)
2. Sentiment Analysis
3. Named Entity Recognition (NER)
4. Text Summarization
5. Machine Translation evaluation (BERTScore)
6. Search engines (Google uses BERT for query understanding)
7. Chatbots and virtual assistants

---

## 7. Metrics: BLEU Score & BERT Score

### 7.1 BLEU Score (Bilingual Evaluation Understudy)

**Definition:** BLEU is a traditional metric for evaluating machine translation quality by measuring the overlap of n-grams between the machine-generated translation (candidate) and one or more human reference translations.

**Formula:**
```
BLEU = BP × exp(Σₙ wₙ × log pₙ)

Where:
  pₙ = modified n-gram precision (ratio of matching n-grams)
  wₙ = weight for each n-gram (usually 1/N for uniform weighting)
  BP = Brevity Penalty (penalizes short translations)
  
  BP = 1                    if c > r
     = exp(1 - r/c)        if c ≤ r
  
  c = length of candidate translation
  r = length of reference translation
```

**How it works:**
1. Compute n-gram precision for n = 1, 2, 3, 4
2. Take geometric mean of precisions
3. Multiply by brevity penalty

**Example:**
```
Reference:  "The cat is on the mat"
Candidate:  "The cat sat on the mat"

Unigram matches: The, cat, on, the, mat → 5/6 = 0.83
Bigram matches:  "The cat", "on the", "the mat" → 3/5 = 0.60
```

**Properties:**
- Score range: 0 to 1 (higher = better)
- Corpus-level metric (less reliable for single sentences)
- Focuses on precision (not recall)
- n-gram based (captures local word ordering)

**Limitations:**
- Does not capture semantic similarity (synonyms scored as wrong)
- Does not handle paraphrasing well
- Requires reference translations
- Brevity penalty is crude

---

### 7.2 BERT Score

**Definition:** BERTScore is a neural evaluation metric that computes semantic similarity between candidate and reference translations using contextual embeddings from pre-trained BERT model, rather than relying on exact n-gram matching.

**How it works:**
1. Get BERT embeddings for each token in candidate and reference
2. Compute cosine similarity between all pairs of tokens
3. Calculate Precision, Recall, and F1 based on maximum similarities

```
Step 1: Embed both sentences using BERT
  Reference tokens → BERT → [r₁, r₂, r₃, ..., rₘ]
  Candidate tokens → BERT → [c₁, c₂, c₃, ..., cₙ]

Step 2: Compute pairwise cosine similarities
  sim(cᵢ, rⱼ) = cosine(cᵢ, rⱼ) for all i, j

Step 3: Compute scores
  Precision = (1/n) × Σᵢ maxⱼ sim(cᵢ, rⱼ)    [each candidate token matched to best reference token]
  Recall    = (1/m) × Σⱼ maxᵢ sim(cᵢ, rⱼ)    [each reference token matched to best candidate token]
  F1        = 2 × (P × R) / (P + R)
```

**Example:**
```
Reference: "The cat is sitting on the mat"
Candidate: "The cat is on the rug"

"rug" and "mat" → high cosine similarity in BERT space (semantically similar)
BERTScore would give higher score than BLEU (which gives 0 for "rug" vs "mat")
```

**Advantages:**
- Captures semantic similarity (synonyms, paraphrases)
- Contextualized matching (handles polysemy)
- Correlates better with human judgments
- No need for exact word matching

**Limitations:**
- Computationally expensive (requires BERT inference)
- Depends on the quality of BERT model
- Less interpretable than BLEU

---

## 8. Traditional vs Neural Metrics for Machine Translation

| Aspect | Traditional Metrics (BLEU, METEOR, TER) | Neural Metrics (BERTScore, BLEURT, COMET) |
|--------|----------------------------------------|------------------------------------------|
| **Basis** | N-gram overlap / string matching | Semantic similarity via neural embeddings |
| **Semantic Understanding** | None (exact match only) | Yes (captures meaning, synonyms) |
| **Paraphrasing** | Cannot handle | Handles well |
| **Synonym Recognition** | No ("rug" ≠ "mat") | Yes ("rug" ≈ "mat") |
| **Computation** | Fast, lightweight | Slower, GPU preferred |
| **Interpretability** | Easy to understand | Less transparent |
| **Human Correlation** | Moderate | High (better alignment with human judgment) |
| **Training Required** | No (rule-based) | Yes (needs pre-trained model) |
| **Context Sensitivity** | None (bag of n-grams) | Yes (word meaning depends on context) |
| **Robustness** | Brittle (sensitive to surface form) | Robust to lexical variation |
| **Language Support** | Any language (just needs tokenizer) | Depends on pre-trained model availability |
| **Reference Requirement** | Required | Required (but more lenient matching) |

**When to use Traditional Metrics:**
- Quick evaluation during development
- When computational resources are limited
- For comparing with prior published work
- When interpretability is important

**When to use Neural Metrics:**
- Final evaluation requiring high correlation with human judgment
- When translations involve paraphrasing
- For morphologically rich languages
- When semantic adequacy matters more than surface form

---

## 📋 EXAM ANSWERS (Model Answers)

---

### Q1. Compare and contrast Word2Vec and GloVe in terms of how they generate word embeddings? [9 marks]

**Answer:**

**Word2Vec (Google, 2013):**
Word2Vec is a neural network-based method that learns word embeddings by predicting words from their local context (or vice versa) using a shallow neural network.

**Two architectures:**
- **CBOW:** Predicts center word from surrounding context words
- **Skip-gram:** Predicts context words from center word

**Training:** Uses a sliding window over the corpus; trains on local context prediction task.

**GloVe (Stanford, 2014):**
GloVe (Global Vectors) learns embeddings by factorizing the global word-word co-occurrence matrix. It combines benefits of count-based and prediction-based methods.

**Training:** Builds co-occurrence matrix from entire corpus, then optimizes vectors to capture log co-occurrence probabilities.

**Comparison:**

| Aspect | Word2Vec | GloVe |
|--------|----------|-------|
| **Approach** | Predictive (neural network) | Count-based + Predictive (matrix factorization) |
| **Training Method** | Local context window (online) | Global co-occurrence matrix (batch) |
| **Information Used** | Local context only | Global corpus statistics |
| **Architecture** | CBOW / Skip-gram neural network | Weighted least squares on co-occurrence matrix |
| **Objective** | Predict target/context words | Minimize difference between dot product and log co-occurrence |
| **Training Speed** | Slower for large corpora | Faster (efficient matrix operations) |
| **Performance on Analogy Tasks** | Very good | Slightly better (captures global patterns) |
| **Memory** | Low (processes one window at a time) | High (stores co-occurrence matrix) |
| **Scalability** | Scales well with corpus size | Matrix grows with vocabulary |
| **Handling Rare Words** | Skip-gram handles better | May not have enough co-occurrence data |

**Similarities:**
- Both produce dense, low-dimensional vectors
- Both capture semantic relationships (king - man + woman ≈ queen)
- Both use unsupervised learning on large corpora
- Both produce static embeddings (one vector per word)
- Both cannot handle polysemy or OOV words

**Key Philosophical Difference:**
- Word2Vec: "Learn meaning from predicting context" (local, incremental)
- GloVe: "Learn meaning from how often words co-occur globally" (global, holistic)

---

### Q2. Explain the architecture of a Seq2Seq model and its role in neural machine translation? [9 marks]

**Answer:**

**Definition:** Sequence-to-Sequence (Seq2Seq) is a neural network architecture that transforms an input sequence of arbitrary length into an output sequence of arbitrary length. It was introduced by Sutskever et al. (2014) and forms the backbone of Neural Machine Translation.

**Architecture:**

The Seq2Seq model consists of two main components:

**1. Encoder:**
- An RNN (LSTM/GRU) that reads the input sequence token by token
- Produces hidden states h₁, h₂, ..., hₙ
- Final hidden state (context vector) encodes the entire input meaning

**2. Decoder:**
- Another RNN that generates the output sequence token by token
- Initialized with the context vector from the encoder
- At each step, uses previous output and hidden state to predict next token

**Architecture Diagram:**
```
INPUT: "Je suis étudiant" (French)

ENCODER:
  "Je" → [LSTM] → h₁
                    ↓
  "suis" → [LSTM] → h₂
                      ↓
  "étudiant" → [LSTM] → h₃ = Context Vector (C)
                                    ↓
DECODER:                           ↓
  <SOS> + C → [LSTM] → s₁ → Softmax → "I"
                                         ↓
  "I" + s₁ → [LSTM] → s₂ → Softmax → "am"
                                         ↓
  "am" + s₂ → [LSTM] → s₃ → Softmax → "a"
                                          ↓
  "a" + s₃ → [LSTM] → s₄ → Softmax → "student"
                                          ↓
  "student" + s₄ → [LSTM] → s₅ → Softmax → <EOS>

OUTPUT: "I am a student" (English)
```

**Working Process:**
1. Input sentence is tokenized and converted to embeddings
2. Encoder processes tokens sequentially, updating hidden state
3. Final encoder hidden state becomes the context vector
4. Decoder starts with <SOS> token and context vector
5. At each step, decoder predicts probability distribution over vocabulary
6. Most probable word is selected (greedy) or beam search is used
7. Process continues until <EOS> is generated

**Attention Mechanism (Enhancement):**
- Basic Seq2Seq suffers from information bottleneck (fixed-size context vector)
- Attention allows decoder to access ALL encoder hidden states
- At each decoding step, computes weighted sum of encoder states
- Weights indicate which input words are most relevant for current output

```
Attention weights at step t:
  αₜᵢ = softmax(score(sₜ, hᵢ))
  Context_t = Σ αₜᵢ × hᵢ
```

**Role in NMT:**
1. **End-to-end translation:** Single model handles entire translation pipeline
2. **Variable length handling:** Maps input of any length to output of any length
3. **Context understanding:** Encoder captures source sentence meaning
4. **Fluent generation:** Decoder produces natural target language text
5. **Better than phrase-based SMT:** Captures long-range dependencies, produces more fluent output

---

### Q3. How does BERT work, and what are its advantages over traditional language models? [9 marks]

**Answer:**

**Definition:** BERT (Bidirectional Encoder Representations from Transformers) is a pre-trained deep language model developed by Google (Devlin et al., 2018). It learns contextualized word representations by jointly attending to both left and right context across all layers using the Transformer architecture.

**Architecture:**
- Based on Transformer Encoder (self-attention mechanism)
- BERT-Base: 12 layers, 768 dimensions, 12 heads, 110M parameters
- BERT-Large: 24 layers, 1024 dimensions, 16 heads, 340M parameters

**How BERT Works:**

**Step 1: Input Representation**
```
Input = Token Embeddings + Segment Embeddings + Position Embeddings

[CLS] I love NLP [SEP] It is great [SEP]
  ↓    ↓   ↓   ↓    ↓    ↓  ↓   ↓     ↓
Token:  E_CLS E_I E_love E_NLP E_SEP E_It E_is E_great E_SEP
Segment: A    A    A      A     A     B    B     B      B
Position: 0   1    2      3     4     5    6     7      8
```

**Step 2: Pre-training (two tasks)**

*Task 1 — Masked Language Model (MLM):*
- Randomly mask 15% of tokens
- Model predicts masked tokens using bidirectional context
```
Input:  "The [MASK] is playing in the [MASK]"
Output: "The dog is playing in the park"
```

*Task 2 — Next Sentence Prediction (NSP):*
- Given sentence pair (A, B), predict if B follows A
- Learns inter-sentence relationships

**Step 3: Fine-tuning**
- Add task-specific output layer on top of pre-trained BERT
- Fine-tune entire model on downstream task with small learning rate
- Tasks: Classification, QA, NER, Similarity, etc.

**Advantages over Traditional Language Models:**

| Aspect | Traditional Models | BERT |
|--------|-------------------|------|
| Directionality | Unidirectional (left→right OR right→left) | Truly bidirectional (both directions simultaneously) |
| Context | Limited context window | Full sentence context in all layers |
| Embeddings | Static (Word2Vec: same vector always) | Contextualized (different vector based on context) |
| Polysemy | Cannot handle ("bank" = one vector) | Handles naturally ("river bank" ≠ "bank account") |
| Transfer Learning | Limited/no transfer | Pre-train once, fine-tune for many tasks |
| Pre-training Data | Task-specific training only | Trained on massive unlabeled data (Wikipedia, BookCorpus) |
| Performance | Good on specific tasks | State-of-the-art across 11+ NLP benchmarks |
| Feature Extraction | Manual feature engineering needed | Automatic feature learning |

**Applications:**
1. Question Answering (SQuAD benchmark)
2. Sentiment Analysis
3. Named Entity Recognition
4. Text Classification
5. Machine Translation Evaluation (BERTScore)
6. Google Search (query understanding)

---

### Q4. Describe the TF-IDF weighting scheme and its significance in text representation? [9 marks]

**Answer:**

**Definition:** TF-IDF (Term Frequency - Inverse Document Frequency) is a numerical statistic that reflects how important a word is to a document in a collection (corpus). It is widely used in information retrieval and text mining as a feature weighting scheme.

**Components:**

**1. Term Frequency (TF):**
Measures how frequently a term appears in a document.

```
TF(t, d) = (Number of times term t appears in document d) / (Total number of terms in d)

Alternative formulations:
- Raw count: f(t, d)
- Boolean: 1 if t ∈ d, else 0
- Log normalization: 1 + log(f(t, d))
```

**2. Inverse Document Frequency (IDF):**
Measures how rare/important a term is across the entire corpus.

```
IDF(t) = log(N / df(t))

Where:
  N = Total number of documents in corpus
  df(t) = Number of documents containing term t

Smooth IDF variant: log(N / (1 + df(t))) + 1
```

**3. TF-IDF Score:**
```
TF-IDF(t, d) = TF(t, d) × IDF(t)
```

**Detailed Example:**
```
Corpus (3 documents):
D1: "machine learning is fun"
D2: "deep learning is powerful"  
D3: "machine learning uses data"

For term "machine" in D1:
  TF = 1/4 = 0.25
  IDF = log(3/2) = 0.405  (appears in D1, D3 → df=2)
  TF-IDF = 0.25 × 0.405 = 0.101

For term "learning" in D1:
  TF = 1/4 = 0.25
  IDF = log(3/3) = 0  (appears in ALL documents → df=3)
  TF-IDF = 0.25 × 0 = 0  ← Common word gets zero weight!

For term "fun" in D1:
  TF = 1/4 = 0.25
  IDF = log(3/1) = 1.099  (appears only in D1 → df=1)
  TF-IDF = 0.25 × 1.099 = 0.275  ← Rare word gets high weight!
```

**Intuition:**
- Words frequent in a document BUT rare in corpus → HIGH TF-IDF (discriminative)
- Words common across all documents → LOW TF-IDF (non-discriminative)
- Stop words ("the", "is", "and") naturally get low scores

**Significance in Text Representation:**

1. **Feature Weighting:** Assigns meaningful weights to words based on their discriminative power, unlike BoW which uses raw counts.

2. **Automatic Stop Word Handling:** Common words get low IDF scores, naturally reducing their importance without explicit stop word lists.

3. **Document Ranking:** Core of search engines — documents with higher TF-IDF scores for query terms rank higher.

4. **Information Retrieval:** Helps identify which words best represent a document's unique content.

5. **Text Classification:** Provides weighted features for ML classifiers (SVM, Naive Bayes).

6. **Keyword Extraction:** High TF-IDF words in a document are likely its keywords.

7. **Document Similarity:** TF-IDF vectors enable cosine similarity measurement between documents.

**Advantages:**
- Simple, fast, effective
- No training required
- Interpretable scores
- Handles variable document lengths

**Limitations:**
- No semantic understanding (synonyms treated as different)
- Sparse, high-dimensional vectors
- Ignores word order
- Cannot handle polysemy

---

### Q5. Explain following Word Embedding Techniques: i) Bag of Words ii) TF-IDF iii) Word2Vec iv) GloVe [8 marks]

**Answer:**

#### i) Bag of Words (BoW) [2 marks]

**Definition:** BoW represents a document as a vector of word frequencies, ignoring grammar and word order.

**Process:** Create vocabulary → Count word occurrences in each document

```
Doc: "I love deep learning and love NLP"
Vocab: [I, love, deep, learning, and, NLP]
BoW:   [1, 2,    1,    1,        1,   1]
```

**Features:** Simple, fast, sparse vectors, no semantics, ignores order.

---

#### ii) TF-IDF [2 marks]

**Definition:** TF-IDF weights words by their importance — frequent in a document but rare across the corpus.

```
TF-IDF(t,d) = TF(t,d) × IDF(t)
TF = count(t in d) / total terms in d
IDF = log(N / df(t))
```

**Features:** Better than BoW for discrimination, automatically downweights common words, still sparse and no semantics.

---

#### iii) Word2Vec [2 marks]

**Definition:** Neural network-based method (Google, 2013) that learns dense vector representations by predicting words from context.

**Two models:**
- **CBOW:** Context words → predict center word
- **Skip-gram:** Center word → predict context words

**Features:** Dense vectors (100-300d), captures semantic relationships (king - man + woman ≈ queen), handles analogies, requires large corpus, one static vector per word.

---

#### iv) GloVe [2 marks]

**Definition:** GloVe (Stanford, 2014) learns word vectors by factorizing the global word-word co-occurrence matrix, combining count-based and prediction-based approaches.

**Process:** Build co-occurrence matrix → Optimize vectors so their dot product equals log co-occurrence probability.

**Features:** Uses global statistics (entire corpus), efficient training, high-quality embeddings, captures semantic + syntactic patterns, static vectors.

**Comparison Summary:**

| Method | Type | Semantics | Density | Context |
|--------|------|-----------|---------|---------|
| BoW | Count-based | No | Sparse | No |
| TF-IDF | Count-based | No | Sparse | No |
| Word2Vec | Prediction-based | Yes | Dense | Local window |
| GloVe | Count + Prediction | Yes | Dense | Global co-occurrence |

---

### Q6. Explain the process of Neural Style Transfer and discuss its applications. [5 marks]

**Answer:**

**Definition:** Neural Style Transfer (NST) is a deep learning technique that applies the artistic style of one image (style image) to the content of another image (content image), generating a new stylized image. It was introduced by Gatys et al. (2015).

**Process:**

```
Content Image (photograph) + Style Image (painting) → Stylized Output
```

**How it works:**

1. **Feature Extraction:** Use a pre-trained CNN (usually VGG-19) to extract features at different layers
   - **Lower layers:** Capture style (textures, colors, patterns)
   - **Higher layers:** Capture content (objects, shapes, structure)

2. **Define Loss Functions:**
   - **Content Loss:** Difference between content features of output and content image (at higher layers)
     ```
     L_content = ½ × Σ(F_output - F_content)²
     ```
   - **Style Loss:** Difference between Gram matrices (style features) of output and style image
     ```
     L_style = Σ (G_output - G_style)²
     ```
   - **Total Loss:** L_total = α × L_content + β × L_style

3. **Optimization:** Start with random/content image and iteratively modify pixels to minimize total loss using gradient descent.

**Applications:**
1. **Art Generation:** Create artistic versions of photographs in styles of famous paintings
2. **Photo/Video Editing:** Real-time style filters in apps (Prisma, DeepArt)
3. **Design & Advertising:** Generate creative visual content
4. **Gaming:** Stylize game environments with artistic textures
5. **Fashion:** Generate clothing designs with specific artistic patterns

---

### Q7. Discuss the significance of pre-trained NLP BERT models and provide examples of their applications. [5 marks]

**Answer:**

**Definition:** Pre-trained BERT models are deep neural language models trained on massive unlabeled text corpora that can be fine-tuned for specific downstream NLP tasks with minimal task-specific architecture changes.

**Significance of Pre-trained BERT:**

1. **Transfer Learning for NLP:** Like ImageNet models for vision, BERT provides pre-trained language understanding that transfers to many tasks — reducing the need for large labeled datasets per task.

2. **Reduced Training Time & Data:** Fine-tuning BERT on a small labeled dataset (few thousand examples) can achieve state-of-the-art results, compared to training from scratch which needs millions of examples.

3. **Contextualized Understanding:** Unlike static embeddings, BERT provides different representations for the same word in different contexts ("bank" near "river" vs "bank" near "money").

4. **Universal Feature Extractor:** One pre-trained model serves as foundation for diverse NLP tasks without task-specific architectures.

5. **State-of-the-Art Performance:** When introduced, BERT achieved best results on 11 NLP benchmarks simultaneously.

**Applications with Examples:**

1. **Question Answering:** Given a passage and question, BERT extracts the answer span (SQuAD dataset — achieved human-level performance)

2. **Sentiment Analysis:** "This movie is amazing!" → Positive. Fine-tune BERT on labeled reviews.

3. **Named Entity Recognition (NER):** "Apple launched iPhone in California" → [Apple: ORG], [iPhone: PRODUCT], [California: LOC]

4. **Text Classification:** Email spam detection, topic categorization, intent classification in chatbots

5. **Semantic Similarity:** Determine if two sentences mean the same thing (paraphrase detection)

6. **Machine Translation Evaluation:** BERTScore uses BERT embeddings to evaluate translation quality

---

### Q8. Describe the architecture of a Neural Machine Translation (NMT) model and discuss the role of Seq2Seq in NMT. [9 marks]

**Answer:**

**Definition:** Neural Machine Translation (NMT) is an end-to-end deep learning approach to machine translation that uses neural networks to directly map a source language sentence to a target language sentence.

**Architecture of NMT:**

The standard NMT architecture consists of three main components:

**1. Encoder:**
- Reads source sentence and creates a meaningful representation
- Uses bidirectional RNN/LSTM/GRU
- Produces a sequence of hidden states (one per input token)
- Final state or all states serve as input to decoder

**2. Attention Mechanism:**
- Allows decoder to focus on relevant parts of the source at each step
- Computes alignment scores between decoder state and all encoder states
- Creates weighted context vector for each output position

**3. Decoder:**
- Generates target sentence one token at a time
- Uses attention context + previous output to predict next word
- Autoregressive: each predicted word feeds into next step

**Complete Architecture Diagram:**
```
Source: "Il fait beau" (French)

┌── ENCODER (Bidirectional LSTM) ──┐
│                                   │
│  "Il"  →  [→LSTM]  [←LSTM]  → h₁ │
│  "fait" → [→LSTM]  [←LSTM]  → h₂ │
│  "beau" → [→LSTM]  [←LSTM]  → h₃ │
│                                   │
└───────────────────────────────────┘
         h₁, h₂, h₃ (encoder states)
              ↓  ↓  ↓
┌──── ATTENTION MECHANISM ─────┐
│  At each decoding step t:     │
│  αₜ = softmax(score(sₜ, hᵢ)) │
│  cₜ = Σ αₜᵢ × hᵢ             │
└──────────────────────────────┘
              ↓
┌──── DECODER (LSTM) ──────────┐
│  <SOS> + c₁ → [LSTM] → s₁ → "The"      │
│  "The" + c₂ → [LSTM] → s₂ → "weather"  │
│  "weather" + c₃ → [LSTM] → s₃ → "is"   │
│  "is" + c₄ → [LSTM] → s₄ → "nice"      │
│  "nice" + c₅ → [LSTM] → s₅ → <EOS>     │
└──────────────────────────────────────────┘

Output: "The weather is nice" (English)
```

**Role of Seq2Seq in NMT:**

1. **Foundation Architecture:** Seq2Seq (Encoder-Decoder) is the fundamental architecture that makes NMT possible. It provides the framework for mapping variable-length input to variable-length output.

2. **Meaning Compression:** The encoder in Seq2Seq compresses the source sentence into a dense representation that captures its meaning, regardless of length.

3. **Language-Independent Representation:** The context vector between encoder and decoder acts as a language-independent semantic representation.

4. **Sequential Generation:** The decoder generates the translation word by word in an autoregressive manner, capturing target language grammar and fluency.

5. **End-to-End Training:** Seq2Seq enables training the entire translation system jointly with a single loss function (cross-entropy on predicted words), unlike pipeline approaches.

6. **Handling Variable Lengths:** Unlike fixed-length methods, Seq2Seq naturally handles the fact that source and target sentences often have different lengths.

7. **Context Utilization:** With attention, Seq2Seq allows every generated word to use relevant information from any part of the source sentence.

**Advantages of NMT (Seq2Seq) over Statistical MT:**
- More fluent output
- Better handling of long-range dependencies
- Simpler architecture (no phrase tables, alignment models)
- Automatically learns representations
- Better with morphologically rich languages

---

### Q9. Define BLEU Score and BERT Score as metrics for evaluating machine translation. Compare traditional metrics with neural metrics. [9 marks]

**Answer:**

#### BLEU Score:

**Definition:** BLEU (Bilingual Evaluation Understudy) is a precision-based metric that evaluates machine translation by counting the overlap of n-grams between the candidate translation and reference translation(s).

**Formula:**
```
BLEU = BP × exp(Σₙ₌₁ᴺ wₙ × log pₙ)

pₙ = modified n-gram precision
BP = min(1, exp(1 - r/c))  [Brevity Penalty]
wₙ = 1/N (uniform weights, typically N=4)
```

**Calculation Steps:**
1. Compute precision for 1-grams, 2-grams, 3-grams, 4-grams
2. Take weighted geometric mean
3. Apply brevity penalty if candidate is shorter than reference

**Example:**
```
Reference: "The cat sat on the mat"
Candidate: "The cat on the mat"

1-gram precision: 5/5 = 1.0 (all unigrams match)
2-gram precision: 3/4 = 0.75 ("The cat", "on the", "the mat" match)
BP = exp(1 - 6/5) = 0.819 (candidate shorter)
```

**Properties:** Range [0,1], higher is better, corpus-level metric, precision-focused.

---

#### BERT Score:

**Definition:** BERTScore evaluates translation quality by computing semantic similarity between candidate and reference using contextualized embeddings from a pre-trained BERT model.

**Calculation:**
```
1. Encode reference and candidate tokens with BERT
   R = {r₁, r₂, ..., rₘ}  (reference embeddings)
   C = {c₁, c₂, ..., cₙ}  (candidate embeddings)

2. Compute token-level similarities:
   Precision = (1/n) × Σᵢ max_j cosine(cᵢ, rⱼ)
   Recall    = (1/m) × Σⱼ max_i cosine(cᵢ, rⱼ)
   F1 = 2 × P × R / (P + R)
```

**Properties:** Captures semantic similarity, handles synonyms/paraphrases, correlates highly with human judgment.

---

#### Comparison: Traditional vs Neural Metrics

| Criterion | Traditional (BLEU, METEOR, TER) | Neural (BERTScore, BLEURT, COMET) |
|-----------|--------------------------------|-----------------------------------|
| **Matching Basis** | Exact n-gram / string overlap | Semantic embedding similarity |
| **Synonym Handling** | ✗ "rug" ≠ "mat" | ✓ "rug" ≈ "mat" |
| **Paraphrase Handling** | ✗ Different surface = low score | ✓ Same meaning = high score |
| **Computation Speed** | Fast (string operations) | Slower (neural inference needed) |
| **Hardware** | CPU sufficient | GPU preferred |
| **Human Correlation** | Moderate (~0.6-0.7) | High (~0.8-0.9) |
| **Interpretability** | Easy (count matching n-grams) | Less transparent |
| **Training Needed** | None (rule-based) | Requires pre-trained model |
| **Context Awareness** | None | Yes (same word in different contexts) |
| **Word Order** | Partial (n-grams capture local order) | Captured through attention |
| **Language Coverage** | Universal (any tokenizable language) | Limited by model availability |
| **Robustness** | Brittle to surface variation | Robust to lexical diversity |
| **Reference Dependency** | Strict (needs close match) | Flexible (semantic match sufficient) |

**Conclusion:**
- Traditional metrics are fast, simple, and widely used as baseline
- Neural metrics provide better alignment with human judgment but at higher computational cost
- Best practice: Use both — BLEU for quick comparison, BERTScore for final evaluation

---

## 🎯 Quick Revision Points for Exam Day

1. **Key Formulas to Remember:**
   - TF-IDF = TF × IDF, where IDF = log(N/df)
   - BLEU = BP × geometric mean of n-gram precisions
   - BERTScore: Precision = avg of max cosine similarities

2. **Key Names & Years:**
   - Word2Vec: Mikolov, Google, 2013
   - GloVe: Pennington, Stanford, 2014
   - Seq2Seq: Sutskever et al., 2014
   - BERT: Devlin et al., Google, 2018
   - Attention: Bahdanau et al., 2015

3. **Draw diagrams for:**
   - Seq2Seq encoder-decoder architecture
   - CBOW vs Skip-gram
   - BERT input representation

4. **Comparison tables work great for:**
   - Word2Vec vs GloVe
   - Traditional vs Neural metrics
   - BoW vs TF-IDF vs Word2Vec vs GloVe

5. **For 9-mark questions:** Definition + Architecture/Diagram + Working Steps + Example + Advantages/Significance
