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

Fuzzy matching -> suggestion about the user is searching
https://www.elastic.co/guide/en/elasticsearch/guide/current/fuzzy-matching.html

## Adding synonyms to index

The word `esporte` can be handled different than `esportes`. We can repeat the same rule for both words. But when we do this, we inflate the index, like a ballon. Making the search inefficient.

We can improve it using the Portuguese analyzer. 

```
GET /indice_com_sinonimo_2/_analyze
{
  "analyzer": "portuguese",
  "text": "esporte"
}
```

and the result is gonna be:
```
{
  "tokens" : [
    {
      "token" : "esport",
      "start_offset" : 0,
      "end_offset" : 7,
      "type" : "<ALPHANUM>",
      "position" : 0
    }
  ]
}
```

So, our synonyms' list gonna have `esport`

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
    "esport => esport,futebol,society,volei,basquet",
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
The `analyzer` field is used to index the tokens of the document used every time you to add or to update a document

The `search_analyzer` is used only on the search

## Loading data

### String types
When we store a new document on the index, we have to preserve the original data. When data is stored in an inverted index, it will first go through parsing, to not have a issue with the data in the inverted index we can use a new property called `fields`

```
...
"nome": {
  "type": "text",
  "fields": {"original": {"type": "text", "index":true}},
  "index": true,
  "analyzer:"portuguese"
}
...
```

Every time we have a `"type": "text"` field we have to remember that this attribute can be parsed before being added to the inverted index. So to fix it and preserve the field, we have to use `"type":"keyword"` instead.

To summarize, we have two types of strings:
- text: this type will always be indexed and analized, that is, it is modified before being added to the inverted index
- keyword: we ensure that it won't be analized


### Add data in bulk
We have to use the `{"index": {}}` plus the document in json, 

```
POST /pessoas/_bulk

{"index": {}}
{"nome": "Abdalla Yussef Tauil Neto", "cidade": "São José do Rio Claro", "formação": "Letras", "estado": "MT", "país": "Brasil", "interesses": ["futebol","society","volei"] }
{"index": {}}
{"nome": "Barbara de Mariz Silva", "cidade": "Beruri", "formação": "Música", "estado": "AM", "país": "Brasil", "interesses": ["computação","artes"] }
...
```

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
