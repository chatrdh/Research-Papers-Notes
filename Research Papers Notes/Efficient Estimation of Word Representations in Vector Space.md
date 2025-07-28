(Original Word2vec paper)
**Link**: [arXiv:1301.3781](https://arxiv.org/abs/1301.3781)

---
###  TL; DR
- Introduced **Word2Vec**, a way to learn distributed word representations efficiently using neural networks.  
- Two architectures: **CBOW** (predict target word from context) and **Skip-Gram** (predict context from target).  
- Leads to word vectors where semantic relationships emerge (e.g., *king - man + woman ≈ queen*).

---
### Problem Statement
- Traditional n-gram models are computationally expensive and poor at generalization.
- How can we create *dense vector representations* of words that capture semantic relationships, efficiently and at scale?

---
### Key Ideas
- Train shallow neural networks to **predict context** or **predict the target word**, depending on architecture.
- Use **continuous bag-of-words (CBOW)** or **Skip-Gram**:
  - CBOW: predicts a word given surrounding words.
  - Skip-Gram: predicts surrounding words given a word.
- Introduced **hierarchical softmax** and **negative sampling** to reduce computation.

---
### Method

#### CBOW Architecture
- Input: context words (e.g., “the *cat* sat on the” → predict “mat”).
- Averages the embeddings of context words.
- Passes through a single projection layer to predict the target.

#### Skip-Gram Architecture
- Input: a single word → predict context words.
- Works better for rare words and large corpora.

#### Efficiency Tricks
- **Hierarchical Softmax**: binary tree over vocabulary (log (V) vs V).
- **Negative Sampling**: randomly sample negative examples instead of computing softmax across full vocab.

---

### Experiments & Results
- Trained on Google News (100 B tokens), Wikipedia, and more.
- Visualizations show semantic clustering (e.g., countries, cities, genders).
- Word vector arithmetic demonstrated: *Paris - France + Italy ≈ Rome*.
- Achieved strong results on analogy tasks.

---

### Limitations & Future Work
- Ignores word order and sentence-level structure.
- One embedding per word → struggles with polysemy (*bank* the river vs. *bank* the money).
- Can be extended using context-aware methods (e.g., ELMo, BERT later did this).
---


