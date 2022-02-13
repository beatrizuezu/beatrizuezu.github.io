---
title: "Elasticsearch notes - part 1"
layout: post
date: 2022-01-29
headerImage: false
tag:
- elasticsearch
published: true
category: blog
author: beatrizuezu
description: Elasticseach notes - Part 1
---

Some Elasticsearch studies notes - Part 1 🎉

---


# ElasticSearch

Relational Database: SQL
ElasticSearch: HTTP REST

HTTP vs SQL
POST - insert
GET - select
PUT - update or insert without duplication
DELETE - delete

ES:
index -> "a ES's database"

to query everything
```
GET _search
{
  "query": {
    "match_all": {}
  }
}
```

- We can use Kibana as a CLI to query the Elasticsearch data

- All fields are indexed, so you don't need to create a index for the fields
- The query is case sensitive and differs between accents (exemple: `musica` and `música`)

==
## POST
### POST [index]/_doc
Let's create a record's of one person in a index, as we call the database in es, called catalog

index (database)-> catalog
table (table) -> person

POST [index]/_doc -> catalog/_doc
{json with the document}

exemple:

```
POST catalogo/_doc/
{
    "nome": "João Silva",
    "interesses": ["futebol", "música", "literatura"],
    "cidade": "São Paulo",
    "formação": "Letras",
    "estado": "SP",
    "país": "Brasil"
}
```

### POST [index]/_doc/{id}
To create a document with a explicit id

POST [index]/_doc/{id} 
{json with document}


exemple:
```
POST catalogo/_doc/1
{
    "nome": "João Silva",
    "interesses": ["futebol", "música", "literatura"],
    "cidade": "São Paulo",
    "formação": "Letras",
    "estado": "SP",
    "país": "Brasil"
}
```

### POST [index]/_update/50
To update documents using POST 

```
POST catalogo/_update/50
{
  "doc": {
    "nome": "Marcelo R. Oliveira"
  }
}

```

## GET
### GET [index]/_count
To count how many documents ("records") there are in my index

exemple:

```
GET catalogo/_count
```

### GET [index]/_doc/{id}
To get a specific document

example
```
GET catalogo/_doc/1
```

Response:
```
{
  "_index" : "catalogo",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1, # shows how many times the doc was modified 
  "_seq_no" : 3,
  "_primary_term" : 1,
  "found" : true,
  "_source" : { # the data
    "nome" : "João Silva",
    "interesses" : [
      "futebol",
      "música",
      "literatura"
    ],
    "cidade" : "São Paulo",
    "formação" : "Letras",
    "estado" : "SP",
    "país" : "Brasil"
  }
}

```


### GET [index]/_search
To seach in all documents

exemple:
```
GET catalogo/_search
```

response
```
{
  "took" : 0, # time in ms
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4, 
      "relation" : "eq"
    },
    "max_score" : 1.0, # it goes 0 to 1, it indicates the relevance degree of the results
    "hits" : [ # the query response, by default it retrieve 10 documents
      {
        "_index" : "catalogo",
        "_type" : "_doc",
        "_id" : "aFTdpn4B20sWFE72MWWr",
        "_score" : 1.0,
        "_source" : {
          "nome" : "João Silva",
          "interesses" : [
            "futebol",
            "música",
            "literatura"
          ],
          "cidade" : "São Paulo",
          "formação" : "Letras",
          "estado" : "SP",
          "país" : "Brasil"
        }
      },
      {
        "_index" : "catalogo",
        "_type" : "_doc",
        "_id" : "aVTgpn4B20sWFE7202W9",
        "_score" : 1.0,
        "_source" : {
          "nome" : "João Silva",
          "interesses" : [
            "futebol",
            "música",
            "literatura"
          ],
          "cidade" : "São Paulo",
          "formação" : "Letras",
          "estado" : "SP",
          "país" : "Brasil"
        }
      },
      {
        "_index" : "catalogo",
        "_type" : "_doc",
        "_id" : "alTgpn4B20sWFE721GVZ",
        "_score" : 1.0,
        "_source" : {
          "nome" : "João Silva",
          "interesses" : [
            "futebol",
            "música",
            "literatura"
          ],
          "cidade" : "São Paulo",
          "formação" : "Letras",
          "estado" : "SP",
          "país" : "Brasil"
        }
      },
      {
        "_index" : "catalogo",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "nome" : "João Silva",
          "interesses" : [
            "futebol",
            "música",
            "literatura"
          ],
          "cidade" : "São Paulo",
          "formação" : "Letras",
          "estado" : "SP",
          "país" : "Brasil"
        }
      }
    ]
  }
}
```

