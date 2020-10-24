# Reading Notes

## Books

Summarizing books in Markdown doesn't always work well.
So sometimes, I'll link to Jupyter notebooks instead.

### OpenIntro Statistics ([book](https://www.openintro.org/stat/) | [notes](https://github.com/florian/reading-notes/blob/master/books/1_OpenIntro-Statistics))

OpenIntro Statistics is a freely available textbook that's meant to introduce readers to statistics.
The book does not make many assumptions about the knowledge of readers and is generally on a fairly basic level.
It's a good first introduction to significance tests and statistical modelling.

### Reinforcement Learning: An Introduction ([book](https://mitpress.mit.edu/books/reinforcement-learning-second-edition) | [interactive notes](https://github.com/florian/reinforcement-learning))

Reinforcement Learning is a subarea of Machine Learning where there's no supervisor that tells us the optimal answer / behaviour. Instead, the feedback is delayed and we only get to know a numerical rating of our actions. *Reinforcement Learning: An Introduction* is the canonical book on Reinforcement Learning and gives a good overview over the field. The notes consist of Jupyter notebooks that explain and show implementations for most algorithms from the first two parts of the book.

### A Tour of C++ ([book](http://www.stroustrup.com/Tour.html) | [notes](books/3_A_Tour_of_Cpp))

Bjarne Stroustrup, the creator of C++, wrote a ~1300-page [book](http://www.stroustrup.com/4th.html) on C++.
The book supposedly covers most of what there is to know about C++ and starts with an overview of the fundamentals.
*A Tour of C++* is a much shorter book based on this overview.
It is meant to be read by people that are already very familiar with programming in general and might already have used some C++.
The book is great for quickly getting up to speed with C++.
There are updated editions for the newest C++ versions and the book contains some interesting insights from Stroustrup.

### Game Theory: A Nontechnical Introduction ([book](https://www.amazon.com/Game-Theory-Nontechnical-Introduction-Mathematics/dp/0486296725) | [notes](books/4_Game_Theory/README.md))

Game theory is the science of decision making. It can be used to mathematically formalize how strategies should be chosen, how voting power can be measured, or how players might cooperate. I was looking for a short introduction to the subject. In particular I wanted a book which would be faster to read than a textbook but still had mathematical rigor. After looking for such a book for quite a long time, I settled on Game Theory: A Nontechnical Introduction. It did not contain that much math but still seemed like the closest thing to what I was looking for.

### A Random Walk Down Wall Street ([book](https://www.amazon.com/Random-Walk-down-Wall-Street/dp/0393352242) | [notes](books/5_%20A_Random_Walk_Down_Wall_Street))

This investment book is split into four parts. The first part describes how people historically lost money in bubbles. The second and third give an overview of how academics and Wall Street, respectively, go about investing. In the last part, the author gives his personal recommendations for how to invest. The first edition of this book, published in 1973, was influental in recommending that something like index funds ought to be created, and popularized the approach of broadly investing into the market.

### Secrets of Sand Hill Road: Venture Capital and How to Get It ([book](https://www.amazon.com/Secrets-Sand-Hill-Road-Venture/dp/059308358X) | [notes](books/6_Secrets_of_Sand_Hill_Road))

Named after the road that most Silicon-Valley-based Venture Capital (VC) firms are located on, this book gives a thorough overview of how VC works.
It explains what kind of companies VC funds, what the life cycle of a VC-funded company is like, and how VC funds are managed.
I especially liked that the book talks a lot about incentives various players have and about how how decisions affect share dilution.

### Never Split the Difference: Negotiating as If Your Life Depended on It ([book](https://www.amazon.com/Never-Split-Difference-audio-cd/dp/1504735056) | [notes](books/7_Never_Split_the_Difference))

Written by the FBI's former head of hostage negotiation, this book describes the art of negotiation.
It outlines fundamentals to pay attention to during negotiations and a lot of stories to emphasize these points.

## Papers

1. [The Unreasonable Effectiveness of Data](papers/001_The_Unreasonable_Effectiveness_of_Data.md)
2. [Exploring the structure of a real-time, arbitrary neural artistic stylization network](papers/002_Arbitrary_Neural_Style_Transfer.md) (neural style transfer)
3. [Why Google Stores Billions of Lines of Code in a Single Repository](papers/003_Why_Google_Stores_Billions_of_Lines_of_Code_in_a_Single_Repository.md) (Google's monorepo)
4. [Are you living in a computer simulation?](papers/004_Are_you_living_in_a_computer_simulation.md) (simulation argument)
5. [Software Engineering at Google](papers/005_Software_Engineering_at_Google.md)
6. [TensorFlow: A system for large-scale machine learning](papers/006_TensorFlow_A_system_for_large-scale_machine_learning.md) (2016 whitepaper)
7. [TensorFlow: Large-Scale Machine Learning on Heterogeneous Distributed Systems](papers/007_TensorFlow_Large-Scale_Machine_Learning_on_Heterogeneous_Distributed_Systems.md) (2015 whitepaper)
8. [Generative Adversarial Nets](papers/008_Generative_Adversarial_Nets.pdf)
9. [MapReduce: Simplified Data Processing on Large Clusters](papers/009_MapReduce_Simplified_Data_Processing_on_Large_Clusters.md)
10. [FlumeJava: Easy, Efficient Data-Parallel Pipelines](papers/010_FlumeJava_Easy_Efficient_Data-Parallel_Pipelines.md) (Google's MapReduce successor)
11. [Neural Machine Translation by Jointly Learning to Align and Translate](papers/011_Neural_Machine_Translation_by_Jointly_Learning_to_Align_and_Translate.md) (attention)
12. [Reflections on Trusting Trust](papers/012_Reflections_on_Trusting_Trust.md) (Thompson’s Turing Award lecture)
13. [A Decomposable Attention Model for Natural Language Inference](papers/013_A_Decomposable_Attention_Model_for_Natural_Language_Inference.md)
14. [Attention Is All You Need](papers/014_Attention_Is_All_You_Need.md) (Transformer architecture)
15. [An Improved Data Stream Summary: The Count-Min Sketch and its Applications](papers/015_An_Improved_Data_Stream_Summary_The_Count-Min_Sketch_and_its_Applications.md)
16. [What is Data Sketching, and Why Should I Care?](papers/016_What_is_Data_Sketching_and_Why_Should_I_Care.md)

---

## Conventions in this repository

- All citations are in the MLA citation format

### Papers

- After the title, the paper is linked as a [pdf]
- Next, the paper is cited in the MLA format in italics
- This is followed by a short abstract of around three sentences
- The remaining headings roughly follow the structure of the paper
