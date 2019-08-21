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

>Given a token sequence ${w_1, \dots, w_n}$, where $n$ is the length of the token sequence. The textual encoder firstly sums the token embedding, segment embedding, positional embedding for each token to compute its input embedding.
>
>![img](/assets/images/post/2019-08-21/004.png) 
>
>Computes lexical and syntactic features $$\left\{\boldsymbol{w}_{1}, \dots, \boldsymbol{w}_{n}\right\}$$.
>
>![img](/assets/images/post/2019-08-21/005.png)
>
>$$
>\begin{equation}
> \left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\}=T-Encoder\left(\left\{w_{1}, \ldots, w_{n}\right\}\right)
> \end{equation}
> $$
> 
> Where $\text{T-Encoder}(\cdot)$ is a multi-layer bidirectional Transformer encoder.
> 

## Knowledgeable Encoder

The upper knowledgeable encoder responsible to integrate extra token-oriented knowledge information into textual information from the underlying layer, so that we can represent heterogeneous information of tokens and entities into united feature space.

> Given the entity sequence aligning to tokens as $${e_1, \dots, e_m}$$ with their entity embeddings (pre-trained by TransE) $$\left\{e_{1}, \dots, e_{m}\right\}$$, where $m$ is the length of the entity sequence. 
> 
> ![img](/assets/images/post/2019-08-21/006.png) 
> 
> Both $$\left\{\boldsymbol{w}_{1}, \dots, \boldsymbol{w}_{n}\right\}$$ and $$\left\{e_{1}, \dots, e_{m}\right\}$$ are fed into K-Encoder for fusing heterogeneous information and computing final output embeddings.
> 
> $$
> \begin{equation}
> \begin{array}{r}{\left\{\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}\right\},\left\{\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}\right\}=K-Encoder(\left\{\boldsymbol{w}_{1}, \ldots, \boldsymbol{w}_{n}\right\},\left\{\boldsymbol{e}_{1}, \dots, \boldsymbol{e}_{m}\right\})}\end{array}
> \end{equation}
> $$
> 
> $$\left\{\boldsymbol{w}_{1}^{o}, \ldots, \boldsymbol{w}_{n}^{o}\right\}$$ and $$\left\{\boldsymbol{e}_{1}^{o}, \ldots, \boldsymbol{e}_{n}^{o}\right\}$$ will be used as features for specific tasks.
> 

> > ![img](/assets/images/post/2019-08-21/007.png) 
> > 
> > For details, in the i-th aggregator, the input token embedding $$\left\{\boldsymbol{w}_{1}^{(i-1)}, \ldots, \boldsymbol{w}_{n}^{(i-1)}\right\}$$ and entity embeddings $$\left\{\boldsymbol{e}_{1}^{(i-1)}, \ldots, \boldsymbol{e}_{m}^{(i-1)}\right\}$$ are fed into two multi-head self-attentions (MH_ATTs) respectively.
> > 
> > $$
> > \begin{equation}
> > \begin{aligned}\left\{\tilde{\boldsymbol{w}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{w}}_{n}^{(i)}\right\} &=\mathrm{MH}-\mathrm{ATT}\left(\left\{\boldsymbol{w}_{1}^{(i-1)}, \ldots, \boldsymbol{w}_{n}^{(i-1)}\right\}\right) \\\left\{\tilde{\boldsymbol{e}}_{1}^{(i)}, \ldots, \tilde{\boldsymbol{e}}_{m}^{(i)}\right\} &=\mathrm{MH}-\mathrm{ATT}\left(\left\{\boldsymbol{e}_{1}^{(i-1)}, \ldots, \boldsymbol{e}_{m}^{(i-1)}\right\}\right) \end{aligned}
> > \end{equation}
> > $$
> > 
> > ![img](/assets/images/post/2019-08-21/008.png) 
> > 
> > For a token $w_j$ and its aligned entity $e_k = f(w_j)$ (in this paper, align an entity to the first token in its named entity phrase), the information fusion process is as follows.
> > 
> > $$
> > \begin{equation}
> > \begin{aligned} \boldsymbol{h}_{j} &=\sigma\left(\tilde{\boldsymbol{W}}_{t}^{(i)} \tilde{\boldsymbol{w}}_{j}^{(i)}+\tilde{\boldsymbol{W}}_{e}^{(i)} \tilde{\boldsymbol{e}}_{k}^{(i)}+\tilde{\boldsymbol{b}}^{(i)}\right) \\ \boldsymbol{w}_{j}^{(i)} &=\sigma\left(\boldsymbol{W}_{t}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{t}^{(i)}\right) \\ \boldsymbol{e}_{k}^{(i)} &=\sigma\left(\boldsymbol{W}_{e}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{e}^{(i)}\right) \end{aligned}
> > \end{equation}
> > $$
> > 
> > where $$\boldsymbol{h}_j$$ is the inner hidden state integrating the information of both the token and the entity. $$\sigma(\cdot)$$ is the non-linear activation function.
> > 
> > For the tokens without corresponding entities, the information fusion layer is as follows.
> > 
> > $$
> > \begin{equation}
> > \begin{aligned} \boldsymbol{h}_{j} &=\sigma\left(\tilde{\boldsymbol{W}}_{t}^{(i)} \tilde{\boldsymbol{w}}_{j}^{(i)}+\tilde{\boldsymbol{b}}^{(i)}\right) \\ \boldsymbol{w}_{j}^{(i)} &=\sigma\left(\boldsymbol{W}_{t}^{(i)} \boldsymbol{h}_{j}+\boldsymbol{b}_{t}^{(i)}\right) \end{aligned}
> > \end{equation}
> > $$
> > 

