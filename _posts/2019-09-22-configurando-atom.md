---
title: "Configurando o Atom para programar em Python"
layout: post
date: 2019-09-22
image: assets/images/atom.png
headerImage: false
tag:
- atom
- ide
- python
- plugin
published: true
category: blog
author: beatrizuezu
description: Plugins do atom para programar em Python
---


Editor bom é aquele é você se sente mais produtiva para programar


---

## Atom
O Atom é um editor de texto open souce desenvolvido pelo GitHub e construído com [Electron](https://github.com/electron/electron). É o editor que mais gosto de usar, já testei alguns, mas não consegui me adaptar ou talvez falte paciência e tempo para isso ¯\\\_(ツ)\_/¯. Vou listar os plugins que uso:


### Plugins

#### [file-icons](https://atom.io/packages/file-icons)

![](/assets/images/file-icons.png){: .center-image }

Gosto de usar esse plugin para os arquivos ficarem com ícones das extensões


#### [linter-flake8](https://atom.io/packages/linter-flake8)

![](/assets/images/linter-flake8.gif){: .center-image }

Linter é uma ferramenta de análise de código que acusa os erros sintáticos, ajuda bastante se você esquece de fechar `)`, `]`, `}` (para evitar isso, se você abrir já fecha para não correr o risco de fechar os parênteses, chaves, colchetes ou aspas), de colocar `:` no final de algum comando entre outras coisas.

Linter para python usando flake8, precisa instalar o flake8(`pip install flake8`).

#### [linter](https://atom.io/packages/linter)

![](/assets/images/linter.gif){: .center-image }

Esse plugin é uma dependência do linter-flake8

#### [highlight-selected](https://atom.io/packages/highlight-selected)

![](/assets/images/highlight-selected.gif){: .center-image }

Ao dar um clique duplo em uma palavra, realça as demais ocorrências

#### [sublime-style-column-selection](https://atom.io/packages/sublime-style-column-selection)

![](/assets/images/column-selection.gif){: .center-image }

Possibilita selecionar multiplas linhas e selecionar um bloco de código em cada linha. Isso é nativo do sublime e lembro de ter isso também no notepad++

#### [busy-signal](https://atom.io/packages/busy-signal)

![](/assets/images/busy-signal.gif){: .center-image }

Mostra que o pacote está executando alguma tarefa


#### [platformio-ide-terminal](https://atom.io/packages/platformio-ide-terminal)

![](/assets/images/terminal.gif){: .center-image }

terminal no editor

#### [autocomplete-python](https://atom.io/packages/autocomplete-python)

![](/assets/images/autocomplete-python.gif){: .center-image }

o próprio nome já diz, rs

#### [python-isort](https://atom.io/packages/python-isort)

![](/assets/images/isort.gif){: .center-image }

deixa os imports na ordem certa
