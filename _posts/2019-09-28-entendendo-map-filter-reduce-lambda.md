---
title: "Entendendo map, filter, reduce e lambda"
layout: post
date: 2019-09-28
image: assets/images/lambda-icon.png
headerImage: false
tag:
- python
- map
- filter
- reduce
- lambda
- list comprehension
published: false
category: blog
author: beatrizuezu
description: Conhecendo mais sobre map, filter, reduce e lambda
---

Antes de falar sobre map, filter e reduce, precisamos entender sobre **função de ordem superior**.

---


## Função de ordem superior
Função de ordem superior é uma função que:
  - recebe uma função como argumento;
  - ou devolve uma função como resultado.

Map, filter e reduce são as funções de ordem superior mais conhecidas.


## Map

![](/assets/images/map.png){: .center-image }


A função map recebe uma função (`dobrar_numero`) e um iterável (`range(4)`), cada item desse iterável (números de 0 à 3) é aplicada na função, o que faz o map retornar um objeto map e convertemos esse resultado em uma lista.

## Filter

![](/assets/images/filter.png){: .center-image }

A função filter recebe uma função (`maior_que_zero`) e um iterável (`valores`), cada item do iterável é aplicada a condição da função que faz o "filtro", retornando também um objeto filter e convertemos o resultado em uma lista

## Reduce

![](/assets/images/reduce.png){: .center-image }

A ideia de usar o reduce é aplicar uma função (`somar`) a cada item de um iterável (`range(5)`, ou seja, números de 0 à 4) a fim de ser "reduzida" a um único valor.


## Lambda

Função lambda também é conhecida como função anônima, e é comum vermos a função lambda ser usada junto com map, filter ou reduce afim de encurtar as funções criadas pelo comando `def`


#### Map e lambda

![](/assets/images/map-lambda.png){: .center-image }

#### Filter e lambda

![](/assets/images/filter-lambda.png){: .center-image }


#### Reduce e lambda

![](/assets/images/reduce-lambda.png){: .center-image }


## Deixando map, filter e reduce mais legíveis

Para tornar o código mais legível é possível substituir as funções map e filter por list comprehension e a função reduce por `sum()`, `all()` e `any()`.

Como já diz no [ Zen of Python](https://www.python.org/dev/peps/pep-0020/)
> Readability counts

### List Comprehension

#### map vs list comprehension

![](/assets/images/map-lambda.png){: .center-image }
<figcaption class="caption">código usando map</figcaption>

![](/assets/images/listcomp-map.png){: .center-image }
<figcaption class="caption">código substituindo map por list comprehension</figcaption>


#### filter vs list comprehension

![](/assets/images/filter-lambda.png){: .center-image }
<figcaption class="caption">código usando filter</figcaption>


![](/assets/images/listcomp-filter.png){: .center-image }
<figcaption class="caption">código substituindo filter por list comprehension</figcaption>

#### reduce vs sum, all, any

![](/assets/images/reduce-lambda.png){: .center-image }
<figcaption class="caption">código usando filter</figcaption>

![](/assets/images/sum.png){: .center-image }
<figcaption class="caption">código substituindo reduce por sum</figcaption>


A função `all()` recebe um iterável e se todos os elementos forem verdadeiros a função retorna `True`

![](/assets/images/all.png){: .center-image }

<figcaption class="caption">exemplo de código utilizando a função all</figcaption>


A função `any()` recebe um iterável e se algum elemento for verdadeiro a função retorna `True`
![](/assets/images/any.png){: .center-image }
<figcaption class="caption">exemplo de código utilizando a função any</figcaption>


Espero que esse post tenha ficado simples e que você tenha entendido sobre map, filter, reduce e lambda!

Obrigada por ter lido <3

---