## Pre-training for Injecting Knowledge

Similar to BERT, this paper also adopts the Masked Language Model (MLM) and the Next Sentence Prediction (NSP) as pre-training tasks. In addtional, it propose a new pre-training task the Denoising Entity Auto-encoder (dEA) to inject knowledge into language representation by informative entities.

### Denoising Entity Auto-encoder
In order to inject knowledge into language representation by informative entities, this paper propose a new pre-training task, which randomly masks some token-entity alignments and then requires the system to predict all corresponding entities based on aligned tokens which based on the given entity sequence.

> Given the token sentence ${w_1, \dots, w_n}$ and its corresponding entity sequence $(e_1, \dots, e_m)$, define the aligned entity distribution for the token $w_i$ as follows,
> 
> $$
> \begin{equation}
> p\left(e_{j} | w_{i}\right)=\frac{\exp \left(1 i \operatorname{near}\left(\boldsymbol{w}_{i}^{o}\right) \cdot \boldsymbol{e}_{j}\right)}{\sum_{k=1}^{m} \exp \left(1 \text { inear }\left(\boldsymbol{w}_{i}^{o}\right) \cdot \boldsymbol{e}_{k}\right)}
\end{equation}
> $$

> Where $linear(\cdot)$ is a linear layer. The equation is used to compute the cross-entropy loss function for dEA.
> 

Considering that there are some errors in token-entity alignments, this paper perform the following operations for dEA:
- In 5% of the time, for a given token-entity alignment, replace the entity with another random entity, which aims to train the model to correct the errors that the token is aligned with a wrong entity;
- In 15% of the time, mask token-entity alignments, which aims to train the model to correct the errors that the entity alignment system does not extract all existing alignments;
- In the rest of the time, keep token-entity alignments unchanged, which aims to encourage the model to integrate the entity information into token representations for better language understanding.

## Fine-tuning for Specific Tasks

 ![img](/assets/images/post/2019-08-21/009.png) 

 - For various common NLP tasks
     + ERNIE can adopt the fine-tuning procedure similar to BERT. Take the final output mebedding of the first token, which corresponds to the special [CLS] token, as the representation of the input sequence for specific tasks.
 - For relation classification
     + Modify the input token sequence by adding two mark tokens to highlight entity mentions, [HD] and [TL] denote for head entities and tail entities respectively. Then, take the [CLS] token embedding for classification.
 - For entity typing
     + Modify the input with the mention mark token [ENT] can guide to combine both context information and entity mention information attentively.

## Experiments

// 实验部分等我跑了代码再补上
