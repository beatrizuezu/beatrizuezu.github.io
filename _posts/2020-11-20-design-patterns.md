---
title: "Design Patterns ou Padrões de Projetos - Parte 1"
layout: post
date: 2020-11-20
image: assets/images/lista.jpg
headerImage: false
tag:
- design patterns
- padrões de projetos
published: false
category: blog
author: beatrizuezu
description: Anotações dos estudos de Desing Patterns(padrões de projetos) - Parte 1
---
-
---


# Design Patterns(Padrões de projeto)

Padrões de projetos são soluções típicas para problemas comuns em projeto de software. Podemos fazer uma analogia com uma planta de obras, podemos ver o resultado e as funcionalidades, mas precisamos fazer a implemetação.

O conceito de padrões de projeto foi apresentado pela GoF (Gang of Four, ou Gangue dos Quatro) no livro Padrões de Projeto - Soluções Reutilizáveis de Software Orientado a Objetos.


## Classificação 

O livro do GoF mostra 23 padrões de projetos, e são classificados em:

- padrões de criação;
- padrões estruturais;
- padrões comportamentais.

### Padrões de criação

Fornecem mecanismos de criação de objetos que aumetnam a flexibilidade e a reutilização de código, como:
- funcionam com base no modo como os objetos podem ser criados;
- isolam os detalhes da criação dos objetos
- o código é independente do tipo do objeto a ser criado.

Exemplo: padrão Singleton.


### Padrões estruturais

Explicam como montar objetos e classes em estruturas maiores, enquanto ainda mantém as estruturas flexíveis e eficientes, como:
- determinam o design da estrutura de objetos e classes para que estes possam ser compostos e resultados mais amplos sejam alcançados;
-  o foco está em simplificar a estrutura e identificar o relacionamento entre classes e objetos;
-  estão centrados em heranças e composição de classes.

Exemplo: padrão Adapter (Adaptador)

### Padrões comportamentais

Cuidam da comunicação eficiente e da indicação de responsabilidade entre objetos, como:
- estão preocupados com a interação entre os objetos e suas responsabilidades;
- os objetos devem ser capazes de interagir e, mesmo assim, devem ter baixo acoplamento.

Exemplo: padrão Observer (Observador).

