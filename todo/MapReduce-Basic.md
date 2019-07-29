---
title: Go Through Map Reduce 
date: 2018-06-21 23:13:12
tags:
  - MapReduce
  - Algorithm
  - Back2Basic
---

## Map Reduce Basic -1

- Sort
- Searching
- Indexing
- Classification: Bayesian Classification/WEKA
- Joining: Join Reducer
- TF-IDF: Term Frequency – Inverse Document Frequency

## Sort: Inputs/Algorithm

Inputs could be :
- A set of files,one value per line
- Mapper key is file name,line number
- Mapper value is the contents of the line

Algorithm:

- Mapper: Identity function for value: ```(k, v)  (v, _)```
- Reducer: Identity function ```(k’, _) -> (k’, “”)```
- (k,v) pairs from mappers are send to particular reducer based on hash(key)
- hash function for data sets, make sure ```k1<k2``` that ```hash(k1)< hash(k2)```
- IO drag race?? 


## Search: Inputs

- A set of files,one value per line
- Mapper key is file name,line number
- Search Pattern sent as special parameter
- Once a file is found to interesting,make it once,Combiner function to 
  fold redundant ```(filename, _)``` into one for sake of reducing network IO


## Indexing: Inverted Index Algorithm

- Mapper: each word in file to (word,file)
- Reduce: reduce to (word,FormattedPageList)

![img](/imgages/bg/index_mr.jpg)

