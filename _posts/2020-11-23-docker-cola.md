---
title: "Comandos Docker"
layout: post
date: 2020-11-19
image: 
headerImage: false
tag:
- docker
- comandos
published: false
category: blog
author: beatrizuezu
description: Alguns comandos para você utilizar o Docker
---

-
---


# Docker

## o que são containers?
- segregação de processos do mesmo Kernel (Isolamento)
- Sistema de arquivos criados a partir de uma imagem
- Ambientems leve e portáteis no qual aplicações são executadas
- Encapsula todos os binários e bibliotecas necessárias para execução de uma App
- algo entre um chroot e uma VM

Boa prática
- tentar ser o mais minimalista possível ao criar um container
- uma aplicação deverá ser em um container enquanto o banco de dados deverá ser em outro.
Assim você terá um ambiente mais isolado, organizado e com a capacidade de escalar de forma independente.

## o que são imagens docker?
- modelo de sistema de arquivos somente-leitura usado para cirar containers
- imagens são criadas através de um processo chamado **build** 
- são armazenadas em repositórios no Registry
- são compostas por uma ou mais camadas (layers)
- um comando representa uma ou mais mudanças no sistema de arquivo
- uma camada também é chamada de de imagem intermediária
- a junção dessas camandas formam a imagem
- a apenas a última camada pode ser alterada quando o container for iniciado
- AUFS(Advance multi-layered unification filesystem) é muito usado
- O grande objetivo dessa estratégia de dividir uma imagem em camadas é o reuso
- É possível compor imagens a partir de uma camadas de outras imagens
