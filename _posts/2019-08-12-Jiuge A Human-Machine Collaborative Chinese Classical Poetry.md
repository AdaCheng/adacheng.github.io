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

    1.Use (THULAC)[http://thulac.thunlp.org/] to conduct Chinese word segmentation; 

    2.compute the importance $r(w)$ of each word $w$:  

    $$
    \begin{equation}
    r(w)=[\alpha * ti(w)+(1-\alpha) * tr(w)]
    \end{equation}
    $$

    where $ti(w)$ and $tr(w)$ are the TF-IDF value and TextRank score,$\alpha$ is a hyper-parameter to balance the weights of $ti(w)$ and $tr(w)$.

    3.Select top K words with the highest scores.

- For each image

    1.Use (Aliyun image recognition tool)[https://ai.aliyun.com/image].

    2.Gives the names of five recognized objects with corresponding probability $s(w)$.
    
    3.Select top K words with the highest $s(w) \cdot r(w)$.

### Keyword Mapping
