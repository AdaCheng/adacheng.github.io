---
title: Transition-Based Dependency Parsing with Stack Long Short-Term Memory
description: An introduction for a paper published in ACL'15.
categories:
 - Paper_Note
tags:
 - Dependency Parsing
---

![Paper](/assets/images/post/2019-05-05/000.png)  

__Chris Dyer, Miguel Ballesteros, Wang Ling, Austin Matthews, Noah A. Smith__  
*Marianas Labs, NLP Group of Pompeu Fabra University, Carnegie Mellon University*  
*ACL'15*

>[Link of Paper](https://arxiv.org/abs/1505.08075)


# Background
## Definition
Transition-based dependency parsing formalizes the parsing problem as a series of decisions that read words sequentially from a buffer and combine them incrementally into syntactic structures.

## Advantages
- The number of operations is linear in the length of the sentence.
- computationally efficient.

## Challenges
Modeling which action should be taken in each of the unboundedly many states encountered as the parser progresses.

## Solutions
Alternative transition sets simplify the modeling problem by making better attachment decisions, through feature engineering and more recently using neural networks.

# Method
## Stack LSTMs
![Stack-LSTM](/assets/images/post/2019-05-05/001.png)  

- The LSTM with a "stack pointer".
- The **POP** operation moves the stack pointer to the previous element.
- The **PUSH** adds a new entry at the end of the list.

## Dependency Parser
### Transition Operations
![Operations](/assets/images/post/2019-05-05/002.png) 

### Token Embeddings and OOVs
![Token Embeddings](/assets/images/post/2019-05-05/003.png)   

![Token Embeddings](/assets/images/post/2019-05-05/004.png) 

- A learned vector representation for each word type ($w$);
- A fixed vector representation from a neural language model ($w_{LM}$)
- A learned representation ($t$) of the POS tag of the token, provided as auxiliary input to the parser.
- A linear map ($V$) is applied to the resulting vector and passed through a component-wise ReLU.
- **OOVs**: stochastically replace (with $p$ = 0.5) each singleton word type in the parsing training data with the UNK token in each training iteration.
- **Pretrained word embeddings**: structured skip n-gram.

### Composition Functions
![Composition Functions](/assets/images/post/2019-05-05/005.png)  

![Composition Functions](/assets/images/post/2019-05-05/006.png) 

- The syntactic head (**h**)
- The dependent (**d**)
- The syntactic relation being satisfied (**r**)
- Concatenating the vector embeddings of the head, dependent and relation, applying a linear operator and a component-wise non-linearity.
- For the relation vector, use an embedding of the parser action that was applied to construct the relation.

### Parser Operation
#### Algorithm
- Initialized by pushing the words and their representations of the input sentence in reverse order onto $B$ such that the first word is at the top of $B$ and the ROOT symbol is at the bottom, and $S$ and $A$ each contain an empty-stack token.
- At each time step, the parser computes a composite representation of the stack states and uses that to predict an action to take, which updates the stack.
- Completes when $B$ is empty, $S$ contains two elements, one representing the full parse tree headed by the ROOT symbol and the other the empty-stack symbol, and $A$ is the history of operations taken by the parser.

#### Model
![Model](/assets/images/post/2019-05-05/007.png) 

#### Formulations
![Formulations](/assets/images/post/2019-05-05/008.png) 

- $P_t$ is the parser state representation at time $t$, which is used to determine the transition to take.
- $W$ is a learned parameter matrix.
- $b_t$ is the stack LSTM encoding of the input buffer $B$.
- $s_t$ is the stack LSTM encoding of $S$.
- $a_t$ is the stack LSTM encoding of $A$.
- $d$ is a bias term.
- Passed through a component-wise ReLU nonlinearity.

![Formulations](/assets/images/post/2019-05-05/009.png) 

- The parser state $p_t$ is used to compute the probability of the parser action at time t.

![Formulations](/assets/images/post/2019-05-05/010.png) 

- The chain rule may be invoked to write the probability of any valid sequence of parse actions $z$ conditional on the input.

# Experiments
## Datasets
### English
- The Stanford Dependency treebank contains a negligible amount of non-projective arcs (Chen and Manning, 2014).
- The part-of-speech tag are predicted by using the Stanford Tagger with an accuracy of 97.3%.
- Language model word embedding were generated from the AFP portion of the English Gigaword corpus (version 5).

### Chinese
- The Penn Chinese Tree-bank 5.1 (Zhang and Clark, 2008).
- Gold part-of-speech tags (Chen and Manning, 2014).
- Language model word embedding were generated from the complete Chinese Gigaword corpus (version 2) as segmented by the Stanford Chinese Segmenter (Tseng et al., 2005).

## Results
Exclude punctuation symbols for evaluation.

### English
![English](/assets/images/post/2019-05-05/011.png) 

### Chinese
![Chinese](/assets/images/post/2019-05-05/012.png) 

