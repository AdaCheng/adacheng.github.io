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

