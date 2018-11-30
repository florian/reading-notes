# Attention Is All You Need [[pdf]](https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf) [[blog post]](https://research.googleblog.com/2017/08/transformer-novel-neural-network.html) [[video 1]](https://www.youtube.com/watch?v=rBCqOTEfxvg) [[video 2]](https://www.youtube.com/watch?v=iDulhoQ2pro)

*Vaswani, Ashish, et al. "Attention is all you need." Advances in Neural Information Processing Systems. 2017.*

Encoder-decoder architectures are commonly based on recurrent or convolutional networks.
Often, they also use attention mechanisms.
The authors propose a new architecture that is entirely based on attention and does not use any convolution or recurrence.
This method is much more parallelizable.
It outperforms the established architectures on machine translation while being much faster to train.

## 1. Introduction

* As of 2017, RNN architectures, such as LSTMs and GRUs, are state-of-the-art in sequence modeling
* RNNs are inherently sequential since each hidden state is based on the previous one
* Attention makes it easier to model dependencies without regard to their distances
* This paper proposes a new architecture, the *Transformer*, that is only based on attention and is thus much easier to parallelize

## 2. Background

* There have been previous attempts at parallelizing encoder-decoder architectures, e.g. using ConvNets
* A problem with these architectures is that it is hard to relate two elements that are not close to each other
* *Self-attention* (sometimes called *intra-attention*) relates different positions of a sequence to compute a representation of that sequence
* (*Note: The authors claim that this is the first purely attention-based model, without recurrent elements. However, they also cite “A Decomposable Attention Model for Natural Language Inference” which uses attention without recurrence. The only difference is that the Transformer also produces a sequence as output*)

## 3. Model Architecture

* Encoders map the input sequence to an intermediate sequence
* The decoder uses this sequence to generate an output sequence, one element at a time
* Autoregressive: Only use previously generated symbols to generate the next
* The Transformer follows this idea using stacked self-attention

### 3.1 Encoder and Decoder Stacks

* Both encoder and decoder use six identical layers each
* Layers have sublayers
* Residual connections around each sublayer, followed by normalization
* Encoder
    * Each layer has two sublayers
    * A multi-head self-attention mechanism
    * Position-wise feed-forward net
* Decoder
    * Third layer that performs multi-head attention over the output of the encoder
    * Self-attention in the decoder only goes over previously generated outputs (to maintain the autoregressive property)
    * Attention over outputs so far is the query for the attention over the encodings

### 3.2 Attention

* Function that takes a query and a set of key-value pairs
* Output is a weighted sum of the values and weights that are determined by the compatibility between the respective keys and the query

#### 3.2.1 Scaled Dot-Product Attention

* Compatibility function is dot product, as it computes similarity
* Scaled by the square root of the dimension to avoid saturating the softmax too much
* Can be implemented using matrix multiplication
* Alternative: Additive attention uses a feed-forward net for the compatibility function
* (*Note: The authors note that dot-product attention is faster to implement because of matrix multiplication optimizations. However, it seems these same optimizations should also benefit the additive attention just as much*)

#### 3.2.2 Multi-Head Attention

* Instead of just using a single attention function, we can do it *h* times
* The results are concatenated and further processed
* This allows the model to jointly align to multiple positions
* By default, the Transformer uses 8 parallel attention heads

#### 3.2.3 Applications of Attention in our Model

* Multi-head attention is used in three different ways
* In the decoder over the output of the encoder: Query comes from the previous decoder layer
* Self-attention in the encoder: Queries, keys, values all come from the output of the previous layer
* self-attention in the decoder: Outputs not generated yet are masked out to maintain the autoregressive property

### 3.3 Position-wise Feed-Forward Network

* Network applied to each position individually
* I.e. convolution with a kernel size of 1

### 3.4 Embeddings and Softmax

* Learned embeddings
* (*Note: Why are embeddings multiplied by the sqrt of the dimension?*)

### 3.5 Positional Encoding

* Without convolution and recurrence, we are missing ordering information
* (*Note: Nice example: “time travel” vs “travel time"*)
* To add ordering information, positional encodings *PE* are added
* Sine- and cosine-based
* *PE_{pos + k}* can be represented as a linear function of *PE_{pos}* this way, for any fixed *k*
* Learned positional encodings behave very similarly, but the hardcoded ones might generalize better to longer sequences

## 4. Why Self-Attention

* Three criteria for analysis
    * Computational complexity per layer
    * Amount of computation that can be parallelized
    * Path length between long-range dependencies
* *n* is sequence length, *d* representation length
* Usually *n < d*
* With this assumption, self-attention outperforms anything else in all criteria
* Recurrent nets require *O(n)* sequential operations for processing the sequence and information has to be kept in every single one of these steps
* Convolutional (and variants of it) are faster to execute and allow information to spread more easily
* Self-attention is completely parallelizable and information can spread everywhere within a single step
* (*Note: All of this just seems to be for encoding? Decoding is autoregressive, so parallelization seems just as limited as for recurrent nets*)

## 5. Training

* WMT datasets for English-German and English-French translation
* Vocabulary of 32k words
* Sentences are batched by approximate length
* Dropout is used in a bunch of places
* A big and a small model were trained
* [Label smoothing](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.pdf) is used

## 6. Results

### 6.1 Machine Translation

* Big model outperforms anything else
* Small model outperforms anything else for German and is on par for French
* Beam search with a beam size of 4 is used to find translations

### 6.2 Model Variations

* Reducing the attention key size hurts model quality
* Nearly the same results with learned positional embeddings

## 7. Conclusion

* The Transformer replaces recurrence with multi-headed self-attention
* It is significantly faster to train
* State-of-the-art results for machine translations
* Future work: Apply Transformers to data other than text and extend the architecture to make it work well with longer sequences

---

## General Thoughts

* A lot of cool ideas that seem to make a lot of sense
* At the same time, a lot of details appear to be missing from the paper (e.g. where do initial keys come from?). This made the paper unnecessarily hard to read
* I wonder if the performance improvement mostly comes from parallelization or from generally better complexities (for *n < d*)
