    
**Link**: [arXiv:1406.2716](https://arxiv.org/abs/1406.2716)

---

###  TL; DR  
- GloVe combines the **global statistics** of word co-occurrences with the **local efficiency**(local context) of prediction-based models (like Word2Vec).  
- It trains word embeddings by **factorizing the co-occurrence matrix**, but instead of using SVD, it optimizes a custom loss function that preserves **ratios of co-occurrence probabilities** — enabling intuitive linear analogies (e.g., *king - man + woman = queen*).

---
### Problem Statement  
Most previous models fall into two camps:
- **Count-based** models : Efficient, use co-occurrence counts, but lack fine-grained predictive structure.
- **Prediction-based** models (like Word2Vec): Learn rich representations, but ignore global word statistics.

 Can we design a model that:
- Leverages the **full co-occurrence matrix**,
- Produces **linear vector relationships**, and
- Is **computationally efficient** to train?

---

###  Key Ideas  

#### 1. Word Meaning via Co-occurrence Ratios  
Instead of just looking at co-occurrence counts, GloVe focuses on **how often word pairs co-occur relative to other words**.  
Example:
- *ice* and *steam* both co-occur with *water*, but *ice* co-occurs more with *solid*, and *steam* more with *gas*.
- So the ratio \( \frac{P (\text{solid}|\text{ice})}{P (\text{solid}|\text{steam})} \gg 1 \)

 These ratios help differentiate meaning.

---

#### 2. Objective Function (Custom Weighted Loss)  
For words \(i\) and \(j\), with co-occurrence count \(X_{ij}\), optimize:  
$$
J = \sum_{i,j=1}^{V} f(X_{ij}) \left( w_i^\top \tilde{w}_j + b_i + \tilde{b}_j - \log X_{ij} \right)^2
$$

- $$(w_i, \tilde{w}_j)$$: word and context embeddings
  
 
- $$\ (b_i, \tilde{b}_j)\ $$: word and context biases
- \(f (x)\) : weighting function to down-weight rare or overly common word pairs

---

#### 3. Weighting Function \(f (x)\)  
A smooth way to ignore noisy or dominating word pairs:  
$$
f(x) = 
\begin{cases}
(x / x_{max})^\alpha & \text{if } x < x_{max} \\
1 & \text{otherwise}
\end{cases}
$$

- Empirically: \( x_{max} = 100, \alpha = \frac{3}{4} \)
- Helps model learn better on **mid-frequency** co-occurrences.

---

#### 4. Vector Composition  
Final embedding = word vector + context vector:  
$$
v_i = w_i + \tilde{w}_i
$$

---

### Intuition  

- **Dot product** between two word vectors approximates the log of how often they co-occur.
- GloVe models the **geometry** of meaning: similar words have nearby vectors, analogies form linear structures.

---

### Experiments & Results  

####  Corpora Used
- Wikipedia 2014 + Gigaword 5 (6 B tokens)
- Twitter (27 B tokens)
- Common Crawl (840 B tokens)

#### Evaluations
- **Word Analogies** (Semantic & Syntactic): e.g. *man : king :: woman : ?*
- **Word Similarity**: Tasks like SimLex-999, WordSim-353
- **Named Entity Recognition**, **Sentiment Analysis**

#### Key Outcomes
- GloVe **outperforms Word 2 Vec** on:
  - Semantic analogy tasks (capital-country, currency, family)
  - Word similarity tasks
- Fast to train (can use pre-built co-occurrence matrix with parallel SGD)

---

### Limitations & Future Work

-  No context awareness (same embedding for “bank” in all sentences)
-  Co-occurrence matrix grows **quadratically** with vocabulary size
-  Doesn’t learn embeddings for **out-of-vocab** words directly

Inspired follow-ups:
- **FastText** (subword-aware),
- **ELMo, BERT** (contextual embeddings),
- **Retro-fitting** (injecting external knowledge into embeddings)



---


