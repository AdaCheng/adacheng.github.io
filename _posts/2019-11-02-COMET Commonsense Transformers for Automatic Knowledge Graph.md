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
Contrary to many conventional KBs that store knowledge with canonival templates, commonsense KBs only store **loosely structured open-text descriptions of knowledge**.

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
COMET is given a training knowledge base of natural language tuples in 
${s, r, o}$ format, where $s$ is the phrase subject of the tuple, $r$ is the relation of tuple, and $o$ is the phrase object of the tuple. The task is to generate $o$ given $s$ and $r$ as inputs.

> For example,
> a ConceptNet tuple relating to "taking a nap" would be: (s = "take a nap", r = Causes, o = "have energy").

