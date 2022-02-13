---
title: "Elasticsearch notes - part 2"
layout: post
date: 2022-02-13
headerImage: false
tag:
- elasticsearch
published: true
category: blog
author: beatrizuezu
description: Elasticseach notes - Part 2
---

Some Elasticsearch studies notes - Part 2 🎉

---


# ElasticSearch

## Analyzer with Synonyms
Synonyms allow us to associate words that are related in real world

### Inverted indexing
Let's recap, what is Inverted indexing? 
It's an association between tokens (values) and the source documents

Exemple:
We have those documents:
```
1: "Sei que sou"
2: "Sou o que sei"
3: "Sou uma banana"
```
inverted index
```
"sei": {1,2}
"que": {1,2}
"sou": {1,2,3}
"o": {2}
"uma": {3}
"banana": {3}
```

### Analyzers
#### Most Commun analyzer
- whitespace: each space in the text is used to split the words
- standard: uses whitespace, ignores commas, periods, convertes all words to lowercase
- simple: similar to standar, but it ignores numbers
- language: similar to simple, and it removes accents, recognizes plurals and convert it to singular (it's possible to seach a word in singular and ES retrieves documents with plural), synonym

We can use some filters to refine the rules' tokenization
```
"analyzer": {
  "portuguese": {
    "tokenizer": "standard",
    "filter": [
      "lowcase",
      "portuguese_stop",
      "portuguese_keywords",
      "portuguese_stemmer"
    ]
  }
}
```

"O RATO roeu a ROUPA do rei de Roma"

token filter: lowercase => ["o", "rato", "roeu", "a", "roupa", "do", "rei", "de", "roma"]
token filter: portuguese_stop => (remove preposition and article)=> ["rato", "roeu", "roupa", "rei","roma"] 

"Quem com ferro fere, com ferro será ferido"
token filter: portuguese_stemmer (extract the root from each word)=> ["ferro", "fere", "ferr", "ferid"]

[term to be indexed] => [list of tokens generated in the index]
[more generic term] => [more specific term]
[colection] => [list of elements]

```
PUT /indice_com_sinonimo_2
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 0
    },
    "analysis": {
      "filter": {
        "filtro_de_sinonimos": {
            "type": "synonym",
            "synonyms": [
    "futebol => futebol,society",
    "society => society,futebol",
    "esporte => esporte,futebol,society,volei,basquete",
            ]
        }
      },
      "analyzer": {
        "sinonimos": {
          "tokenizer":  "standard",
          "filter": [
            "lowercase",
            "filtro_de_sinonimos"
          ]
        }
      }
    }
  }
}
```

Fuzzy matching

## Adding synonyms to index
## Loading data

## Query language
Elastic Search vs SQL

### Exact correspondence
### Selecting attributes
### Matching criteria
#### Should
#### Must
#### Must not
### Non-exact correspondence
### Sorting result
## Dashboard Kibana
