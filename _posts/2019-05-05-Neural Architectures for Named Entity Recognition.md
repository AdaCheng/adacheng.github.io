---
title: Neural Architectures for Named Entity Recognition
description: An introduction for a paper published in NAACL'16.
categories:
 - Paper_Note
tags:
 - Named Entity Recognition
---

![Paper](/assets/images/post/2019-05-05/00.png)  

__Guillaume Lample, Miguel Ballesteros, Sandeep Subramanian, Kazuya Kawakami, Chris Dyer__  
*Carnegie Mellon University, NLP Group of Pompeu Fabra University*  
*NAACL'16*

>[Link of Paper](https://arxiv.org/abs/1603.01360)

# Motivation
## Challenges
- Only a very small amount of supervised training data available.
- Few constraints of words that can be names.

## Solutions
1. Hand-crafted features & domain-specific knowledge
    - costly in new languages and new domains
2. Unsupervised learning
    - but to augment rather than replace

# Methods
## Input Word Embedding
### Intuitions
1. Many languages have orthographic or morphological evidence that something is a name.  
-> Use a model that constructs representations of words from representations of the characters.
2. Names appear in regular contexts in large corpora.  
-> Use embeddings learned from a large corpus that are sensitive to word order.

### Character-based models of words
#### Advantages
Useful for morphologically rich languages and to handle the out-of-vocabulary problem.

#### Framework

![Char-level](/assets/images/post/2019-05-05/01.png)  

- The embedding for a word derived from its characters is the concatenation of its forward and backward representations from the bidirectional LSTM.
- The word-level representation is from lookup table initialized by word embedding (skip-n-gram).
- Then the character-level representation is concatenated with the word-level representation. To encourage the model to depend on both representations, use dropout training.

## LSTM-CRF Model
### Traditional CRF
#### Notations
- $P$ is the matrix of scores output by the bidirectional LSTM network. $P$ is of size $n \times k$, where $k$ is the number of distinct tags, and $P_{i,j}$ is the score of the $j^{th}$ tag of the $i^{th}$ word in a sentence.
- $A$ is a matrix of transition scores such that $A_{i,j}$ represents the score of a transition from the tag $i$ to tag $j$. $y_0$ and $y_n$ are the __start__ and __end__ tags of a sentence, that we add to the set of possible tags. $A$ is therefore a square matrix of size $k + 2$.

#### Formulations
- Predictions' Scores  

![Predictions' Scores](/assets/images/post/2019-05-05/02.png)

- Training  

![Training](/assets/images/post/2019-05-05/03.png)  

![Training](/assets/images/post/2019-05-05/04.png)

- Decoding  

![Decoding](/assets/images/post/2019-05-05/05.png)

### Framework

![LSTM-CRF](/assets/images/post/2019-05-05/06.png)  

- The representation of a word using this model is obtained by concatenating its left and right context representations, $c_i = [l_i;r_i]$;
- Then $c_i$ was linearly projected onto a layer whose size is equal to the number of distinct tags.
- Use a CRF as previously described to take into account neighboring tags, yielding the final predictions for every word $y_i$.

## Transition-Based Chunking Model
### Stack Long Short-Term Memories (Reference)

![Stack-LSTM](/assets/images/post/2019-05-05/07.png)  
    
> From Paper **Transition-Based Dependency Parsing with Stack Long Shrot-Term Memory**. 

> For details, please see [my another paper note](https://adacheng.github.io/paper_note/2019/05/05/Transition-Based-Dependency-Parsing-with-Stack-Long-Short-Term-Memory/).

- The LSTM with a "stack pointer".
- The **POP** operation moves the stack pointer to the previous element.
- The **PUSH** adds a new entry at the end of the list.

### Transitions
![Transitions](/assets/images/post/2019-05-05/08.png)  

- Make use of two stacks (output and stack) and a buffer.
- The **SHIFT** transition moves a word from the buffer to the stack.
- The **OUT** transition moves a word from the buffer directly into the output stack
- The **REDUCE(y)** transition pops all items from the top of the stack creating a "chunk" labels this with label y, and pushes a representation of this chunk onto the output stack.

### Chunking Algorithm
- Use stack LSTMs to compute a fixed dimensional embedding of stack, buffer, and output, and take a concatenation of them.
- This representation is used to define a distribution over the possible actions that can be take at each time step.

### Example: Mark Watney visited Mars.
![Example](/assets/images/post/2019-05-05/09.png)

- **Representing Labeled Chunks**: run a bidirectional LSTM over the embeddings of its constituent tokens together with a token representing the type of the chunk being identified, given as $g(u, ..., v, r_y)$, where $r_y$ is a learned embedding of a label type. 

## IOBES Tagging Schemes
- I -> Inside;
- O -> Outside;
- B -> Beginning;
- E -> the End of named entities;
- S -> Singleton entities.

# Experiments
## Datasets
- CoNLL-2002 and CoNLL-2003 datasets that contain independent named entity labels for English, Spanish, German and Dutch.
- All datasets contain four different types of named entities: locations, persons, organizations, and miscellaneous entities.

## Results
### English NER results (CoNLL-2003 test set)
![Results](/assets/images/post/2019-05-05/10.png)

### German NER results (CoNLL-2003 test set)
![Results](/assets/images/post/2019-05-05/11.png)

### Dutch NER results (CoNLL-2002 test set)
![Results](/assets/images/post/2019-05-05/12.png)

### Spanish NER results (CoNLL-2002 test set)
![Results](/assets/images/post/2019-05-05/13.png)

### Different configurations (English)
![Results](/assets/images/post/2019-05-05/14.png)

# Related Works
- **Carreras (2002):** combining several small fixed-depth decision trees;
- **Florian (2003):** combining the output of four diverse classifiers;
- **Qi (2009):** proving with a neural network by doing unsupervised learning on a massive unlabeled corpus;
- **Collobert (2011):** a CNN over a sequence of word embedding with a CRF layer on top;
- **Huang (2015):** LSTM-CRF with hand-crafted spelling features;
- **Zhou and Xu (2015):** adapting to the semantic role labeling task;
- **Lin and Wu (2009):** a linear chain CRF with L2 regularization, and add phrase cluster features extracted from the web data and spelling features;
- **Passos (2014):** a linear chain CRF with spelling features and gazetteers.
- **Cucerzan and Yarowsky (1999;2002):** semi-supervised bootstrapping algorithms for NER by co-training character-level and token-level features;
- **Eisenstein (2011):** Bayesian nonparametrics to construct a database of named entities in an almost unsupervised setting;
- **Ratinov and Roth (2009):** using a regularized average perceptron and aggregating context information;
- **Gillick (2015):** think as a sequence to sequence learning problem and incorporate character-based representations into their encoder model;
- **Chiu and Nichols (2015):** use CNNs to learn character-level features.

# Future
- add some hand-crafted features or domain-spefic knowledge that is different from char-level.

