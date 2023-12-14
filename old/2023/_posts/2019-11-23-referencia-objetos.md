---
title: "Referência de objetos"
layout: post
date: 2019-11-23
image: assets/images/lista.jpg
headerImage: false
tag:
- variável
- memória
- referência de objetos
published: true
category: blog
author: beatrizuezu
description: Entendendo as referências de objetos
---

Já aconteceu de você ter duas variáveis com os mesmos valores e alterar uma variável a outra também é alterada?

---


# O que é variável em programação?

Variável é uma informação que será armazenada em um endereço da memória do computador, para ficar mais fácil identificar esse endereço atribuimos um nome para esse valor.

# Caixas e post-its?
Para melhor entendimento podemos usar a metáfora da caixa e do post-its. Em algumas linguagens a variável representa uma caixa que vai armazenar um determinado valor, ou seja, cada caixa representado por uma variável terá um endereço na memória.

![variável autora representado pela caixa com valor beatriz](/assets/images/box-autora.png){: .center-image }
<figcaption class="caption">variável autora representado pela caixa com valor beatriz</figcaption>

![variável nome representado pela caixa com valor beatriz](/assets/images/box-nome.png){: .center-image }
<figcaption class="caption">variável nome representado pela caixa com valor beatriz</figcaption>

Em Python, essa metáfora não é válida. Podemos dizer que as variáveis são como post-its, as variáveis quando tiverem o mesmo valor terão a mesma referência.

![Valor beatriz com as variáveis nome e autora representados pelo post-its](/assets/images/postits.png){: .center-image }
<figcaption class="caption">Valor beatriz com as variáveis nome e autora representados pelo post-its</figcaption>

Isso pode ser observado nesse código:

{% gist beatrizuezu/d840aa64b4a2f7f2f47bf42395c6fd2f %}

A função [id()](https://docs.python.org/pt-br/3/library/functions.html#id) retorna o endereço da memória que está armazenado aquele determinado valor.

# listas, tuplas, dicionários e conjuntos armazenam referências
Ao declarar listas, tuplas ou dicionários com os mesmos valores, os objetos não terão o mesmo id, ou seja a mesma referência, porém se os valores forem iguais "apontarão" para o mesmo id.

![Desenho ilustrativo das variaveis lista_1 e lista_2](/assets/images/lista.jpg){: .center-image }
<figcaption class="caption">Variávies lista_1 e lista_2 possuem os mesmos valores</figcaption>

Podemos observar no código abaixo que a variável `lista_1` e `lista_2` possuem os ids diferentes, mas o valor de index 0 de ambas as listas possuem o mesmo id.

Podemos usar o operador `==` para comparar os valores, enquanto o `is` é utilizado para comparar os ids

{% gist beatrizuezu/35254604b2ce26757ff7bdb1ae61bd26 %}

Caso atribuirmos o valor de `lista_2` em `lista_1`, eles terão o mesmo id.

{% gist beatrizuezu/2219a404ebd014633f03ce53ca2f4b3c %}


Atente-se quando for manipular uma lista ou dicionário que for atribuída a uma outra variável para nāo perder os valores do objeto, pois estaremos alterando a mesma referência!

{% gist beatrizuezu/d0081a5b9ee6bbfed64cd8b4162a8f1e %}

Para a alteração não afetar a variável original, podemos usar o módulo `copy` com as funçōes [`copy()`](https://docs.python.org/pt-br/3/library/copy.html?highlight=copy#copy.copy), que retorna uma cópia rasa, e [`deepcopy()`](https://docs.python.org/pt-br/3/library/copy.html?highlight=copy#copy.deepcopy), que retorna uma cópia profunda.

## copy
No código abaixo estamos usando a função `copy()`
{% gist beatrizuezu/c78e72f96cd8568611cba991e98997a4 %}

Notamos que ao alterar a lista interna (index 2) da variável `lista_2` alterou também a variável `lista_1`. Podemos dizer que tanto a `lista_1` quanto a `lista_2` "apontam" para a mesma lista interna (`[9,8]`)

![Desenho ilustrativos das variaveis lista_1 e lista_2 apontando para a mesma lista [9,8]](/assets/images/lista-manipulacao.jpg){: .center-image }
<figcaption class="caption">Desenho ilustrativos das variaveis lista_1 e lista_2 apontando para a mesma lista [9,8]</figcaption>

## deepcopy
No `deepcopy()` é feita uma cópia profunda

{% gist beatrizuezu/b1ddc80c92da70608384fa0f7864d719 %}

As variáveis não compartilham da mesma referência quando usado o `deepcopy()`,logo o index 2 da variável `lista_1` e da variável `lista_2` são diferentes.


Espero que esse post tenha ficado simples e que você tenha entendido sobre referência de objetos!

Obrigada por ter lido <3

---
