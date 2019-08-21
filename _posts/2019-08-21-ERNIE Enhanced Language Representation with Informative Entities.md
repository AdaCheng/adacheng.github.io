---
title: ERNIE, Enhanced Language Representation with Informative Entities
description: An introduction for a paper published in ACL'19.
categories:
 - Paper_Note
tags:
 - Pretraining
 - Knowledge Graph
---

![Paper](/assets/images/post/2019-08-21/001.png)  

__Zhengyan Zhang, Xu Han, Zhiyuan Liu, Xin Jiang, Maosong Sun, Qun Liu__  
*Tsinghua University*  
*ACL'19*

>[Linke of Paper](https://arxiv.org/abs/1905.07129)

# Motivations
- The existing pre-trained language models rarely consider incorporating knowledge graphs (KGs), which can provide rich strcutured knowledge facts for better language understanding.
- Considering rich knowledge information can benefit various knowledge-driven applications, such as entity typing and relation classification.

> eg. Entity Typing Task
> 
> ![img](/assets/images/post/2019-08-21/002.png) 
> 
> Without knowing *Blowin' in the Wind* and *Chronicles: Volume One* are *song* and *book* respectively, it is difficult to recognize the two occupations of *Bob Dylan*, i.e., *songwriter* and *writer*.

# Challenges
For incorporating external knowledge into language representation models, there are two main challenges:

- **Structured Knowledge Encoding**: how to effectively extract and encode its related informative facts in KGs.
    + Solution in ERNIE: 
        * Recognize named entity mentions in text.
        * Align mentions to their corresponding entities in KGs.
        * Encode KGs with knowledge embedding algorithms like TransE.
        * Take the informative entity embedding as input.
- **Heterogeneous Information Fusion**: The pre-training procedure for language representation is quite different from the knowledge representation procedure, leading to two individual vector spaces.
    + Solution in ERNIE:
        * Design a new pre-training objective by randomly masking some of the named entity alignments in the input text.
        * Ask the model to select appropriate entities from KGs to complete the alignments.
        * The objectives require models to aggregate both context and knowledge facts for predicting both tokens and entities.

# Architecture

![img](/assets/images/post/2019-08-21/003.png) 

## Textual Encoder

The underlying textual encoder responsible to capture basic lexical and syntactic information from the input tokens.

Given a token sequence ${w_1, \dots, w_n}$, where $n$ is the length of the token sequence. The textual encoder firstly sums the token embedding, segment embedding, positional embedding for each token to compute its input embedding.

![img](/assets/images/post/2019-08-21/004.png) 

Computes lexical and syntactic features $\left\{\boldsymbol{w}_{1}, \dots, \boldsymbol{w}_{n}\right\}$.

$$
\begin{equation}
\left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\}=\mathrm{T}-\text { Encoder }\left(\left\{w_{1}, \ldots, w_{n}\right\}\right)
\end{equation}
$$

Where $\text{T-Encoder}(\cdot)$ is a multi-layer bidirectional Transformer encoder.

![img](/assets/images/post/2019-08-21/005.png) 

## Knowledgeable Encoder

The upper knowledgeable encoder responsible to integrate extra token-oriented knowledge information into textual information from the underlying layer, so that we can represent heterogeneous information of tokens and entities into united feature space.

Given the entity sequence aligning to tokens as ${e_1, \dots, e_m} with their entity embeddings (pre-trained by TransE) $\left\{e_{1}, \dots, e_{m}\right\}$, where $m$ is the length of the entity sequence. 

Both $\left\{\boldsymbol{w}_{1}, \dots, \boldsymbol{w}_{n}\right\}$ and $\left\{e_{1}, \dots, e_{m}\right\}$ are fed into K-Encoder for fusing heterogeneous information and computing final output embeddings.

$$
\begin{equation}
\begin{array}{r}{\left\{\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}\right\},\left\{\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}\right\}=\mathrm{K}-\text { Encoder }( \left.\quad\left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\},\left\{\boldsymbol{e}_{1}, \dots, \boldsymbol{e}_{m}\right\}\right)}\end{array}
\end{equation}
$$

${\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}}$ and ${\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}}$ will be used as features for specific tasks.

For details,

$$
\begin{equation}
\begin{aligned}\left\{\tilde{\boldsymbol{w}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{w}}_{n}^{(i)}\right\} &=\mathrm{MH}-\mathrm{ATT}\left(\left\{\boldsymbol{w}_{1}^{(i-1)}, \ldots, \boldsymbol{w}_{n}^{(i-1)}\right\}\right) \\\left\{\tilde{\boldsymbol{e}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{e}}_{m}^{(i)}\right\} &=\mathrm{MH}-\mathrm{ATT}\left(\left\{\boldsymbol{e}_{1}^{(i-1)}, \ldots, \boldsymbol{e}_{m}^{(i-1)}\right\}\right) \end{aligned}
\end{equation}
$$



