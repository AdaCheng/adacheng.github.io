---
title: COMET, Commonsense Transformers for Automatic Knowledge Graph
description: An introduction for a paper published in ACL'19.
categories:
 - Paper_Note
tags:
 - Commonsense
---

![Paper](/assets/images/post/2019-11-02/01.png)

__Antoine Bosselut, Hannah Rashkin, Maarten Sap, Chaitanya Malaviya, Asli Celikyilmaz, Yejin Choi__  
*Allen Institute for Artificial Intelligence, Seattle, WA, USA*  
*Paul G. Allen School of Computer Science & Engineering, Seattle, WA, USA*  
*Microsoft Research, Redmond, WA, USA*  
*ACL'19*  

>[Link of Paper](https://arxiv.org/abs/1906.05317)  
>[Link of Demo](https://mosaickg.apps.allenai.org/)  
>[Link of Code](https://github.com/atcbosselut/comet-commonsense)  

# Background
## Commonsense KBs
Contrary to many conventional KBs that store knowledge with canonival templates, commonsense KBs only store loosely structured open-text descriptions of knowledge.

> For example,  
> a ConceptNet tuple relating to "taking a nap" would be: (s = "take a nap", r = Causes, o = "have energy").  

## Motivation
1. Commonsense knowledge does not cleanly fit into a schema comparing two entities with a known relation.
2. Current approaches can only capture knowledge that is explicitly mentioned in text, limiting their applicability for capturing commonsense knowledge, which is often implicit.

## Solution
This paper casts commonsense acquisition as knowledge base construction and investigate whether large-scale language models can effectively learn to generate the knowledge necessary to automatically construct a commonsense knowledge base(KB).

## Contribution
1. Develop a generative approach to knowledge base construction.
2. Develop a framework for using large-scale transformer language models to learn to produce commonsense knowledge tuples.
3. Perform an empirical study on the quality, novelty, and diversity of the commonsense knowledge produced by this paper for two domains, ATOMIC and ConceptNet, as well as an efficiency study on the number of seed tuples needed to learn an effective knowledge model.

# Method
## Task
### Definition
COMET is given a training knowledge base of natural language tuples in 
{$s, r, o$} format, where $s$ is the phrase subject of the tuple, $r$ is the relation of tuple, and $o$ is the phrase object of the tuple. The task is to generate $o$ given $s$ and $r$ as inputs.

![Structure](/assets/images/post/2019-11-02/04.png)

### Notation
- $$X^{s}=\left\{x_{0}^{s}, \ldots, x_{\|s\|}^{s}\right\}$$ as the tokens that make up the subject of the relation.
- $$X^{r}=\left\{x_{0}^{r}, \ldots, x_{\|r\|}^{r}\right\}$$ as the tokens that make up the relation of the tuple.
- $$X^{o}=\left\{x_{0}^{o}, \ldots, x_{\|o\|}^{o}\right\}$$ as the tokens that make up the object of the tuple.
- The embedding for any word $x$ is denoted as $e$.

## Structure
### Transformer Langague Model
Use the transformer language model architecture introduced in [GPT](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf), which uses multiple transformer blocks of multi-headed scaled dot product attention and fully connected layers to encode input text.

![Structure](/assets/images/post/2019-11-02/02.png)

> Defualt readers are familiar with GPT, we will not explore it in this article. If you are interested in this part, please check my another article (todo).

### Input Encoder
Represent a knowledge tuple {$s, r, o$} as a concatenated sequence of the words of each item of the tuple:

$$
\begin{equation}
\mathbf{X}=\left\{X^{S}, X^{r}, X^{o}\right\}
\end{equation}
$$

For any input word $$x_{t} \in \mathbf{X}$$, the encoding of the input is the sum of its word embedding, $e_t$ with a position embedding encoding its absolute position (becuase the transformer has no concept of ordering of tokens) in the sequence $X$:

$$
\begin{equation}
h_{t}^{0}=e_{t}+p_{t}
\end{equation}
$$

where $p_t$ is the position embedding for time step $t$, and $h_0$ is the input to the first transformer layer.

## Experiment
### Training
COMET is trained to learn to generate the token of $o$: $X^o$ by given the concatenation of the tokens of $s$ and $r$: $\left[X^{s}, X^{r}\right]$ as input.

#### Dataset
COMET relies on a seed set of knowledge tuples from an existing KB to learn to produce commonsense knowledge.

In this work, use:
- [ATOMIC](https://arxiv.org/abs/1811.00146)
    + [download the data](https://storage.googleapis.com/ai2-mosaic/public/atomic/v1.0/atomic_data.tgz)
- [ConceptNet](https://arxiv.org/abs/1612.03975)
    + [download the data](https://s3.amazonaws.com/conceptnet/downloads/2019/edges/conceptnet-assertions-5.7.0.csv.gz)

#### Input Token Setup
The tokens in $s$, $r$, and $o$ are organized for different training tasks.

![input_token](/assets/images/post/2019-11-02/03.png)

#### Loss Function
Maximize the conditional loglikelihood of predicting the phrase object tokens, $X^o$:

$$
\begin{equation}
\mathcal{L}=-\sum_{t=|s|+|r|}^{|s|+|r|+|o|} \log P\left(x_{t} | x_{<t}\right)
\end{equation}
$$

where $\|s\|$, $\|r\|$, and $\|o\|$ are the number of tokens in the subject phrase, relation, and object phrase, respectively.

#### Initialization
- Parameters are initialized to the final language model weights from GPT.
- Additional special tokens that are added to the vocabulary for fine tuning (e.g., relation embeddings such as *oReact* for ATOMIC and *IsA* for ConceptNet) are initialized by sampling from the standard normal distribution.

#### Hyperparameters
- 12 layers, 768-dimensional hidden states, and 12 attention heads.
- A dropout rate of 0.1.
- Use GeLU units as activation function.
- Batch-size is 64.

### ATOMIC Experiments
#### ATOMIC Dataset
The [ATOMIC dataset](https://storage.googleapis.com/ai2-mosaic/public/atomic/v1.0/atomic_data.tgz) contains 877K tuples covering a variety of social commonsense knowlegde around specific event prompts.

> For example,  
> a ATOMIC tuple: (event = "X goes to the store" = phrase subject $s$, inference dimension = xIntent = phrase relation $r$, cause/effect = "to get food" = phrase object $o$).

Split the dataset to 710k training, 80k development, and 87k test tuples respectively.

#### Result
##### Baseline
The model trained in [ATOMIC](https://arxiv.org/abs/1811.00146) that use LSTM sequence-to-sequence models to encode the input subject and relation and produce an output object.

##### Automatic Evaluation Metric

![Result](/assets/images/post/2019-11-02/05.png)

- Perplexity of the model on its gold generations (PPL)
- BLEU-2
- The proportion of generated tuples and generated objects which are not in the training set
    + The proportion of all generated tuples that are novel (N/T *sro*)
    + The proportion of all generated tuples that have a novel object (N/T *o*)
    + The number of novel objects as a function of the set of unique objects produced for all test set events (N/U *o*)

##### Human Evaluation

![Result](/assets/images/post/2019-11-02/06.png)

Using workers from Amazon Mechanical Turk (AMT).
- n = 5000 (100 events $\times$ 5 workers $\times$ 10 candidates) ratings are produced per relation
    + Evaluate 100 randomly selected events from the test set.
    + For each event and relation type, 10 candidates are generated using beam search.
    + The full beam is evaluated by five different workers.

##### Effect of decoding algorithm

![Result](/assets/images/post/2019-11-02/07.png)

##### Efficiency of learning from seed tuples

![Result](/assets/images/post/2019-11-02/08.png)

### ConceptNet Experiments
#### ConceptNet Dataset
The [ConceptNet dataset](https://s3.amazonaws.com/conceptnet/downloads/2019/edges/conceptnet-assertions-5.7.0.csv.gz) consists of tuples obtained from the Open Mind Common Sense (OMCS) entries in ConceptNet 5.

> For example,  
> a ConceptNet tuple: (Subject $s$= "take a nap", Relation $r$ = "Causes", Object $o$ = "have energy").

The most confident 1200 tuples were used to create the test set, while the next 1200 tuples were used to create two development sets. The 100k version of the training set was used to train models, which contains 34 relation types.

#### Result
##### Mertic

![Result](/assets/images/post/2019-11-02/09.png)

- The number of generated positive examples in the test set that are scored as correct by the [pre-trained Bilinear AVG model](https://www.aclweb.org/anthology/P16-1137/). (Score)

