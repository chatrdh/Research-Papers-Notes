[Link](https://www.cs.columbia.edu/~blei/papers/RudolphBlei2018.pdf) : 

### Core Idea

The main goal of this paper is to create a model that can track how the meanings of words change over time. Traditional word embeddings are static; they assume a word has the same meaning no matter when it was used. This paper introduces
**dynamic embeddings**, which represent a word's meaning as a vector that drifts from year to year, capturing its semantic evolution. 

### Methodology

- **Probabilistic Framework:** The model is built on **exponential family embeddings**, a probabilistic approach to word embeddings. 
    
- **Time Slices:** The authors divide large text collections into time slices, such as one for each year. 
    
- **Gaussian Random Walk:** The key innovation is treating a word's embedding vector as a **sequential latent variable**. The embedding for one year is modeled as a**Gaussian random walk** from the previous year's embedding. This means the embedding for 2015 is assumed to be the embedding from 2014 plus a small, random "drift." This enforces smoothness and ensures the embeddings are comparable across time. 
    
- **Shared Context Vectors:** While the main word embeddings (ρvt​) change over time, a set of **context vectors** (αv​) is shared across all time slices. This helps "ground" the evolving meanings in a consistent semantic space. 
    
- **Learning Objective:** The model is trained by maximizing a **pseudo log-likelihood** objective, which includes the log-likelihood of the observed data and the log priors from the Gaussian random walk. This balances fitting the data for a specific year with ensuring a smooth transition between years. 
    

### Key Results

1. **Quantitative Improvement:** In a quantitative evaluation on three different datasets, the proposed **Dynamic Embeddings (D-EMB)** consistently achieved a better fit (higher held-out likelihood) than both **Static Embeddings (S-EMB)** and **Time-Binned Embeddings (T-EMB)**, which train a separate model for each year. 
    
2. **Qualitative Exploration:** The model provides a powerful tool for exploring how language changes. The authors show several examples:
    
    - **New Meanings:** The word **COMPUTER** changed from referring to a person who computes to the electronic device we know today. 
        
    - **Shifting Dominance:** The word **BUSH** can refer to a plant or a politician. The model shows how the political meaning became dominant in the 1990s. 
        
    - **Changing Context:** The word **IRAQ** always referred to the same country, but its embedding neighborhood shifted from other regions in the  to words like "invasion," "terror," and "Saddam" in recent decades. 
        
3. **Discovering Change:** The authors introduce a metric called **absolute drift** to automatically find words whose meanings have changed the most over the entire time period. They also perform a changepoint analysis to find specific years where many words changed at once, highlighting the end of World War II (1946-1947) as a period of massive linguistic shift in U.S. Senate speeches. 