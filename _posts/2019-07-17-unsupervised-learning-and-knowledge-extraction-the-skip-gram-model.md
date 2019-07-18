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

> knowledge [noun] : understanding of or information about a subject that you get by experience or study, either known by one person or by people generally

Clearly, just producing a bunch of graphs isn't extracting knowledge as there's no concept of "understanding". It might help people gain knowledge about a particular set of data, but I would argue that we aren't extracting knowledge directly. Something like *process mining* for example is *knowledge extraction* as it will get an understanding of the underlying process(es) that came to generate the data.

In this article I will focus on only one very specific *knowledge extraction* application : drawing correspondances and linking different concepts together by using the skip-gram model.

### A brief foreword about the skip-gram model
I've been inspired to write this article after having read [this article](https://www.nature.com/articles/s41586-019-1335-8) published in nature only a few weeks back. Needless to say, the efficiency of the methods and their findings impressed me greatly. So I chose to dig a little bit deeper and realized that they only used... the skip-gram model. If you're in any way familiar with NLP techniques, you probably have heard of word embeddings. Well, the skip-gram algorithm is the first building block for creating word embeddings and is used in many models such as word2vec. I won't spend too much time explaining the ins and outs of the technique as articles such as [this one](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/) already do a fantastic job, but essentially, we want to train a shallow network to recognize for any given word, the probability of other words surrounding it. The trained network is a shallow 2-layer network with one-hot encoded inputs, the input layer will, as such, contain as many neurons as there is words in our vocabulary, a bottleneck layer that has less neurons than the size of our vocabulary and an output layer with as many neurons as the input layer. Our network will, as such, resemble an autoencoder network :

