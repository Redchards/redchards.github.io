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
This article won't