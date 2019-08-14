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

## Generation Module - Core Component

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

### Encoder

Use **GRU** for bidirectional encoder.

Denote $X$ a line in encoder ($L_{i-1}$), $X=\left(x_{1} x_{2} \ldots x_{T_{e n c}}\right)$, and $h_t$ represent the encoder hidden states.

### <span id='GTV'>Global Trace Vector</span>

$v_{i}$ is a global trace vector, which records what has been generated so far and provides implicit global information for the model. Once $L_i$ is generated, it is updated by a simple vanilla RNN.

![img](/assets/images/post/2019-08-12/010.png) 

$$
\begin{equation}
v_{i}=\sigma\left(v_{i-1}, \frac{1}{T_{e n c}} \sum_{t=1}^{T_{e n c}} h_{t}\right), v_{0}=\mathbf{0}
\end{equation}
$$

where $\sigma$ defines a non-linear layer and $\mathbf{0}$ is a vector with all 0-s.

### <span id='AF'>Addressing Function</span>

Defining an Addressing Function, $\alpha=A(\tilde{M}, q)$, which calculates the probabilities that each slot of the memory is to be selected and operated.

$$
\begin{equation}
z_{k}=b^{T} \sigma(\tilde{M}[k], q)
\end{equation}
$$

$$
\begin{equation}
\alpha[k]=\operatorname{softmax}\left(z_{k}\right)
\end{equation}
$$

where $q$ is the query vector, $b$ is the parameter, $\tilde{M}$ is the memory to be addressed, $\tilde{M}[k]$ is the k-th slot of $\tilde{M}$ and $\alpha[k]$ is the k-th element in vector $\alpha$.

### Memory Writing

Before the generation, all memory slots are initialized with 0.

- For topic memory, feed characters of each topic word $w_k$ into the encoder, then fill each topic vector into a slot.

    ![img](/assets/images/post/2019-08-12/007.png) 

- For local memory, fill the encoder hidden states of characters in $L_{i-1}$ in.

    ![img](/assets/images/post/2019-08-12/008.png)

