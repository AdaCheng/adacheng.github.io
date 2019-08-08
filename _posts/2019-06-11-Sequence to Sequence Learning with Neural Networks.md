---
title: Sequence to Sequence Learning with Neural Networks
description: An introduction for a paper published in NIPS'14.
categories:
 - Paper_Note
tags:
 - Machine Translation
---

![Paper](/assets/images/post/2019-06-11/00.png)  

__Ilya Sutskever, Oriol Vinyals, Quoc V.Le__  
*Google*  
*NIPS'14*

>[Link of Paper](https://arxiv.org/abs/1409.3215)

# Motivation
## Challenge

**Deep Neural Network**  

- Advantages:
    + Can perform arbitrary parallel computation for a modest number of steps.
    + Can be trained with supervised backpropagation whenever the labeled training set has enough information to specify the network's parameters.

- Disadvantages:
    + Cannot be used to map sequences to sequences whose lengths are not known a-priori, because DNNs can only be applied to problems whose inputs and targets can be sensibly encoded with vectors of fixed dimensionality.

## Solution
- Use one LSTM to read the input sequence to obtain large fixed-dimensional vector representation, and then use another LSTM to extract the output sequence from the vector.

![Pic](/assets/images/post/2019-06-11/01.png)  

# Model
## Function
Estimate the conditional probability $p(y_1, ..., y_T'|x_1, ..., x_T)$ where $(x_1, ..., x_T)$ is an input sequence and $(y_1, ..., y_T')$ is its corresponding output sequence whose length $T'$ may differ from $T$.

![Pic](/assets/images/post/2019-06-11/02.png) 

Each $p(y_t|v, y_1, ..., y_{t-1})$ distribution is represented with a softmax over all the words in the vocabulary.

## Training
Maximize the log probability of a correct translation $T$ given the source sentence $S$.

![Pic](/assets/images/post/2019-06-11/03.png)

Where $|S|$ is the size of training set.

## Decoding
Produce translations by finding the most likely translation according to the LSTM.

![Pic](/assets/images/post/2019-06-11/04.png)

Using a simple left-to-right beam search decoder.

# Experiment
## Dataset
- WMT'14 English to French MT task
    + On a subset of 12M sentences consisting of 348M French words and 304M English words.
    + Use 160,000 of the most freqeunt words for the source language and 80,000 of the most frequent words for the target language. Every out-of-vocabulary word was replaced with a special "UNK" token.

## Two ways
- Directly translate the input sentence without using a reference SMT system.
- Use it to rescore the n-best lists of an SMT baseline.

## Trick
> More details can be found in [Paper $3.4](https://arxiv.org/pdf/1409.3215.pdf)

### EOS
- Each sentence ends with a special end-of-sentence symbol "<EOS>", which enables the model to define a distribution over sequences of all possible lengths.

### LSTMs
- Use two different LSTMs: one for the input sequence and another for the output sequence, because doing so increses the number model parameters at negligible computational cost and makes it natural to train the LSTM on multiple language pairs simultaneously.
- Use deep LSTMs with 4 layers, with 1000 cells at each layer.

### Reverse
- Reverse the order of the words of the input sentence.
- Reduce the problem 'minimal time lag', backpropagation has an easier time 'establishing communication' between the source sentence and the target sentence.

## Parallelization
- Use an 8-GPU machine.
    + Each layer of the LSTM was executed on a different GPU and communicated its activations to the next GPU.
    + Another 4 GPUs were used to parallelize the softmax.
- Achieve a speed of 6,300 words per second with a minibatch size of 128.
- Training took about a ten days with this implementation.

## Results
![Pic](/assets/images/post/2019-06-11/05.png)

![Pic](/assets/images/post/2019-06-11/06.png)

# Analysis
- The representations are sensitive to the order of words, while being fairly insensitive to the replacement of an active voice with a passive voice.

![Pic](/assets/images/post/2019-06-11/07.png)

- The LSTM did well on long sentences.

![Pic](/assets/images/post/2019-06-11/08.png)

# Shortages
- The fixed-length vector lost some information during encoding.
- It performs not well when the sentence is too long.

> Solution: Attention.
