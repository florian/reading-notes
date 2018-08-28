# The Unreasonable Effectiveness of Data [[pdf](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/35179.pdf)]

*Halevy, Alon, Peter Norvig, and Fernando Pereira. "The unreasonable effectiveness of data." IEEE Intelligent Systems 24.2 (2009): 8-12.*

In this paper, the authors argue that some problems, such as NLP,  can possibly not be solved by finding elegant theories as it was done in physics. Instead, we should focus on extracting information from large amounts of text. Even simple models based on a lot of data can perform extremely well, so we should be aware of how powerful data can be.

## Introduction
* Maybe there are no elegant theories that model languages as nicely as we can describe physics
* Instead, we should embrace the complexity of language and try to learn from data
* The [Brown corpus](https://www1.essex.ac.uk/linguistics/external/clmt/w3c/corpus_ling/content/corpora/list/private/brown/brown.html) contains a million labeled English words and used to be a particularly great dataset for NLP
* In 2006, Google released a [trillion-word corpus](https://books.google.com/ngrams). This data is much less clean: It is directly taken from unfiltered websites, there are spelling mistakes, and so on
* However, this dataset is much larger and thus contains much more information
* The only question is how to extract the important information and build a model from it

## Learning from Text at Web Scale
* Machine learning has solved very difficult tasks when a lot of data could be used but failed so far when not much labeled data was available
* > "But invariably, simple models and a lot of data trump more elaborate models based on less data"
* > "Simple n-gram models or linear classifiers based on millions of specific features perform better than elaborate models that try to discover general rules"
* With enough data, little more than memorization is necessary to solve problems. However, *enough* could imply a lot of data
* Throwing away rare events is a bad idea because there are many of them on the web
* Three orthogonal problems in NLP: Choosing a representation language, encoding the model in that language and performing inference on the model. The authors give some examples for these

## Semantic Web versus Semantic Interpretation
* *Semantic Web*: Conventions for representing data in a way that allows machines to read it without requiring any sort of AI
* *Semantic Interpretation*: Understanding imprecise natural language that could be ambiguous
* Creating large and high-quality datasets is important to solve the latter task
* The web is especially important here: We figured out how to (a) create an infrastructure that allows people to share content and (b) managed to aggregate and index this information. The remaining task (c) is to understand this information
* > "The same meaning can be expressed in many different ways, and the same expression can express many different meanings"

## Examples
* Consider the problem of learning synonyms. Whether two words are a synonym or not could depend on the context, e.g. for words referring to companies
* Dictionaries are not that useful for this problem. A large corpus of text from which we learn automatically is

## Conclusion
* > "Follow the data"
* Representations that can use large amounts of unlabeled data are useful
* Consider non-parametric models to really make use of the data