### GET [index]/_search/?q=[term]
If you want to query the documents by a term
We call this as free search, independently the field, the elastic seach is gonna seach the value 
exemple
```
GET catalogo/_search/?q=futebol
```

### GET [index]/_seach/?q=field:value
Seach in a specific field


```
GET catalogo/_search/?q=estado:SP
```

## PUT
### PUT [index]/_doc/{id}
To create or update a document
exemple
```
PUT /catalogo/_doc/50
{
    "nome": "Marcelo Ricardo de Oliveira",
    "interesses": [
        "cinema",
        "música",
        "programação"
    ],
    "cidade": "São Paulo",
    "formação": "Tecnologia da Informação",
    "estado": "SP",
    "país": "Brasil"
}
```

if you send a PUT many times, you gonna see the `_version` increasing. In fact, you're creating a new document every time, it means that elasticsearch documents are immutable, 


### PUT /[new_index]
To create a new index

exemple: 
```
PUT /catalogo_v2
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 0
    }
  },
  "mappings": {
    "properties": {
      "cidade": {
        "type": "text",
        "index": true,
        "analyzer": "portuguese"
      },
      "estado": {
        "type": "text"
      },
      "formação": {
        "type": "text",
        "index": true,
        "analyzer": "portuguese"
      },
      "interesses": {
        "type": "text",
        "index": true,
        "analyzer": "portuguese"
      },
      "nome": {
        "type": "text",
        "index": true,
        "analyzer": "portuguese"
      },
      "país": {
        "type": "text",
        "index": true,
        "analyzer": "portuguese"
      }
    }
  }
}
```

## DELETE
### DELETE [index]/_doc/{id}
To delete a document

exemple
```
DELETE catalogo/_doc/1
```

## HEAD
### HEAD [index]/_doc/{id}
To know if the document exists, the response will be a http status code

exemple:
```
HEAD catalogo/_doc/1
```
curl exemple:
```
curl -IHEAD "http://localhost:9200/catalogo/_doc/1"
```
response
```
200 - OK
```
==

## Concepts


| Relational Database   	|  Elasticsearch  	|
|------------------------	|---------------	|
| instance                  | index             |
| table                     | type              |
| schema                    | mapping           |
| tuple                     | document          |
| column                    | attribute         |


### Shards
The "pieces" of index / partitions


## Pagination

### GET [index]/_search/?q=value&ize=10&from=0
To paginate the results
`value` means the value to be search
`size`: page size
`from`: offset

GET catalogo/_search/?q=futebol&size=10&from=10

## Mapping
Elasticsearch defines automatically the index from the document

### GET [index]/_mapping
it's gonna retrieve the schema 

exemple
```
GET catalogo/_mapping
```

Response
```
{
  "catalogo" : {
    "mappings" : {
      "properties" : {
        "cidade" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "estado" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "formação" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "interesses" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "nome" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "país" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}

```

"Core" Data Type
- string
- int
- data

"Complex" data type
- arrays
- IPV4
- IPV6
- Geospacial information

## Inverted indexing

It's an association between tokens (values) and the source documents

Tokens
```
futebol
João
música
Pedro
```

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


#### _analyze
We can use `_analyze` to analize the text, it uses the standard analyzer by default

```
GET catalogo/_analyze
{
  "text": "Eu nasci há 10 mil (sim, isso mesmo, 10 mil) anos atrás"
}
```

If we want a different analyzer, we have to pass the attribute `analyzer`

```
GET catalogo/_analyze
{
  "analyzer": "simple", 
  "text": "Eu nasci há 10 mil (sim, isso mesmo, 10 mil) anos atrás"
}
```

```
GET catalogo/_analyze
{
  "analyzer": "portuguese", 
  "text": "Eu nasci há 10 mil (sim, isso mesmo, 10 mil) anos atrás"
}
```
