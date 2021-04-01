---
title: "Funções de Escopo do Kotlin"
layout: post
date: 2021-04-01
headerImage: false
tag:
- funções de escopo
- kotlin
published: true
category: blog
author: beatrizuezu
description: Entendendo funções de escopo do Kotlin
---
Recentemente comecei a trabalhar em um projeto em Kotlin, e no trabalho tenho a oportunidade de aprender e a compartilhar conhecimento com meu time. Esse foi o primeiro assunto que escolhi compartilhar com eles :)

Esse post é o compilado do que li e entendi ~~ou não~~ sobre o assunto.

---

## O que são Funções de Escopo (Scope Functions)?

São funções da biblioteca padrão do Kotlin cujo o objetivo é executar um bloco de código dentro de um escopo/contexto de um objeto que podem ou não ter um valor de retorno. São cinco funções: `let`, `run` ,  `with` , `apply` e `also`.

Para começar vamos utilizar a classe `Product` como exemplo:

```kotlin
data class Product(
    var title: String,
    var price: Double,
    var isActive:Boolean
){
    fun deactivate(){
        isActive = false
    }
}

var product = Product("Smarthphone", 999.99, true)
```

Quando queremos mudar os atributo de `product` fazemos isso:

```kotlin
product.title = "Smarthpone Bonitono"
product.price = 1_000.00
product.deativate()
```

Com as funções de escopo também é possível mudar os atributos e deixar o código mais com o estilo do Kotlin.

## Let

Podemos utilizar o `let` para alterar os atributos de `product` e usamos o `it` para referenciar o `product`.

```kotlin
product.let {
    it.title = "Smarthpone Bonitono"
    it.price = 1_000.00
    it.deactivate()
}
```

É possível trocar o `it` para qualquer outra variável para referenciar o product:

```kotlin
product.let { att ->
    att.title = "Smarthpone Bonitono"
    att.price = 1_000.00  
    att.deactivate()      
}

print(product)
//  Product(title=Smarthpone Bonitono, price=1000.0, isActive=false)
```

Logo, os atributos de `product` foram modificados com `let`.

Agora vamos entender o valor de retorno do `let`:

```kotlin
var result = product.let {
    it.title = "Smarthpone Bonitono"
    it.price = 1_000.00
    it.deactivate()
}

print(result)
//  kotlin.Unit
```

