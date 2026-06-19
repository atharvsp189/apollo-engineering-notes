# Unit V: Computational Intelligence and NLP - 1 Hour Exam Revision

Use this as a quick pre-exam sheet. For most NLP answers, write: **definition + working/architecture + example + advantages + comparison table if possible**.

---

## 1. NLP Basics

**Definition:** Natural Language Processing (NLP) is a branch of AI that enables computers to understand, process, interpret, and generate human language.

### Main Challenge

Human language is symbolic, ambiguous, and context-dependent, while computers work with numbers. So NLP converts text into numerical representations that preserve useful meaning.

### Word Embeddings

**Definition:** Word embeddings are numerical vector representations of words where semantically similar words have similar vectors.

```text
king  -> [0.2, 0.5, -0.1, ...]
queen -> [0.21, 0.49, -0.12, ...]
car   -> [-0.5, 0.1, 0.9, ...]
```

---

## 2. Word Representation Techniques

## 2.1 Bag of Words (BoW)

**Definition:** Bag of Words represents a document using word counts or word presence, ignoring grammar and word order.

### Working

```text
1. Create vocabulary of unique words
2. Create vector for each document
3. Each vector position stores count/presence of a word
```

### Example

```text
Vocabulary: [I, love, machine, learning, deep]

"I love machine learning" -> [1, 1, 1, 1, 0]
"I love deep learning"    -> [1, 1, 0, 1, 1]
```

### Advantages

- Simple and fast.
- Good baseline for classification.
- Easy to implement.

### Limitations

- Ignores word order.
- Sparse and high-dimensional.
- No semantic meaning.
- Similar words are treated as unrelated.

---

## 2.2 TF-IDF

**Definition:** TF-IDF gives higher weight to words that are frequent in a document but rare in the overall corpus.

### Formula

```text
TF-IDF(t,d) = TF(t,d) * IDF(t)

TF(t,d) = count of term t in document d / total terms in d
IDF(t)  = log(N / df(t))

N     = total documents
df(t) = documents containing term t
```

### Intuition

- **TF:** Word appears often in this document, so it may be important.
- **IDF:** Word appears in many documents, so it is less informative.

### Significance

- Highlights discriminative words.
- Downweights common words.
- Better than raw BoW for search and document ranking.
- Used in information retrieval and text classification.

### Limitations

- Still ignores word order.
- Sparse vector.
- Does not capture semantic similarity.
- Cannot handle context or polysemy.

---

## 2.3 Word2Vec

**Definition:** Word2Vec is a neural embedding technique developed by Mikolov et al. at Google in 2013. It learns dense word vectors from local context windows.

### Core Idea

Words that occur in similar contexts have similar meanings.

### Two Architectures

| Model | Input | Output | Best for |
|---|---|---|---|
| CBOW | Context words | Center word | Faster, frequent words |
| Skip-gram | Center word | Context words | Better for rare words |

### CBOW

```text
Input:  The cat ___ on the mat
Given:  The, cat, on, the, mat
Output: sat
```

### Skip-gram

```text
Input:  sat
Output: The, cat, on, the, mat
```

### Properties

- Dense vectors, usually 100-300 dimensions.
- Captures semantic similarity.
- Supports analogies such as `king - man + woman ~= queen`.
- Uses neural network training.

### Limitations

- Needs large corpus.
- Static vector: same vector for a word in all contexts.
- Cannot handle polysemy well.
- Out-of-vocabulary problem.

---

## 2.4 GloVe

**Definition:** GloVe, developed by Stanford in 2014, learns word embeddings by factorizing a global word-word co-occurrence matrix.

### Core Idea

Meaning is captured from how often words co-occur across the whole corpus.

### Working

```text
1. Build co-occurrence matrix X
2. X[i][j] = count of word j appearing near word i
3. Learn vectors so dot products match log co-occurrence values
4. Final vectors become word embeddings
```

### Features

- Uses global corpus statistics.
- Combines count-based and prediction-based ideas.
- Produces dense vectors.
- Captures semantic and syntactic relationships.

---

## 3. BoW vs TF-IDF vs Word2Vec vs GloVe

| Method | Vector Type | Uses Meaning? | Uses Word Order? | Key Idea |
|---|---|---|---|---|
| BoW | Sparse | No | No | Count words |
| TF-IDF | Sparse | No | No | Weight important words |
| Word2Vec | Dense | Yes | Local context | Predict word/context |
| GloVe | Dense | Yes | Global co-occurrence | Factorize co-occurrence matrix |

---

## 4. Word2Vec vs GloVe

| Aspect | Word2Vec | GloVe |
|---|---|---|
| Type | Predictive neural model | Count + matrix factorization |
| Information used | Local context window | Global co-occurrence statistics |
| Training | Online/sliding window | Batch/global matrix |
| Main architectures | CBOW, Skip-gram | Co-occurrence objective |
| Strength | Captures local semantic patterns | Captures global corpus statistics |
| Weakness | May miss global statistics | Needs large co-occurrence matrix |
| Developer | Google, Mikolov, 2013 | Stanford, Pennington, 2014 |

