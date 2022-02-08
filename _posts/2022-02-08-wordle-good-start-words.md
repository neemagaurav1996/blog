---
layout: post
title:  Wordle - What are some good starting words?
date:   2021-06-26 02:00:00 +0530
categories: [Blogging, Article]
permalink: wordle-good-start-words
tags: wordle python python3 nltk
image: images/article-images/wordle-good-start-words.png
image_alt: wordle-good-start-words
comments: true
display_image: images/article-images/wordle-good-start-words-withoutbg.png
description: You might have probably heard of Wordle, which is a word guessing game. If you've played Wordle before, you'll know that the most important part of solving the puzzle is starting with a good word. In this article, we will use word analysis to determine the best first word to use when solving Wordle.
duration: 2 min read
clean_date: February 08, 2022
---

## {{page.title}}
<div class="article-info muted-text">
    <span class="published-on">Published on {{page.clean_date}}</span>
    <span class="duration"><i class="icon-clock"></i> {{page.duration}}</span>
</div>

<figure>
	<img class="article-image" src="{{ page.image }}" alt="{{ page.image_alt }}" width="200">
</figure>


You might have probably heard of Wordle, which is a word guessing game. If you've played Wordle before, you'll know that the most important part of solving the puzzle is starting with a good word. In this article, we will use word analysis to determine the best first word to use when solving Wordle.

<!--more-->

### Assumptions...

First of all, we won't be looking into the source code of the Wordle. Thus we assume that we don't know vocabulary of Wordle.

### Requirements

- Python3
- pip install nltk
- pip install english-words

### Let's do it!

**1) Get all common 5-letter words**

```python
from english_words import english_words_set
from nltk import ngrams
from itertools import islice

_5_letter_words = set(w for w in english_words_set if len(w) == 5)
print("No. of 5-letter words: ",len(_5_letter_words))

```

> No. of 5-letter words:  3210

**2) Create frequency distribution of 2-grams of 3210 5-letter words**

```python
freq = {}
for word in _5_letter_words:
    _2_grams_word = ngrams(list(word), 2)
    for x in _2_grams_word:
        _2_gram = ''.join(x).lower()
        if _2_gram in freq:
            freq[_2_gram] += 1
        else:
            freq[_2_gram] = 1
```

**3) Sort frequency distribution and get top 12 most occuring 2-grams**

```python
freq_sorted = {k: v for k, v in sorted(freq.items(), key=lambda item: item[1], reverse=True)}
freq_sorted_top_12 = {k: freq_sorted[k] for k in list(freq_sorted)[:12]}
print(freq_sorted_top_12)
```
> {'er': 195, 'an': 177, 'in': 170, 'ar': 169, 'al': 168, 'ra': 157, 'le': 149, 'st': 149, 're': 146, 'ch': 140, 'la': 136, 'on': 134}


**4) Find some good words. The words that contain more than 2 top 12 2-grams.**

```python
good_words = []

for word in _5_letter_words:
    word = word.lower()
    matching_grams = 0
    _2_grams_word = ngrams(list(word), 2)
    for x in _2_grams_word:
        if("".join(x) in freq_sorted_top_12):
            matching_grams+=1
    if(matching_grams > 2):
        good_words.append(word)

print(good_words)

```

> ['lares', 'allan', 'glare', 'stare', 'blare', 'alarm', 'ranch', 'alert', 'saran', 'stale', 'larch', 'clare', 'flare', 'clara']

The above list contain some good words to start with.

Link to [Google Collab Notebook](https://colab.research.google.com/drive/1zXLxQ3RTvBM_2_ryJMno-rjL86toOzhu?usp=sharing).