Ao imprimir a variável `result` o retorno é um [kotlin.Unit](https://kotlinlang.org/docs/functions.html#unit-returning-functions) significa que o retorno é um valor não útil. Portanto, concluímos que o `let` não retorna um valor.

Podemos utilizar o `let` quando invocamos uma ou mais funções em resultados de chamadas encadeadas:

```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter { it > 3 }.let {
    println(it)
}
```

Se dentro do bloco de código do `let` tiver apenas uma função com o argumento `it`, podemos escrever o escopo da função `let` como:

```kotlin
numbers.map { it.length }.filter { it > 3 }.let(::println)
```

## Run

Também é possível utilizar o `run` para mudar os atributos de `product` e utilizamos o `this` para referenciar o escopo do objeto:

```kotlin
var product = Product("Smarthphone", 999.99, true)

product.run {
    this.title = "Smarthpone Bonitono"
    this.price = 700.00
    this.deactivate()
}
```

O uso do `this` é opcional, e também deixa o código redundante, portanto podemos escrever dessa forma:

```kotlin
product.run {
    title = "Smarthpone Bonitono"
    price = 700.00
    deactivate()
}
```

Vamos entender o retorno do `run`:

```kotlin
var result = product.run {
    title = "Smarthpone Bonitono"
    price = 700.00
    deactivate()
}

println(result)
//kotlin.Unit
```

Ao imprimir a variável `result` o retorno é um `[kotlin.Unit`,](https://kotlinlang.org/docs/functions.html#unit-returning-functions) significa que o retorno é um valor não útil. Portanto, concluímos que o `run` não retorna um valor.

Podemos utilizar o `run` para executar operações sobre um objeto e obter um resultado dentro de um escopo.

## With

Utilizamos o `with` para executar funções no objeto dentro de um contexto. Podemos ler como "*com esse objeto, faça*"

```kotlin
var product = Product("Smarthphone", 999.99, true)

with(product) {
    deactivate()
    println("produto desativado")
}
// produto desativado
```

O valor de retorno do `with` é um `kotlin.Unit`

```kotlin
var result = with(product) {
    deactivate()
    println("produto desativado")
}
println(result)

// produto desativado
// kotlin.Unit
```

## Apply

Podemos utilizar o `apply` para alterar os atributos de `product`, utilizamos o `this` para referenciar o escopo do objeto. Podemos ler "*aplique as seguintes atribuições ao objeto*"

```kotlin
var product = Product("Smarthphone", 999.99, true)

product.apply {
    this.title = "Smarthpone Bonitono"
    this.price = 1_000.00
    this.deactivate()
}
```

O uso do `this` é opcional e também deixa o código redundante, portanto podemos escrever dessa forma:

```kotlin
product.apply {
    title = "Smarthpone Bonitono"
    price = 1_000.00
    deactivate()
}
```

O valor de retorno do `apply` é o próprio objeto, podemos imprimir a variável `result` para entender isso:

```kotlin
var result = product.apply {
    title = "Smarthpone Bonitono"
    price = 1_000.00
    deactivate()
}
println(result)

// Product(title=Smarthpone Bonitono, price=1000.0, isActive=true)
```

## Also

Usamos o `also` para ações que utilizam o objeto do contexto como argumento ou quando precisamos de uma referência ao invés das propriedades e funções. Podemos ler como: "*e também faça com o objeto".*

Para referenciar o objeto dentro do escopo utilizamos o `it`

```kotlin
var product = Product("Smarthphone", 999.99, true)

product.apply {
    title = "Smarthpone Bonitono"
    price = 700.00
    deactivate()
}.also{ println("Promoção do Produto $it") }

// Promoção do Produto Product(title=Smarthpone Bonitono, price=700.0, isActive=false)
```

## Objeto nulos

Quando tivermos um objeto que possa ser nulo, as funções de escopo garantem que a função será chamada apenas se o objeto for não nulo:

```kotlin
var product2: Product? = null
// O ? permite que a variável seja nula

product2?.let {
  it.title = "tv"
  it.price = 100.00
  it.isActive = false
}

println(product2)
//  null
```

## Resumo

| Função | Retorno     | Variável dentro do escopo | Uso                                                         |
|:--------:|:-------------:|:---------------------------:|:-------------------------------------------------------------:|
| Let    | Unit/Lambda* | `it`                        | Funções com chamadas encadeadas e checagem de objetos nulos |
| Run    | Unit/Lambda* | `this`                      | Inicialização do objeto e o cálculo do valor de retorno     |
| With   | Unit/Lambda*        | `this`                      | Executar funções no objeto dentro de um contexto.           |
| Apply  | Objeto      | `this`                     | Inicializar e configurar um objeto                          |
| Also   | Objeto      | `it`                        | Ações adicionais que não alteram o objeto                   |

\* Na [própria documentação do Kotlin](https://kotlinlang.org/docs/scope-functions.html#return-value) diz que retorna o resultado lambda

## Link útil
Você pode testar o código usando o [Kotlin Playground](https://play.kotlinlang.org/), divirta-se :)


## Referências

- Youtube Playlist - [Kotlin Scope Functions Explained](https://www.youtube.com/watch?v=i-bWvj10k0k&list=PLMZ2RODGNLRK1D9kOfHLIvnhsdB8C7E4k)

- Documentação Kotlin - [Scope Functions](https://kotlinlang.org/docs/scope-functions.html)

- Documentação Kotlin - [Unit](https://kotlinlang.org/docs/functions.html#unit-returning-functions)

- Artigo Medium - [Kotlin Scoping Functions apply vs. with, let, also, and run](https://medium.com/@fatihcoskun/kotlin-scoping-functions-apply-vs-with-let-also-run-816e4efb75f5)

- Artigo Medium - [Kotlin: let, run, with, also ou apply?](https://medium.com/luizalabs/kotlin-let-run-with-also-ou-apply-24e8745f12fd)

- Artigo Medium - [Kotlin Scope Functions Made Simple](https://proandroiddev.com/kotlin-scope-functions-made-simple-c59b97a04ca2)




Espero que você tenha entendido sobre as funções de escopo do Kotlin!

Obrigada por ter lido <3

---
