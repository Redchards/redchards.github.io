---
layout: post
published: false
title: 'Unsupervised learning and knowledge extraction : the skip-gram model'
---
## Unsupervised learning and knowledge extraction : the skip-gram model

### Introduction
With the growing amount of data generated throughout the world every single day, both public and private, whether the information leaves the wall of organizations, crosses borders or is kept in secrecy, data mining is becoming ever so more relevant. Though this is far from being a recent phenomenon, dating back to ancient Egypt, the sheer quantity of data generated and the computational power at our disposal allows us to leverage novel techniques and extract knowledge from the mainly unstructured format of our data. 

### "Knowledge Extraction" vs "Data Mining"
In this article, I will use the term *knowledge extraction* instead of *data mining* as, though there are many instances in which these two terms will be considered homonymous, *data mining* is much more all-encompassing than *knowledge extraction*. Indeed, take for example an evaluation backed by a data analysis of a service's users with the goal to devise an efficient marketing campaign : in this case, we will indeed extract some form of knowledge, but it'll be indirect and will require an expert to have a look at the analysis and decide what the best course of action would be. Let's take a look at Cambridge Dictionary's definition of knowledge :
`
knowledge [noun] : understanding of or information about a subject that you get by experience or study, either known by one person or by people generally
`
Clearly, just producing a bunch of graphs isn't extracting knowledge as there's no concept of "understanding". It might help people gain knowledge about a particular set of data, but I would argue that we aren't extracting knowledge directly. Something like *process mining* for example is *knowledge extraction* as it will get an understanding of the underlying process(es) that came to generate the data.

In this article I will focus on only one very specific *knowledge extraction* application : drawing correspondances and linking different concepts together by using the skip-gram model.

### A brief foreword about the skip-gram model
I've been inspired to write this article after having read [this article](https://www.nature.com/articles/s41586-019-1335-8) published in nature only a few weeks back. Needless to say, the efficiency of the methods and their findings impressed me greatly. So I chose to dig a little bit deeper and realized that they only used... the skip-gram model. If you're in any way familiar with NLP techniques, the skip-gram algorithm is the first building block for word embeddings such as word2vec. I won't spend too much time explaining the ins and outs of the technique as articles such as [this one](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/) already do a fantastic job, but essentially, we want to train a shallow network to recognize for any given word, the probability of other words surrounding it. The trained network is a shallow 2-layer network with one-hot encoded inputs, the input layer will, as such, contain as many neurons as there is words in our vocabulary, a bottleneck layer that has less neurons than the size of our vocabulary and an output layer with as many neurons as the input layer. Our network will, as such, resemble an autoencoder network :
![Image of an autoencoder architecture](http://mccormickml.com/assets/word2vec/output_weights_function.png)
*credits : Jeremy Jordan

Once the network is trained to recognize neighbouring words, we keep the weights of the bottleneck layer as vectors to represent our words. These so-called word embeddings will have many interesting properties, namely interesting relationships between words "of the same kind", allowing us to draw analogies from the learned representation.
![Typical example for word2vec word embedding](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQE-xyMuWa1z7JEMsxaxJXPYq7T4Tfz7M922AsaWAa3P92KOPGgQQ)

Though there's of course many technical difficulties associated with the training of such a model (hint : count the number of parameters when the vocabulary is large), the fact still remains that it's deceptively simple and... unsupervised! You don't need human input for the machine to learn a representation. Well, that's not exactly true as a lot of data cleaning will have to be performed before any training can start and some manual tweaking might be required eventually, but you don't need to go through the harduous process of labelling examples, building ontologies, relationship graphs, etc...

Now, the idea of using embeddings to draw analogies is nothing new and though I'm glad the aforementioned article brought it to my attention, I can't reasonably go on without talking about a few other (arguably more fleshed out) pieces of work that the authors left out :
* [Automated Cognome Construction and Semi-automated Hypothesis Generation](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3376233/) : In this paper, the authors build a semi-automated hypothesis generation framework mixing both clustering methods and logical inference tools to draw
* [Law2Vec](https://archive.org/details/Law2Vec) : I can't find the original paper, but it's cited in many cases as being the first to apply the skip-gram model to unstructured documents (in this case, korean legal documents). I'd be extremely interested to know more about it, if anybody has more information don't hesitate to e-mail me!

Many people will argue that the authors' work isn't anything new, which is true, but I will say that they went up and above to provide us with the original data as well as [the code](https://github.com/materialsintelligence/mat2vec) for their model, which allows us to replicate their findings