---
title: "Visualizando query SQL a partir do ORM Django"
layout: post
date: 2019-02-27 22:58
image: /assets/images/markdown.jpg
headerImage: false
tag:
- django
- orm
- sql
published: true
category: blog
author: beatrizuezu
description: Um dos motivos para falar sobre esse assunto foi a dificuldade que tive para entender como funcionava a ORM do Django. Em uma das minhas primeiras experiências com Django precisei gerar um relatório e como já tinha trabalhado com SQL resolvi fazer com SQL mesmo e posteriormente e passaria a query pra ORM pra entender. Uma maneira simples para o meu entendimento foi associar o que eu sabia de SQL para entender o ORM.
---

Falei sobre esse assunto na palestra que fiz na TDC 2017 (The Developer’s Conference) e vou mostrar como visualizar query SQL do ORM do Django.

Um dos motivos para falar sobre esse assunto foi a dificuldade que tive para entender como funcionava a ORM do Django. Em uma das minhas primeiras experiências com Django precisei gerar um relatório e como já tinha trabalhado com SQL resolvi fazer com SQL mesmo e posteriormente e passaria a query pra ORM pra entender. Uma maneira simples para o meu entendimento foi associar o que eu sabia de SQL para entender o ORM.

---

## ORM o que?

ORM significa Object Relational Mapper e é uma biblioteca que automatiza a transferência de dados do banco de dados relacional entre objetos do model. O legal de se usar é que pode acelerar a velocidade do desenvolvimento permitindo ler, escrever, atualizar e deletar dados do banco, além de ser compatível com diferentes banco de dados (MySQL, SQLite, PostgreSQL…) e ser nativa no Django.

![Relação tabela vs classes](/assets/images/relacao-tabela-vs-classes.png){: .center-image }
<figcaption class="caption">https://www.fullstackpython.com/object-relational-mappers-orms.html</figcaption>

## Model vs Tabela