### One-Line Difference

**Word2Vec learns from local context prediction; GloVe learns from global word co-occurrence counts.**

---

## 5. Neural Word Embeddings

**Definition:** Neural word embeddings are dense vector representations learned using neural networks.

### Evolution

```text
BoW / TF-IDF -> Word2Vec / GloVe -> ELMo -> BERT / GPT
sparse          dense static        contextual  deep contextual
```

### Static vs Contextual Embeddings

| Static Embeddings | Contextual Embeddings |
|---|---|
| Same vector for word always | Vector changes by sentence context |
| Word2Vec, GloVe | ELMo, BERT |
| Cannot handle polysemy well | Handles polysemy better |
| Example: "bank" has one vector | "river bank" and "bank loan" differ |

---

## 6. Neural Machine Translation (NMT)

**Definition:** Neural Machine Translation is an end-to-end neural network approach that translates a source sentence into a target sentence.

### Key Features

- Single end-to-end model.
- Handles variable-length input and output.
- Learns representations automatically.
- Produces fluent translations.
- Usually based on Seq2Seq with attention.

### NMT Architecture

```text
Source sentence
      |
      v
Encoder
      |
      v
Context / encoder states
      |
      v
Attention mechanism
      |
      v
Decoder
      |
      v
Target sentence
```

### Advantages over Statistical MT

- More fluent output.
- Better long-range dependency handling.
- No separate phrase tables/alignment models.
- Learns features automatically.
- End-to-end training.

---

## 7. Seq2Seq Model

**Definition:** Seq2Seq is an encoder-decoder neural architecture that maps one variable-length sequence to another variable-length sequence.

### Architecture

```text
Input:  x1 x2 x3 <EOS>
          |  |  |
          v  v  v
       [ Encoder RNN/LSTM/GRU ]
              |
              v
        Context vector / states
              |
              v
       [ Decoder RNN/LSTM/GRU ]
          |  |  |
          v  v  v
Output: y1 y2 y3 <EOS>
```

### Working

```text
1. Encoder reads source sequence token by token
2. Encoder produces hidden representation
3. Decoder starts with <SOS>
4. Decoder predicts target tokens one by one
5. Generation stops at <EOS>
```

### Role in NMT

- Converts source language sequence to target language sequence.
- Handles different input/output lengths.
- Learns translation end-to-end.
- Decoder generates fluent target sentence autoregressively.

### Limitation of Basic Seq2Seq

Basic Seq2Seq compresses the whole input into one fixed-size context vector, causing an **information bottleneck**, especially for long sentences.

---

## 8. Attention Mechanism

**Definition:** Attention allows the decoder to focus on relevant encoder hidden states at each output step instead of relying only on one fixed context vector.

### Working

```text
At decoding step t:

score(s_t, h_i) = relevance between decoder state s_t and encoder state h_i
alpha_ti = softmax(score values)
c_t = weighted sum of encoder states using alpha_ti
```

### Benefits

- Reduces information bottleneck.
- Improves long sentence translation.
- Gives soft alignment between source and target words.
- Allows each output word to use different source context.

---

## 9. BERT

**Definition:** BERT (Bidirectional Encoder Representations from Transformers) is a pre-trained deep bidirectional language model developed by Google in 2018. It uses Transformer encoder layers to learn contextual word representations.

### Key Innovation

BERT uses both left and right context together.

```text
Traditional LM: The bank of the ___
BERT:          The [MASK] of the river
```

### Architecture

- Based on Transformer Encoder.
- Uses multi-head self-attention.
- BERT-Base: 12 layers, 768 hidden size, 12 heads.
- BERT-Large: 24 layers, 1024 hidden size, 16 heads.

### Input Format

```text
[CLS] Sentence A [SEP] Sentence B [SEP]
Token embeddings + segment embeddings + position embeddings
```

### Pre-training Tasks

| Task | Meaning |
|---|---|
| MLM | Mask some tokens and predict them using both-side context |
| NSP | Predict whether sentence B follows sentence A |

### Fine-tuning

After pre-training, add a small task-specific output layer and train on labeled data for tasks such as classification, QA, NER, or sentiment analysis.

### Advantages over Traditional Models

| Traditional Models | BERT |
|---|---|
| Often unidirectional | Bidirectional |
| Static embeddings | Contextual embeddings |
| Task-specific training | Pre-train then fine-tune |
| Limited context | Full sentence context |
| Lower semantic understanding | Better language understanding |

### Applications

- Question answering.
- Sentiment analysis.
- Named Entity Recognition.
- Text classification.
- Semantic similarity.
- Search query understanding.
- Machine translation evaluation through BERTScore.

---

## 10. BLEU Score

**Definition:** BLEU is a traditional machine translation evaluation metric that measures n-gram overlap between candidate translation and reference translation.

### Formula

```text
BLEU = BP * geometric mean of modified n-gram precisions

BP = brevity penalty for too-short translations
```

### Calculation Steps

