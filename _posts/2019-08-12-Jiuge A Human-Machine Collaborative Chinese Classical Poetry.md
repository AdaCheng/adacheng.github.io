---
title: Jiuge, A Human-Machine Collaborative Chinese Classical Poetry Generation System
description: An introduction for a paper published in ACL'19.
categories:
 - Paper_Note
tags:
 - Poetry Generation
---

![Paper](/assets/images/post/2019-08-12/001.png)  

__Zhipeng Guo, Xiaoyuan Yi, Maosong Sun, Wenhao Li, Cheng Yang, Jiannan Liang, Huimin Chen, Yuhui Zhang, Ruoyu Li__  
*Tsinghua University*  
*ACL'19*

>[Linke of Paper](https://www.aclweb.org/anthology/papers/P/P19/P19-3005/)

# Motivations
- Most existing systems are merely **model-oriented**, which input some user-specified keywords and directly complete the generation proess in one pass, with little user participation.
- Most existing systems generate poetry in fewer styles and genres, and proviede limited options for users.

# Contributions
- **Multi-modal input**, such as keywords, plain text and images.
- **Various styles and genres**.
- **Human-machine collaboration**.

# Architecture
## Overview

![img](/assets/images/post/2019-08-12/002.png)  

**Four modules:**
- input preprocessing
- generation
- postprocessing
- collaborative revision

**Processing:**
Given the user-specified genre, style and inputs, the preprocessing module extractes several keywords from the inputs and then conducts keyword expansion to introduce richer information. With these preprocessed keywords, the generation module generates a poem draft. The postprocessing module re-ranks the candidates of each line and removes the ones that do not conform to structural and phonological requirements. At last, the collaborative revision module interacts with the user and dynamically updates the draft for several times according to the user's revision, to collaboratively create a satisfying poem.

## Input Preprocessing Module
### Keyword Extraction
- For plain text

    1.Use [THULAC](http://thulac.thunlp.org/) to conduct Chinese word segmentation; 

    2.compute the importance $r(w)$ of each word $w$:  

    $$
    \begin{equation}
    r(w)=[\alpha * ti(w)+(1-\alpha) * tr(w)]
    \end{equation}
    $$

    where $ti(w)$ and $tr(w)$ are the TF-IDF value and TextRank score,$\alpha$ is a hyper-parameter to balance the weights of $ti(w)$ and $tr(w)$.

    3.Select top K words with the highest scores.

- For each image

    1.Use [Aliyun image recognition tool](https://ai.aliyun.com/image).

    2.Gives the names of five recognized objects with corresponding probability $s(w)$.

    3.Select top K words with the highest $s(w) \cdot r(w)$.

### Keyword Mapping

- **Problem:**The generation module will take some modern concepts(which never occur in the classical poetry corpus, such as airplane and refrigerator) as a UNK symbol and generate totally irrelevant poems.

- **Solution:**

    1.Build a Poetry Knowledge Graph (PKG) from Wikipedia data, which contains 616,360 entities and 5,102,192 relations.

    2.Use PKG to map the modern concepts to its most relevant entities in poetry, to guarantee both quality and relevance of generated poems.

    3.For a modern concept word $w_i$, score its each neighbor word $w_j$ by:

    $$
    \begin{equation}
    g\left(w_{j}\right)=t f_{w i k i}\left(w_{j} | w_{i}\right) \cdot \log \left(\frac{N}{1+d f\left(w_{j}\right)}\right) \cdot \arctan \left(\frac{p\left(w_{j}\right)}{\tau}\right)
    \end{equation}
    $$
    
    where $tf_{wiki}(w_j \| w_i)$ is the term frequency of $w_j$ in the Wikipedia article of $w_i$, $df(w_j)$ is the number of Wikipedia articles containing $w_j$, $N$ is the number of Wikipedia articles, and $p(w_j)$ is the word frequency counted in all articles.

- **Example:**

    ![img](/assets/images/post/2019-08-12/003.png)  

### Keyword Extension

- **Problem:**More keywords could lead to richer contents and emotions in generated poems. If the number of extracted keywords is less than $K$, further conduct keywords extension.

- **Solution:**

    1.Build a Poetry Word Co-occurrence Graph (PWCG), which indicates the co-occurrence of two words in the same poem.

    2.The weight of the edge between two words is calculated according to the Pointwise Mutual Information (PMI).

    $$
    \begin{equation}
    P M I\left(w_{i}, w_{j}\right)=\log \frac{p\left(w_{i}, w_{j}\right)}{p\left(w_{i}\right) * p\left(w_{j}\right)}
    \end{equation}
    $$

    where $p(w_i)$ and $p(w_i, w_j)$ are the word frequency and co-occurrence frequency in poetry corpus.

    3.For a given word $w$, get all its adjacent words $w_k$ in PWCG and select those with higher values of $\log p\left(w_{k}\right) * P M I\left(w, w_{k}\right)+\beta * r\left(w_{k}\right)$ where $\beta$ is a hyperparameter.

- **Example:**

    ![img](/assets/images/post/2019-08-12/004.png) 

## Generation Module


>Reference:  
>[Chinese Poetry Generation with a Working Memory Model](https://arxiv.org/abs/1809.04306)  
>__Xiaoyuan Yi, Maosong Sun, Ruoyu Li, Zonghan Yang__   
>*Tsinghua University*  
>*IJCAI'18*  
>![img](/assets/images/post/2019-08-12/006.png) 

### Overview

![img](/assets/images/post/2019-08-12/005.png) 

**Three modules:**
- topic memory $M_{1} \in \mathbb{R}^{K_{1} * d_{h}}$
    + Each topic word $w_k$ is written into the topic memory in advance, which acts as the 'major message' and remains unchanged during the generation process of a poem.
- history memory $M_{2} \in \mathbb{R}^{K_{2} * d_{h}}$
    + Each character of $L_{i-1}$ is written into the local memory to provide full short-distance history.
- local memory $M_{3} \in \mathbb{R}^{K_{3} * d_{h}}$
    + Select some salient characters of $L_{i-2}$ to write into the history memory which maintains informative partial long-distance history.

where $K_1$, $K_2$ and $K_3$ are the numbers of slots and $d_h$ is slot size.

### Memory Writing

Use GRU for decoder and bidirectional encoder. Before the generation, all memory slots are initialized with 0.

- For topic memory, feed characters of each topic word $w_k$ into the encoder, then fill each topic vector into a slot.

    ![img](/assets/images/post/2019-08-12/007.png) 

- For local memory, fill the encoder hidden states of characters in $L_{i-1}$ in.

    ![img](/assets/images/post/2019-08-12/008.png)

- For history memory, select a slot by writing addressing function and fill encoder state $h_t$ of $L_{i-2}$ into it.

    ![img](/assets/images/post/2019-08-12/009.png)

    $$
    \begin{equation}
    \alpha_{w}=A_{w}\left(\tilde{M}_{2},\left[h_{t} ; v_{i-1}\right]\right)
    \end{equation}
    $$

    where $\alpha_w$ is the writing probabilities vector, $\tilde{M}_{2}$ is the concatenation of history memory $M_2$ and a null slot, $v_{i-1}$ is a global trace vector, which records what has been generated so far and provides implicit global information for the model. Once $L_i$ is generated, it is updated by a simple vanilla RNN.

    $$
    \begin{equation}
    v_{i}=\sigma\left(v_{i-1}, \frac{1}{T_{e n c}} \sum_{t=1}^{T_{e n c}} h_{t}\right), v_{0}=\mathbf{0}
    \end{equation}
    $$
    For testing (non-differentiable),

    $$
    \begin{equation}
    \beta[k]=I\left(k=\arg \max _{j} \alpha_{w}[j]\right)
    \end{equation}
    $$

    where $I$ is an indicator function.

    For training, simply approximate $\beta$ as,

    $$
    \begin{equation}
    \beta=\tanh \left(\gamma *\left(\alpha_{w}-\mathbf{1} * \max \left(\alpha_{w}\right)\right)\right)+\mathbf{1}
    \end{equation}
    $$

    where $\mathbf{1}$ is a vector with all 1-s and $\gamma$ is a large positive number.

    $$
    \begin{equation}
    \tilde{M}_{2}[k] \leftarrow(1-\beta[k]) * \tilde{M}_{2}[k]+\beta[k] * h_{t}
    \end{equation}
    $$

    If there is no need to write $h_t$ into history memory, model learns to write it into the null slot, which wil be ignored.