Criei um exemplo (e você pode conferir nesse repositório [aqui](https://github.com/beatrizuezu/de-sql-para-orm-django) e fazer na sua máquina para entender também) para mostrar como fica no model e na tabela do banco de dados (estou usando mysql).

{% gist beatrizuezu/92faae7b82cbf932c3983e14e3717ea9 %}

{% gist beatrizuezu/5ec77ef4799a3d1b5e8a178e5d9fa6de %}

Enquanto no banco de dados temos:

{% gist beatrizuezu/dfbe3a002291767f2e3d0d8b3ed3432b %}

{% gist beatrizuezu/20a162167367e2fed3171404bc251c67 %}


## Como Funciona?

Tenho uma determinada ORM, vou utilizar essa abaixo como exemplo:

```
Produto.objects.all()
```

Vamos entender o fluxo para obtermos o script SQL:

![Fluxo para obtermos o script SQL](/assets/images/fluxo-script-sql.png){: .center-image }
<figcaption class="caption">Fluxo parar obtermos o script SQL </figcaption>

### Model
É a representação dos dados, contém os campos e os métodos. No nosso exemplo, o nosso model é Produto.

### Manager
O manager está acoplado ao Model, para acessar qualquer objeto salvo no banco é preciso acessar o Manager, isto é o objects presente em todo Model.

### QuerySet
É o conjunto de ações quer serão realizadas no banco (select, insert, update, delete). Representado pelo `all()` no nosso exemplo.

### Query
Cria uma estrutura de dados complexa com todos os elementos presentes em uma consulta. É gerado uma representação SQL de um QuerySet, ou seja, é através desse método que conseguimos ver as conversões do ORM para SQL (MySQL, PostgreSQL, SQLite).

### SQLCompiler
Recebe o SQL da Query e executa de acordo com as regras específicas do backend escolhido.

## Métodos
Vou abordar alguns métodos da ORM, você pode visualizar todos os métodos [aqui](https://docs.djangoproject.com/en/dev/ref/models/querysets/).

### .all()
O método `.all()` retornará todos os produtos.

```
In [1]:from core.produtos.models import Produto
In [2]:Produto.objects.all()
Out [2]:<QuerySet [<Produto: Produto 1 da Categoria 1>,
<Produto: Produto 2 da Categoria 1>, <Produto: Produto 3 da Categoria 1>,
<Produto: Produto 4 da Categoria 1>, <Produto: Produto 5 da Categoria 1>,
<Produto: Produto 1 da Categoria 2>, <Produto: Produto 2 da Categoria 2>,
<Produto: Produto 3 da Categoria 2>, <Produto: Produto 4 da Categoria 2>,
<Produto: Produto 5 da Categoria 2>, <Produto: Produto 1 da Categoria 3>,
<Produto: Produto 2 da Categoria 3>, <Produto: Produto 3 da Categoria 3>,
<Produto: Produto 4 da Categoria 3>, <Produto: Produto 5 da Categoria 3>,
<Produto: Produto 1 da Categoria 4>, <Produto: Produto 2 da Categoria 4>,
<Produto: Produto 3 da Categoria 4>, <Produto: Produto 4 da Categoria 4>,
<Produto: Produto 5 da Categoria 4>, ‘…(remaining elements truncated)…’]>
```

e para visualizarmos o script SQL:

```
In [3]: orm = Produto.objects.all()
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`id`, `produtos_produto`.`nome`,
  `produtos_produto`.`valor`, `produtos_produto`.`categoria_id`
FROM `produtos_produto`
```

### .filter()
O método `.filter()` retornará todos os produtos em uma determinada condição.

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.filter(id=1)
Out[2]: <QuerySet [<Produto: Produto 1 da Categoria 1>]>
```

e para visualizarmos o script SQL:

```
In [3]: orm = Produto.objects.filter(id=1)
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`id`, `produtos_produto`.`nome`,
  `produtos_produto`.`valor`, `produtos_produto`.`categoria_id`
FROM `produtos_produto` WHERE `produtos_produto`.`id` = 1
```

### .filter(campo__lookup)
Podemos passar lookups quando utilizamos o `.filter()`, você pode conferir todos os lookups [aqui](https://docs.djangoproject.com/en/dev/ref/models/querysets/#field-lookups):

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.filter(id__in=[1,3,5])
Out[2]: <QuerySet [<Produto: Produto 1 da Categoria 1>,
<Produto: Produto 3 da Categoria 1>, <Produto: Produto 5 da Categoria 1>]>
```

e para visualizarmos o script:

```
In [3]: orm = Produto.objects.filter(id__in=[1, 3 ,5])
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`id`, `produtos_produto`.`nome`,
  `produtos_produto`.`valor`, `produtos_produto`.`categoria_id`
FROM `produtos_produto` WHERE `produtos_produto`.`id` IN (1, 3, 5)
```

### .filter(fk__campo)

Conseguimos acessar o campo de uma chave estrangeira utilizando ORM:

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.filter(categoria__nome='categoria_1')
Out[2]: <QuerySet [<Produto: Produto 1 da Categoria 1>,
<Produto: Produto 2 da Categoria 1>, <Produto: Produto 3 da Categoria 1>,
<Produto: Produto 4 da Categoria 1>, <Produto: Produto 5 da Categoria 1>]>
```

visualizando em SQL:

```
In [3]: orm = Produto.objects.filter(categoria__nome='categoria_1')
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`id`, `produtos_produto`.`nome`,
  `produtos_produto`.`valor`, `produtos_produto`.`categoria_id`
FROM `produtos_produto`
INNER JOIN `categorias_categoria` ON (
  `produtos_produto`.`categoria_id` = `categorias_categoria`.`id`
)
WHERE `categorias_categoria`.`nome` = categoria_1
```

o interessante é que não precisamos nos preocupar com os joins do SQL quando utilizamos ORM!

### .get()
O método `.get()` é semelhante ao `.filter()`, vamos entender as diferenças posteriormente.

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.get(id=1)
Out[2]: <Produto: Produto 1 da Categoria 1>
```

visualizando em SQL:

```
In [3]: orm = Produto.objects.get(id=1)
In [4]: print(orm.query)
 — — — — — —— — — — — — — — — — — — — — — — — — — — — — — — — —
AttributeError Traceback (most recent call last)
<ipython-input-4–9d5bf753c2b6> in <module>()
 — → 1 print(script.query)
AttributeError: ‘Produto’ object has no attribute ‘query’
```

EITA! Caímos em um erro. Isso ocorreu porque diferente do `.filter()`, o `.get()` não retorna um queryset e sim o objeto do model.

```
In [5]: script = Produto.objects.filter(id=1)
In [6]: type(script)
Out[6]: django.db.models.query.QuerySet
In [7]: script = Produto.objects.get(id=1)
In [8]: type(script)
Out[8]: core.produtos.models.Produto
```

Mas ainda sim conseguimos visualizar o SQL dessa ORM:

```
In [5]: from django.db import connection
In [6]: connection.queries
Out[6]: [{'sql':
'SELECT
  `produto_produto`.`id`, `produto_produto`.`nome`,
  `produto_produto`.`valor`, `produto_produto`.`categoria_id`
FROM `produto_produto`
WHERE `produto_produto`.`id` = 1',
'time': '0.000'}]
```

### .exclude()
Esse método trará os objetos que não correspondem aos parâmetros fornecidos:

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.exclude(id__gte=4)
Out[2]: <QuerySet [<Produto: Produto 1 da Categoria 1>,
<Produto: Produto 2 da Categoria 1>, <Produto: Produto 3 da Categoria 1>]>
```
visualizando em SQL:

```
In [3]: orm = Produto.objects.exclude(id__gte=4)
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`id`, `produtos_produto`.`nome`,
  `produtos_produto`.`valor`, `produtos_produto`.`categoria_id`
FROM `produtos_produto` WHERE NOT (`produtos_produto`.`id` >= 4)
```

### .values()
Com o `.values()` podemos passar os campos que sejam retornados no queryset, além disso a queryset retornará dicionários.

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.values(‘nome’, ‘valor’)
Out[2]: <QuerySet [
{‘valor’: Decimal(‘2.00’), ‘nome’: ‘Produto 1 da Categoria 1’},
{‘valor’: Decimal(‘5.00’), ‘nome’: ‘Produto 2 da Categoria 1’},
{‘valor’: Decimal(‘20.00’), ‘nome’: ‘Produto 3 da Categoria 1’},
{‘valor’: Decimal(‘15.00’), ‘nome’: ‘Produto 4 da Categoria 1’},
{‘valor’: Decimal(‘10.00’), ‘nome’: ‘Produto 5 da Categoria 1’},
{‘valor’: Decimal(‘5.00’), ‘nome’: ‘Produto 1 da Categoria 2’},
{‘valor’: Decimal(‘7.00’), ‘nome’: ‘Produto 2 da Categoria 2’},
{‘valor’: Decimal(‘20.00’), ‘nome’: ‘Produto 3 da Categoria 2’},
‘…(remaining elements truncated)…’]>
```
visualizando em SQL:

```
In [3]: orm = Produto.objects.values(‘nome’, ‘valor’)
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`nome`, `produtos_produto`.`valor`
FROM `produtos_produto`
```

### .values_list()
Similar ao `.values()`, a diferença é que a queryset retorna tupla.

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.values_list(‘nome’)
Out[2]: <QuerySet [
(‘Produto 1 da Categoria 1’,), (‘Produto 2 da Categoria 1’,),
(‘Produto 3 da Categoria 1’,), (‘Produto 4 da Categoria 1’,),
(‘Produto 5 da Categoria 1’,), (‘Produto 1 da Categoria 2’,),
(‘Produto 2 da Categoria 2’,), (‘Produto 3 da Categoria 2’,),
(‘Produto 4 da Categoria 2’,),’…(remaining elements truncated)…’]>
```
Visualizando em SQL:

```
In [3]: orm = Produto.objects.values_list(‘nome’)
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`nome`
FROM `produtos_produto`
```

### .values_list(field, flat=True)
Quando utilizamos o flat=True como paramêtro do `.values_list()` o resultado são retornado em valores únicos ao em vez de tuplas.

```
In [1]: from core.produtos.models import Produto
In [2]: Produto.objects.values_list(‘nome’, flat=True)
Out[2]:<QuerySet [‘Produto 1 da Categoria 1’, ‘Produto 2 da Categoria 1’,
‘Produto 3 da Categoria 1’, ‘Produto 4 da Categoria 1’,
‘Produto 5 da Categoria 1’, ‘Produto 1 da Categoria 2’,
‘Produto 2 da Categoria 2’, ‘Produto 3 da Categoria 2’,
‘Produto 4 da Categoria 2’, ‘Produto 5 da Categoria 2’,
‘Produto 1 da Categoria 3’, ‘Produto 2 da Categoria 3’,
‘…(remaining elements truncated)…’]>
```

Visualizando em SQL:

```
In [3]: orm = Produto.objects.values_list(‘nome’, flat=True)
In [4]: print(orm.query)
SELECT
  `produtos_produto`.`nome`
FROM `produtos_produto`
```

### .annotate()
Retorna um queryset agrupado levando em consideração cada objeto.

```
In [1]: from core.produtos.models import Produto
In [2]: from django.db.models import Sum
In [3]: Produto.objects.values(‘categoria’).annotate(Sum(‘valor’))
Out[3]: <QuerySet [{‘valor__sum’: Decimal(‘52.00’), ‘categoria’: 1},
{‘valor__sum’: Decimal(‘66.00’), ‘categoria’: 2},
{‘valor__sum’: Decimal(‘147.60’), ‘categoria’: 3},
{‘valor__sum’: Decimal(‘209.30’), ‘categoria’: 4},
{‘valor__sum’: Decimal(‘87.20’), ‘categoria’: 5}]>
```
No exemplo acima foi agrupado a soma de todas as categorias dos produtos.

Visualizando em SQL:

```
In [4]: orm = Produto.objects.values(‘categoria’).annotate(Sum(‘valor’))
In [5]: print(orm.query)
SELECT
  `produtos_produto`.`categoria_id`,
  SUM(`produtos_produto`.`valor`) AS `valor__sum`
FROM `produtos_produto`
GROUP BY `produtos_produto`.`categoria_id` ORDER BY NULL
```

### .aggregate()
Semelhante ao `.annotate()` com a diferença de retornar um dicionário com valores agregados calculado sobre todo o queryset

```
In [1]: from core.produtos.models import Produto
In [2]: from django.db.models import Avg, Max, Min
In [3]: Produto.objects.aggregate(
   ...: valor_maximo=Max('valor'),
   ...: valor_minimo=Min('valor'),
   ...: valor_medio=Avg('valor')
   ...: )
Out[3]:
{'valor_maximo': Decimal('100.00'),
 'valor_medio': 22.484,
 'valor_minimo': Decimal('2.00')}
```

Visualizando SQL:
```
In [4]: orm = Produto.objects.aggregate(
 …: valor_maximo=Max(‘valor’),
 …: valor_minimo=Min(‘valor’),
 …: valor_medio=Avg(‘valor’)
 …: )
In [5]: print(orm.query)
 — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — -
AttributeError Traceback (most recent call last)
<ipython-input-5–02ba74239328> in <module>()
 — → 1 print(orm.query)
AttributeError: ‘dict’ object has no attribute ‘query’
```

Caímos no erro novamente, o `.aggregate()` não retona um queryset.

```
In [4]: orm = Produto.objects.values(‘categoria’).annotate(Sum(‘valor’))
In [5]: type(orm)
Out[5]: django.db.models.query.QuerySet
In [6]: orm = Produto.objects.aggregate(
 …: valor_maximo=Max(‘valor’),
 …: valor_minimo=Min(‘valor’),
 …: valor_medio=Avg(‘valor’)
 …: )
In [7]: type(orm)
Out[7]: dict
```

Assim como `.get()` também conseguimos visualizar a query SQL:

```
In [4]: from django.db import connection
In [5]: connection.queries
Out[5]:
[{‘sql’: ‘SELECT
  MAX(`produtos_produto`.`valor`) AS `valor_maximo`,
  AVG(`produtos_produto`.`valor`) AS `valor_medio`,
  MIN(`produtos_produto`.`valor`) AS `valor_minimo`
FROM `produtos_produto`’,
 ‘time’: ‘0.000’}]
```

## QuerySet são Lazy
__Todos os métodos que retornam um QuerySet__ são considerados lazy.

Se eu tiver essas consultas abaixo, quantas consultas foram realizadas no banco de dados?

```
In [1]: from core.produtos.models import Produto
In [2]: produtos = Produto.objects.all()
In [3]: categoria_2 = produtos.filter(categoria=2)
In [4]: lista_produto = produtos.values_list(‘nome’, flat=True)
In [5]: print(lista_produto)
```

__APENAS UMA!__

```
In [1]: from core.produtos.models import Produto
In [2]: from django.db import connection
In [3]: produtos = Produto.objects.all()
In [4]: categoria_2 = produtos.filter(categoria=2)
In [5]: lista_produto = produtos.values_list(‘nome’, flat=True)
In [6]: print(len(connection.queries))
0
In [7]: print(lista_produto)
<QuerySet [‘Produto 1 da Categoria 1’, ‘Produto 2 da Categoria 1’,
‘Produto 3 da Categoria 1’, ‘Produto 4 da Categoria 1’,
‘Produto 5 da Categoria 1’, ‘Produto 1 da Categoria 2’,
‘Produto 2 da Categoria 2’, ‘Produto 3 da Categoria 2’,
‘Produto 4 da Categoria 2’, ‘Produto 5 da Categoria 2’,
‘Produto 1 da Categoria 3’, ‘Produto 2 da Categoria 3’,
‘Produto 3 da Categoria 3’, ‘Produto 4 da Categoria 3’,
‘Produto 5 da Categoria 3’, ‘Produto 1 da Categoria 4’,
‘Produto 2 da Categoria 4’, ‘Produto 3 da Categoria 4’,
‘Produto 4 da Categoria 4’, ‘Produto 5 da Categoria 4’,
‘…(remaining elements truncated)…’]>
In [8]: print(len(connection.queries))
1
```

Isso significa que as consulta são realizadas no banco de dados quando pedimos. Mas quando pedimos?

Podemos pedir para serem executadas nas seguintes formas:

* Quando solicitamos somente um resultado:

```
>> Produto.objects.all()[0]
```

* Quando fazemos um slicinf passando um "step":

```
>> Produtos.objects.all()[::2]
```

* Quando iteramos:

```
>> categoria for categoria in Categoria.objects.all()
```

* Quando chamamos o método len() :

```
>> len(Produto.objects.all())
```

* Quando chamamos um list() :

```
>> list(Produto.objects.all())
```

* Quando chamamos um bool() :

```
>> bool(Produto.objects.all())
```

* Quando chamamos o repr() :

```
>> repr(Produto.objects.all())
```

## DICAS

### reset_queries
Se precisar limpar a lista de query do connection, utilize:

```
from django.db import reset_queries
>> reset_queries()
```

### Django logging
Caso queira que a query sql apareça toda vez que seja executada uma consulta orm, adicione ao arquivo `settings.py`

{% gist beatrizuezu/deb68f559403ab9c0ad709c991d3e496 %}

Espero que tenha ajudado a entender um pouco sobre ORM Django :)

Obrigada ❤


esse post foi originalmente escrito no [Medium](https://medium.com/@beatrizuezu/visualizando-query-sql-a-partir-do-orm-django-5771370a9c55) no dia 07/08/2017
