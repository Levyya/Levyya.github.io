---
title: wordnet
date: 2022-04-12 15:09:53
tags: NLP
---

## wordnet介绍

[wordnet](https://wordnet.princeton.edu/)是大规模的英文词汇数据库，按**同义词集**将**名词、动词、形容词、副词**分成不同组。

[senseidx](https://wordnet.princeton.edu/documentation/senseidx5wn)提供了访问WordNet数据库中的同义词集和词义的另一种方法:

sense key

> *lemma* **%** *lex_sense*
>
> *lex_sense*: ss_type:lex_filenum:lex_id:head_word:head_id

## 使用NLTK库中的wordnet

关于wordnet的下载，可能会遇到一些麻烦（外网的缘故），可能需要自动手动去官网下载。

```python
from nltk.corpus import wordnet as wn
```

常用：

```python
# 获取一个单词的同义词集
wn.synsets('dog')  # -> List[Synset]
"""
[Synset('dog.n.01'),
 Synset('frump.n.01'),
 Synset('dog.n.03'),
 Synset('cad.n.01'),
 Synset('frank.n.02'),
 Synset('pawl.n.01'),
 Synset('andiron.n.01'),
 Synset('chase.v.01')]
"""
# 获取一个单词的同义词集对应的词根
wn.lemmas('seek')  # -> List[Lemma]
"""
[Lemma('seek.n.01.seek'),
 Lemma('seek.v.01.seek'),
 Lemma('search.v.01.seek'),
 Lemma('try.v.01.seek'),
 Lemma('seek.v.04.seek'),
 Lemma('seek.v.05.seek')]
"""
# 获取词根对应的senseidx
wn.lemma('seek.v.01.seek').key()  # -> sense_key: ss_type:lex_filenum:lex_id:head_word:head_id
"""
'seek%2:40:00::
"""
# 获取对应的释义
wn.lemma('seek.v.01.seek').synset().definition()  # -> str
wn.synset('seek.v.01').definition()  # -> str
"""
'try to get or reach'
"""

```

## 几个重要的网站

[nltk官网](https://www.nltk.org/api/nltk.corpus.reader.wordnet.html?highlight=wordnet#module-nltk.corpus.reader.wordnet)

[wordnet官网](https://wordnet.princeton.edu/)