- For history memory, select a slot by writing [addressing function](#AF) and fill encoder state $h_t$ of $L_{i-2}$ into it.

    ![img](/assets/images/post/2019-08-12/009.png)

    $$
    \begin{equation}
    \alpha_{w}=A_{w}\left(\tilde{M}_{2},\left[h_{t} ; v_{i-1}\right]\right)
    \end{equation}
    $$

    where $\alpha_w$ is the writing probabilities vector, $$\tilde{M}_{2}$$ is the concatenation of history memory $M_2$ and a null slot, $v_{i-1}$ is [global trace vector](#GTV).

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

### <span id='MR'>Memory Reading</span>

![img](/assets/images/post/2019-08-12/011.png)

$$
\begin{equation}
\alpha_{r}=A_{r}\left(M,\left[s_{t-1} ; v_{i-1} ; a_{i-1}\right]\right)
\end{equation}
$$

$$
\begin{equation}
o_{t}=\sum_{k=1}^{K} \alpha_{r}[k] * M[k]
\end{equation}
$$

where $\alpha_r$ is the reading probability vector, $s_{t-1}$ is the [decoder hidden states](#Decoder) and the [trace vector](#GTV) $v_{i-1}$ is used to help the [Addressing Function](#AF) avoid reading redundant content, $a_{i-1}$ is the [topic trace vector](#TT). Joint reading from the three memory modules enables the model to flexibly decide to express a topic or to continue the history content.

### <span id='TT'>Topic Trace Mechanism</span>

**Problem:** A global trace vector $v_i$ is not enough to help the model remember whether each topic has been used or not.

**Solutions:** Design a Topic Trace (TT) mechanism to record the usage of topics in a more explicit way.

![img](/assets/images/post/2019-08-12/013.png)

$$
\begin{equation}
c_{i}=\sigma\left(c_{i-1}, \frac{1}{K_{1}} \sum_{k=1}^{K_{1}} M[k] * \alpha_{r}[k]\right), c_{0}=\mathbf{0}
\end{equation}
$$

$$
\begin{equation}
u_{i}=u_{i-1}+\alpha_{r}\left[1 : K_{1}\right], u_{i} \in \mathbb{R}^{K_{1} * 1}, u_{0}=\mathbf{0}
\end{equation}
$$

$$
\begin{equation}
a_{i}=\left[c_{i} ; u_{i}\right]
\end{equation}
$$

$c_i$ maintains the content of used topics and $u_i$ explicitly records the times of reading each topic. $a_i$ is the topic trace vector.

### <span id='GE'>Genre embedding</span>

$g_t$ is the concatenation of a phonology embedding and a length embedding, which are learned during training.

- The phonology embedding indicates the required category of $y_t$.
- The length embedding indicates the number of characters to be generated after $y_t$ in $L_i$ and hence controls the length of $L_i$.

### <span id='Decoder'>Decoder</span>

Denote $Y$ a generated line in decoder ($L_i$), $Y=\left(y_{1} y_{2} \ldots y_{T_{d e c}}\right)$, and $s_t$ represent the decoder hidden states. $e(y_t)$ is the word embedding of $y_t$.

![img](/assets/images/post/2019-08-12/012.png)

$$
\begin{equation}
s_{t}=G R U\left(s_{t-1},\left[e\left(y_{t-1}\right) ; o_{t} ; g_{t} ; v_{i-1}\right]\right)
\end{equation}
$$

$$
\begin{equation}
p\left(y_{t} | y_{1 : t-1}, L_{1 : i-1}, w_{1 : K_{1}}\right)=\operatorname{softmax}\left(W s_{t}\right)
\end{equation}
$$

where $g_t$ is a special [genre embedding](#GE), $o_t$ is the [memory output](#MR) and $W$ is the projection parameter. $v_{i-1}$ is a [global trace vector](#GTV).

## Generation Module - Unsupervised Style Control

>Reference:  
>[Stylistic Chinese Poetry Generation via Unsupervised Style Disentanglement](https://www.aclweb.org/anthology/papers/D/D18/D18-1430/)  
>__Cheng Yang, Maosong Sun, Xiaoyuan Yi, Wenhao Li__   
>*Tsinghua University*  
>*EMNLP'18*  
>![img](/assets/images/post/2019-08-12/014.png) 

### Overview

Take two arguments as input: input sentence $s_{input}$ and style id $k \in 1,2 \ldots K$ where $K$ is the total number of different styles. Then enumerate each style id $k$ and generate style-specific output sentence $s^{k}_{output}$ respecitively.

![img](/assets/images/post/2019-08-12/015.png)

### Mutual Information

Given two random variables $X$ and $Y$, the mutual information $I(X, Y)$ measures "the amount of information" obtained about one random variable given another one. Mutual information can also be interpreted as a measurement about how similar the joint probability distribution $p(X, Y)$ is to the product of marginal distributions $p(X)p(Y)$.

$$
\begin{equation}
I(X, Y)=\int_{Y} \int_{X} p(X, Y) \log \frac{p(X, Y)}{p(X) p(Y)} d X d Y
\end{equation}
$$

### Attention Mechanism

>Reference:  
>[Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/abs/1409.0473v2)  
>__Dzmitry Bahdanau, Kyunghyun Cho, Yoshua Bengio__   
>*Jacobs University, Universite de Montr´eal*  
>*ICLR'15*  
>![img](/assets/images/post/2019-08-12/017.png) 

![img](/assets/images/post/2019-08-12/016.png)

- The encoder uses bidirectional LSTM model to project the input sentence $X$ into the vector space.
- The hidden state of LSTM are computed by

    $$
    \begin{equation}
    \overrightarrow{h_{i}}=L S T M_{\text {forward}}\left(\overrightarrow{h_{i-1}}, e\left(x_{i}\right)\right)
    \end{equation}
    $$

    $$
    \begin{equation}
    \overleftarrow h_{i}=L S T M_{b a c k w a r d}\left(\overleftarrow h_{i+1}, e\left(x_{i}\right)\right)
    \end{equation}   
    $$

    for $i=1,2 \ldots T$ where $$\overrightarrow{h_{i}}$$ and $$\overleftarrow h_{i}$$ are the i-th hidden state of forward and backward LSTM respectively, $$e\left(x_{i}\right) \in \mathcal{R}^{d}$$ is the character emebedding of character $x_i$ and $d$ is the dimension of character embeddings.

- Concatenate corresponding hidden states of forward and backward LSTM $$h_{i}=\left[\vec{h}_{i}, \overleftarrow h_{T-i+1}\right]$$ as the i-th hidden state of bi-LSTM.

- The decoder module contains an LSTM decoder with attention mechanism, which computes a context vector as a weighted sum of all encoder hidden states to represent the most relevant information at each stage.

- The i-th hidden state in the decoder LSTM

    $$
    \begin{equation}
    s_{i}=L S T M_{d e c o d e r}\left(s_{i-1},\left[e\left(y_{i-1}\right), a_{i}\right]\right)
    \end{equation}
    $$

    for $i=2, \ldots T$ and $s_{1}=h_{T}$, where $[ ]$ indicates concatenation operation, $e\left(y_{i-1}\right)$ is character embedding of $y_{i-1}$ and $a_i$ is the context vector learned by attention mechanism.

    $$
    \begin{equation}
    a_{i}=\operatorname{attention}\left(s_{i-1}, h_{1 : T}\right)
    \end{equation}
    $$

- The character probability distribution when decoding the i-th character can be expressed as 

    $$
    \begin{equation}
    p\left(y_{i} | y_{1} y_{2} \ldots y_{i-1}, X\right)=g\left(y_{i} | s_{i}\right)
    \end{equation}
    $$