```text
1. Compare candidate and reference translations
2. Count matching 1-gram, 2-gram, 3-gram, 4-gram
3. Compute modified precision for each n-gram
4. Take weighted geometric mean
5. Apply brevity penalty
```

### Properties

- Range: 0 to 1.
- Higher is better.
- Fast and easy to compute.
- Precision-focused.
- Poor with synonyms and paraphrases.

---

## 11. BERTScore

**Definition:** BERTScore is a neural evaluation metric that compares candidate and reference translations using contextual BERT embeddings and cosine similarity.

### Working

```text
1. Encode candidate and reference tokens using BERT
2. Compute cosine similarity between token embeddings
3. For each token, find best matching token
4. Compute precision, recall, and F1
```

### Why Better than BLEU?

```text
Reference: The cat sat on the mat
Candidate: The cat sat on the rug

BLEU may penalize "rug" vs "mat" heavily.
BERTScore gives partial credit because the words are semantically related.
```

### Advantages

- Captures semantic similarity.
- Handles synonyms and paraphrases.
- Uses context.
- Better correlation with human judgment.

### Limitations

- Slower than BLEU.
- Needs pre-trained model.
- Less interpretable.
- Model quality affects score.

---

## 12. Traditional vs Neural MT Metrics

| Aspect | Traditional Metrics | Neural Metrics |
|---|---|---|
| Examples | BLEU, METEOR, TER | BERTScore, BLEURT, COMET |
| Basis | Exact n-gram/string overlap | Semantic embedding similarity |
| Synonyms | Poor handling | Good handling |
| Paraphrases | Poor handling | Good handling |
| Context | Limited/none | Context-aware |
| Speed | Fast | Slower |
| Hardware | CPU enough | GPU preferred |
| Interpretability | Easy | Lower |
| Human correlation | Moderate | Higher |

### Best Exam Conclusion

Traditional metrics are fast and simple, while neural metrics are semantically richer and closer to human judgment. In practice, both can be used together.

---

## 13. Neural Style Transfer (NST)

**Definition:** Neural Style Transfer is a deep learning technique that combines the content of one image with the style of another image to generate a stylized output.

### Process

```text
Content image + Style image -> Stylized output image
```

### Working

```text
1. Use pre-trained CNN such as VGG-19
2. Extract content features from higher layers
3. Extract style features from lower/multiple layers
4. Define content loss and style loss
5. Optimize output image to minimize total loss
```

### Loss

```text
Total loss = alpha * content loss + beta * style loss
```

### Applications

- Artistic image generation.
- Photo/video filters.
- Advertising and design.
- Game texture stylization.
- Fashion and creative media.

---

## 14. Must-Remember Names

| Concept | Name | Year |
|---|---|---|
| Word2Vec | Mikolov, Google | 2013 |
| GloVe | Pennington, Stanford | 2014 |
| Seq2Seq | Sutskever et al. | 2014 |
| Attention | Bahdanau et al. | 2015 |
| BERT | Devlin et al., Google | 2018 |
| Neural Style Transfer | Gatys et al. | 2015 |

---

## 15. Last-Minute Answer Templates

### If asked: "Explain BoW, TF-IDF, Word2Vec, GloVe"

Use a table. BoW counts words; TF-IDF weights important words; Word2Vec predicts local context; GloVe factorizes global co-occurrence matrix.

### If asked: "Compare Word2Vec and GloVe"

Word2Vec is predictive and local-context based. GloVe is count-based plus predictive and uses global co-occurrence statistics. Both generate dense static embeddings.

### If asked: "Explain Seq2Seq in NMT"

Write encoder-decoder architecture, variable-length input/output, source sentence encoded into context/states, decoder generates target sentence token by token, attention improves long sentences.

### If asked: "Explain BERT"

Write full form, Transformer encoder, bidirectional context, MLM and NSP pre-training, fine-tuning, contextual embeddings, and applications.

### If asked: "Compare BLEU and BERTScore"

BLEU uses exact n-gram overlap and is fast but weak with synonyms. BERTScore uses BERT embeddings and semantic similarity, so it handles meaning better but is slower.

### If asked: "Explain Neural Style Transfer"

Write content image plus style image gives stylized output. Use pre-trained CNN, content loss, style loss, total loss, and applications.

---

## 16. One-Page Memory Hook

```text
NLP = make language numerical and meaningful

BoW      -> word counts, sparse, no meaning
TF-IDF   -> important word weighting, sparse
Word2Vec -> neural, local context, CBOW/Skip-gram
GloVe    -> global co-occurrence matrix
BERT     -> Transformer encoder, bidirectional, contextual

Seq2Seq = Encoder + Decoder
Attention = decoder focuses on relevant encoder states
NMT = Seq2Seq + attention for translation

BLEU = n-gram overlap + brevity penalty
BERTScore = BERT embeddings + cosine similarity
NST = content image + style image -> stylized image

Must draw:
1. CBOW vs Skip-gram
2. Seq2Seq encoder-decoder
3. Attention flow
4. BERT input representation
5. BLEU vs BERTScore comparison table
```

