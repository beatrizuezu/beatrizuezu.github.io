---
title: "Twelve Factor App"
layout: post
date: 2021-06-24
headerImage: false
tag:
- twelve factor app
- metodologia de aplicações doze fatores
published: true
category: blog
author: beatrizuezu
description: 
---
Essas são as minhas anotações do meu estudo sobre 12 Factor App, aproveitei também para compartilhar esses conhecimentos com o meu time do trabalho :)

---

# Metodologia de aplicações doze fatores ou 12 Factor App

## O que é?

É uma metodologia criada pelo Heroku que traz doze regras/princípios/boas práticas de desenvolvimento de software que podem ser aplicada em qualquer linguagem de programação e que utilizem qualquer combinação de serviços de suportes (banco de dados, caches...).

Essas práticas nos ajudam a automatizar e minimizar a configuração inicial do projeto, ter uma portabilidade entre os ambientes em que o projeto será executado, facilitar o deploy em serviços em cloud, minimizar divergências entre ambientes e escalar a aplicação de uma forma sustentável.

[Documentação em portugês](https://12factor.net/pt_br/)

[Documentação em inglês](https://12factor.net/)


## Quando e como utilizar?

Dependendo do projeto conseguimos aplicar a maioria dos fatores,mas será necessário o entendimento da necessidade para que não torne o projeto complexo. Quanto mais fatores utilizado mais o software estará pronto para ser utilizado como um serviço (*Software as a Service*, SAAS).

## Os 12 fatores

### 01 - Base de código (Codebase)

> One codebase tracked in revision control, many deploys

*"Uma base de código com rastreamento utilizando controle de revisão, muitos deploys"*

Ou seja, utilizar um sistema de controle de versão de código, como git, mercurial ou subversion, para ter o código centralizado em um repositório. Se tivermos mais de uma aplicação compartilhando o mesmo código, estamos violando os 12 fatores. A solução para isso é fatorar o código compartilhado em uma biblioteca e incluir  através de um gerenciador de [dependências](próximo tópico.) 

Várias aplicações/serviços devem estar base de código independentes, para que elas sejam auto contidas, assim a alteração em uma aplicação não dependa e impacte a outra.

Podemos ter alguns ambientes para fazer o deploy (implantação do código), ambiente de desenvolvimento, também chamado de ambiente local, homologação (staging) e produção (production). Cada pessoa desenvolvedora pode ter uma versão da aplicação rodando no ambiente de desenvolvimento (ambiente local), assim como os ambientes de homologação e produção podemos ter versões diferentes em cada deploy.


[Documentação em portugês](https://12factor.net/pt_br/codebase)

[Documentação em inglês](https://12factor.net/codebase)

### 02 - Dependências (Dependencies)

> Explicitly declare and isolate dependencies

*"Declare e isole explicitamente as dependências"*

Maioria das linguagens de programação possuem gerenciadores de dependências, com elas podemos instalar as bibliotecas que são utilizadas na aplicação. Todas as dependências precisam estar explicitamentes declaradas por meio de um manifesto de declaração de dependência, ou seja, haverá um arquivo que contém as bibliotecas que são utilizadas na aplicação e qual a versão usada na aplicação. Podemos utilizar ferramentas que isolem o ambiente para que não tenhamos conflitos de dependências com outras bibliotecas de outros projetos.

Por exemplo, no Python podemos utilizar o pip, pipenv, poetry como gerenciadores de dependência e o virtualenv para isolar o ambiente. No Java temos o gradle e o maven, no Ruby bundler como gerenciadores de dependência.

Isso simplifica o processo de configuração da aplicação para as pessoas desenvolvedoras e também facilita a portabilidade de servidor.


[Documentação em portugês](https://12factor.net/pt_br/dependencies)

[Documentação em inglês](https://12factor.net/dependencies)

### 03 - Configurações (Config)

> Store config in the environment

*"Armazene as configurações no ambiente"*

Esse fator significa que devemos armazenar as configurações no ambiente, pois as configurações do aplicativo variam em cada ambiente (local, staging e production) e assim conseguimos ter as configurações separadas do código. Entende-se como configuração como credenciais para serviços externos e da aplicação, variáveis que diferem em cada ambiente, como o hostname e banco de dados.

Na aplicação podemos ter um arquivo para armazenar essas configurações e que não deve ser versionado. Chamamos essas configurações de variáveis de ambiente (*environment variables* ou *env vars*), as variáveis são definidas no próprio sistema operacional.


[Documentação em portugês](https://12factor.net/pt_br/config)

[Documentação em inglês](https://12factor.net/config)

### 04 - Serviços de Apoio (Backing Services)

> Treat backing services as attached resources

*"Trate serviços de apoio como recursos anexados"*

São considerados serviços de apoio qualquer serviço terceiro como banco de dados, sistema de mensageria e filas, serviços de emails e de cache, serviços de métricas e logs, ou seja, são serviços que a aplicação utiliza como parte do sistema, isto é um recurso anexado.

Esse fator sugere que em um serviço local e de terceiros não deve haver distinção. Isso quer dizer que, caso seja necessário a troca de um recurso para outro não deve haver mudança no código, a não ser que tenhamos algo muito específico que estamos utilizando do recurso. Podemos utilizar o exemplo a mudança do banco de dados MySQL para um outro banco seja PostgreSQL ou RDS da Amazon, isso deve ser transparente para a aplicação.

[Documentação em portugês](https://12factor.net/pt_br/backing-services)

[Documentação em inglês](https://12factor.net/backing-services)

### 05 - Construa, lance, execute (Build, release, run)
> Strictly separate build and run stages

*"Separe estritamente os estágios de construção e execução"*

O processo do deploy da base de código ocorre em três estágios:

#### Construção (*Build*)

Nesse estágio é obtido as dependências do projeto, compilado em binário e lida com os arquivos estáticos (*assets*) para que seja gerado o executável do projeto.

#### Lançamento (*Release*)

O estágio de release é a combinação do build com as configurações de ambiente em que será feita o deploy.É muito comum termos versões de release com um identificador único, pode ser utilizado um número incremental (como `v123`), [versionamento semântico](https://semver.org/lang/pt-BR/) ( como `2.1.1`) ou por data e hora (como `202106052230`), assim caso precise fazer a reversão da versão anterior, também conhecido como *rollback*, fique fácil de identificar o release.

#### Execução (*Run*)

É nesse estágio em que será inicializado os processos para que a aplicação seja executada.


Para cada estágio há ferramentas especificas da cada linguaguem em fazer a automatização do build, release e run.

[Documentação em portugês](https://12factor.net/pt_br/build-release-run)

[Documentação em inglês](https://12factor.net/build-release-run)


### 06 - Processos (*Processes*)
> Execute the app as one or more stateless processes

*"Execute a aplicação como um ou mais processos que não armazenam estado"*

Esse fator significa que os processos não armazenam estados (*stateless*) e não devem compartilhar nada entre si. Caso os estados precisem ser armazenados, devemos utilizar um serviço de apoio como uma base dados ou sistema de cache.


[Documentação em portugês](https://12factor.net/pt_br/processes)

[Documentação em inglês](https://12factor.net/processes)


### 07 - Vínculo de Portas (*Port binding*)

> Export services via port binding

*"Exporte serviços via vínculo de portas"*

A aplicação exporta uma porta HTTP como um serviço através dessa porta, assim atua como um serviço independente podendo servir com um serviço de apoio e não dependa de servidores externos.

[Documentação em portugês](https://12factor.net/pt_br/port-binding)

[Documentação em inglês](https://12factor.net/port-binding)


### 08 - Concorrência (*Concurrency*)

> Scale out via the process model

*"Escale através do processo modelo"*

A aplicação deve ter um modelo para conseguir escalar. A escalabilidade pode ser:
- Horizontal: adição de mais servidores com as mesmas configurações de recursos (CPU, RAM);

- Vertical: aumentar os recursos de cada servidor no qual a aplicação ou o sistema está sendo executado.

[Documentação em portugês](https://12factor.net/pt_br/concurrency)

[Documentação em inglês](https://12factor.net/concurrency)


### 09 - Descartabilidade (*Disposability*)

> Maximize robustness with fast startup and graceful shutdown

*"Maximize robustez com inicialização rápida e desligamento gracioso"*

Os processos devem inicializar em poucos segundos para que a aplicação esteja pronta para receber requisições. A inicialização rápida promove agilidade para os processos de releases e escalabilidade.

Quanto o desligamento gracioso quer dizer que o servidor para de receber novas requisições e finaliza o que estiver em andamento e encerrar o processo.


[Documentação em portugês](https://12factor.net/pt_br/disposability)

[Documentação em inglês](https://12factor.net/disposability)


### 10 - Paridade entre desenvolvimento e produção (*Dev/prod parity*)

> Keep development, staging, and production as similar as possible

*"Mantenha o desenvolvimento, homologação e produção o mais similares possível"*

Manter os ambientes mais similares possíveis para que tenhamos o mínimo de divergências entre os ambientes de desenvolvimento, staging e produção.

Esse fator pretende diminuir as divergências em três áreas:
- versão de código: a pessoa desenvolvedora escreve o código em algumas horas e faz o deploy minutos depois;
- equipes: a equipe que está envolvida no desenvolvimento daquele código realiza e acompanha o deploy em produção;
- ferramentas: utilizar os mesmos serviços de apoio nos ambientes.

[Documentação em portugês](https://12factor.net/pt_br/dev-prod-parity)

[Documentação em inglês](https://12factor.net/dev-prod-parity)

### 11 - Logs (*Logs*)

> Treat logs as event streams

*"Trate logs como fluxos de eventos"*

Os logs nos dão visibilidade sobre o comportamento da aplicação, podemos agregar os eventos e ordernar por tempo de execução de todos os processos em execução e de serviços de apoio.

No ambiente de desenvolvimentos, os logs devem ser exibidos no terminal, enquanto que nos ambientes de staging e produção devemos utilizar ferramentas (como Graylog, Logentries, ELK) para nos auxiliar com a coleta e armazenamento dos logs.

[Documentação em portugês](https://12factor.net/pt_br/logs)

[Documentação em inglês](https://12factor.net/logs)


### 12 - Processos administrativos (*Admin processes*)

> Run admin/management tasks as one-off processes

*"Rode tarefas de administração/gestão em processos pontuais"*

Processos administrativos são processos pontuais, como migrações, scripts e execuções no console, devem ser executados em ambientes idênticos e isolados de ambientes em que tarefas de processamento longo rodam.

Esses processos também devem ser versionados, estar dentro do processo de deploy e utilizar as mesmas configurações do ambiente.

[Documentação em portugês](https://12factor.net/pt_br/admin-processes)

[Documentação em inglês](https://12factor.net/admin-processes)

### Notas
[Curso de git e github]( https://www.udemy.com/course/git-e-github-para-iniciantes/) - gratuito

## Referências

- Youtube - [The Twelve Factor APP - Zup Insights](https://youtu.be/C2VbGlOBYyw)

- Youtube - [12 Factor App (Boas práticas para criar uma aplicação SaaS) // Dicionário do Programador](https://youtu.be/Xayg7f2utgk)

- Alura - [The Twelve-Factor App: Metodologia para construção de aplicações robustas](https://www.alura.com.br/curso-online-the-twelve-factor-app)

- Medium - [Twelve Factor app - Boas práticas para microsserviços](https://medium.com/trainingcenter/twelve-factor-app-f51665af95e7)

Obrigada por ter lido <3

---