![Image of an autoencoder architecture](https://www.jeremyjordan.me/content/images/2018/03/Screen-Shot-2018-03-06-at-3.17.13-PM.png)
*credits : Jeremy Jordan*

Once the network is trained to recognize neighbouring words, we keep the weights of the bottleneck layer as vectors to represent our words. These so-called word embeddings will have many interesting properties, namely interesting relationships between words "of the same kind", allowing us to draw analogies from the learned representation. As the words are embedded in a vector space, we can use very basic mathematical operations to retrieve simple information, for example $$king - man = queen$$ or measuring the similarity of words, using the cosine similarity for example.

![Typical example for word2vec word embedding](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQE-xyMuWa1z7JEMsxaxJXPYq7T4Tfz7M922AsaWAa3P92KOPGgQQ)

Though there's of course many technical difficulties associated with the training of such a model (hint : count the number of parameters when the vocabulary is large), the fact still remains that it's deceptively simple and... unsupervised! You don't need human input for the machine to learn a representation. Well, that's not exactly true as a lot of data cleaning will have to be performed before any training can start and some manual tweaking might be required eventually, but you don't need to go through the harduous process of labelling examples, building ontologies, relationship graphs, etc...

Now, the idea of using embeddings to draw analogies is nothing new and though I'm glad the aforementioned article brought it to my attention, I can't reasonably go on without talking about a few other (arguably more fleshed out) pieces of work that the authors left out :
* [Automated Cognome Construction and Semi-automated Hypothesis Generation](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3376233/) : In this paper, the authors build a semi-automated hypothesis generation framework mixing both clustering methods and logical inference tools to draw
* [Law2Vec](https://archive.org/details/Law2Vec) : I can't find the original paper, but it's cited in many cases as being the first to apply the skip-gram model to unstructured documents (in this case, korean legal documents). I'd be extremely interested to know more about it, if anybody has more information don't hesitate to e-mail me!

Many people will argue that the authors' work isn't anything new, which is true, but I will say that they went up and above to provide us with the original data as well as [the code](https://github.com/materialsintelligence/mat2vec) for their model, which allows us to replicate their findings.

### Summary of the paper
*Disclaimer : though the paper can be accessed through [SharedIt](https://www.springernature.com/gp/researchers/sharedit), it is not technically "open", so I will refrain from including too many tables/graphs from it.*

The authors used the skipgram variant of the word2vec model with a 200 dimensions embedding and trained it on 3.3 million scientific abstracts, providing them with a vocabulary of about 500,000 words.

To measure the performances of their model, the team tested it on multiple diverse tasks
* Material science analogies (ex : pressure - Pa + Hz = frequency)
* Prediction of a material group
* Prediction of which material was going to be studied as a thermoelectric in the future using "historical data" (that is to say, only using data prior to the material being discovered as a thermoelectric)

The first task can be achieved in a fairly straightforward way using a test database of pairs, the first element being the formula to test and the second element being the expected response. Below are all the categories of relationship tested :
* Chemical element names
* Crystal symmetries
* Crystal structure names
* Elemental crystal structures
* Principal oxides
* Units
* Mangetic properties
* Applications
* Grammar

The answer is considered correct if and only if it corresponds exactly to the expected response, homonyms are, as such, not considered valid. The model obtains an average accuracy of 60.1%, though with a rather wide standard deviation, performing the worst on *Crystal structure names* (18.7%) and the best on *Chemical element names* (71.4%). The types of relationships on which the model perform the best (*Chemical element names* and *Grammar*) are disproportionally represented in the validation database, explaining the rather high average score.

It's unclear to me as to how the authors predicted the material classes however, but my simple guess would be that they used a simple network model using their embedding as inputs. A neat finding is that materials from the same group (metalloid, noble gas, etc...) tend to cluster together.

All that being said, the main focus of this paper (and the achievement that made the headlines) was the last task : to predict which material was gonna be studied as being a thermoelectric material only based on prior  historical knowledge. For instance, they trained the model only using scientific abstract from 1922 to 2001 and then ranks the 50 best candidates for thermoelectric materials. They found that these materials were 8 times more likely to be studied as thermoelectrics in the 5 years after the time window the model was trained on than any randomly chosen unstudied material from the time and 3 times more than with a non null [density functional theory](https://en.wikipedia.org/wiki/Density_functional_theory) value. These predicted words never appeared side by side with a term explicitly linked to thermoelectric materials ('ZT', 'zT', 'seebeck', 'thermoelectric', 'thermoelectrics', 'thermoelectrical', ...)! Why these materials were still detected as being related to thermoelectrics can be understood by studying some of their connections to the actual term, some of which are laid out in the diagram below :

![relationship diagram for predicted thermoelectric materials](https://redchards.github.io/img/skip_gram_nature/relationship_graph.png)

Though some of these relations were well-known even at the time of the historical articles being published, when the elements leading to the conclusions are disseminated across hundreds of articles, humans have a hard time drawing parallels. Moreover, throughout the paper the authors make a point to stress out that the model has **no** field knowledge whatsoever and wasn't enriched with expert information in any way, shape or form. According to the authors :

> This demonstrates that word embeddings go beyond trivial compositional or structural similarity and have the potential to unlock latent knowledge not directly accessible to human scientists.

The validity of this claim has yet to be established through practical applications, but it's definitely an interesting way forward.

If you've read the article, you might've noticed that I completely skipped the comparison of the DFT ranking with the authors' method ranking as well as the technical details of the method. It is indeed on purpose as the former is ancdotal and of lesser interest and the latter is way too unweildy to be included in this summary.

### Conclusion
While it has been commonly known for a long time that word embeddings contained semantic and syntactic information about language, works making use of their wealth of information beyond simple correspondances and analogies have been rather scarce. The author, though understandably optimistic, stay careful and only mentioning usages aimed at assisting scientists in their research, even mentioning that :

> The success of our unsupervised approach can partly be attributed to the choice of the training corpus. The main purpose of abstracts is to communicate information in a concise and straightforward manner, avoiding unnecessary words that may increase noise in embeddings during training.

The information to noise ratio can be pretty low in most common corpus and that's not event counting spelling errors. For example, when I trained a word2vec model on the Amazon review corpus and tried to find the top 10 most common words, I got presented with words such as "phonethis", "phonethe", "phoneit", which are obvious missing spaces cause by a simple typo. The team then concludes with a rather edifying statement :

> Scientific progress relies on the efficient assimilation of existing knowledge in order to choose the most promising way forward and to minimize re-invention. As the amount of scientific literature grows, this is becoming increasingly difficult, if not impossible, for an individual scientist. We hope that this work will pave the way towards making the vast amount of information found in scientific literature accessible to individuals in ways that enable a new paradigm of machine-assisted scientific breakthroughs.

I would even argue that such technique could become a much more prominent tool in the data miner toolbox once it has been refined and fleshed out. Something that is certain is that the use-cases for this type of knowledge extraction are plenty.