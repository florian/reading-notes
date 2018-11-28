# A Decomposable Attention Model for Natural Language Inference [[pdf]](https://aclweb.org/anthology/D16-1244)

*Parikh, Ankur P., et al. "A decomposable attention model for natural language inference." EMNLP 2016*

Models working on sequences are usually based on RNNs or ConvNets.
The authors propose an alternative architecture based on attention.
By decomposing the attention function into two parts, it can be computed more efficiently.
This makes the architecture as efficient as RNN-based architectures.
At the same time, it works better than RNNs for problems that are easier to solve by using alignment than by aggregating all information in one vector.
The architectures are evaluated on the Natural Language Inference task.

## 1. Introduction

* *Natural Language Inference* (*NLI*) is a task where two sentences are given and one has to decide how they relate to each other
* There are three options: Neutral, entailment (one implicates the other) or contradiction
* So far, this problem has been approached using LSTMs or ConvNets
* It might be easier to solve using alignment, e.g. by aligning “cannot sleep” with “awake” or by aligning “thunder” with “sunny"
* Idea: First align sequences, then process the subresults individually and finally combine them
* The new architecture has the same complexity as LSTM encoders but can be parallelized better

## 2. Related Work

* Motivated by alignment in machine translation
* Attention is the neural counterpart to alignment

## 3. Approach

* Input is represented using word embeddings of dimension *d*
* Three steps: Attend, compare, aggregate

### 3.1 Attend

* Attention function compares two elements of the input sequences. Each element is processed individually be a feed-forward net. The final result is the dot product of the two intermediate vectors
* This decomposition allows us to avoid querying the network a quadratic number of points. Instead we can query the network once for each input element and then re-use those results
* (*Note: This seems to be the main contribution of the paper*)
* The final result are the input representations weighted by the softmax of this attention function
* One output for each input element

### 3.2 Compare

* Next, each element of the inputs is compared with its respective aligned phrase
* This is done using another feed-forward net

### 3.3 Aggregate

* Finally we sum up the resulting vectors, once for each input sequence
* The two resulting vectors and concatenated and processed by a third network

### 3.4 Intra-Sentence Attention (Optional)

* This architecture proposed so far does not use the exact positions of words
* To encode these, *intra-sentence attention* can be used
* In the *exp* of the softmax, we add a distance penalty so that words close to each other get a higher weight

## 4. Computational Complexity

* *d* is the embedding dimension and *l* the sentence length
* Generally, *l < d*
* *d* is usually at least 300, while *l* is smaller than 80
* LSTM cells have a complexity of *O(d^2)*. Since each element of the sequence is processed individually, the total complexity is *O(l \* d^2)*
* The steps of LSTMs cannot be parallelized within a layer
* Applying a feed-forward net is also *O(d^2)* so the *attend* and *compare* also have a complexity of *O(l \* d^2)*
* However, all these *l* steps could be performed in parallel
* (*Note: It seems like the complexities for LSTM cells and feed-forward nets make some assumption about the output size. It feels like the terms should rather be cubic, not quadratic, because of matrix multiplication. Still, this won’t make any difference for comparing the architectures*)

## 5. Experiments

### 5.1 Implementation Details

* Stanford Neural Language Inference (SNLI) dataset
* 550k training pairs, 10k for dev and testing each
* (*Note: These seem to be weirdly balanced*)
* *NULL* token is prepended to each sentence
* During training, sentences are padded up to some maximum length. The padding is done in a way that won’t affect the results
* (*Note: The paper does not clearly motivate it. Is it to make matrix multiplication work for easier batching?*)
* To implement this, training data is sorted into three groups: Sentences with <20 words, <50 words, and everything else
* Out-of-vocabulary words are hashed to one of 100 random embeddings
* Embeddings are 300-d and fixed during training. A matrix projects the embeddings down to 200-d and is trained
* (*Note: It seems a bit weird not to train those random embeddings*)
* 50 million steps

### 5.2 Results

* The vanilla approach achieves state-of-the-art results with an order of magnitude fewer weights
* (*Note: It’s not totally clear where this improvement comes from. Does the architecture just work so well that the individual nets can be smaller? Why?*)
* Intra-sentence attention yields another 0.5% improvement
* Examples that require inference about numbers or sequence information are harder
* Sometimes intra-attention just seems to help because word embeddings are not fine-grained enough

## 6. Conclusion

* New attention-based architecture that is trivially parallelizable
* Outperforms more complex neural architectures
* Motivation: Sometimes pairwise comparisons are more important than a global sentence-level representation
