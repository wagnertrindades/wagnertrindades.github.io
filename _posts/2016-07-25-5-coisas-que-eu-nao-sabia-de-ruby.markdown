---
title:  "5 coisas que eu não sabia de Ruby"
date:   2016-07-25 21:44:44
categories: [ruby]
tags: [ruby]
---

### Introdução

##  Diferença entre :Symbol e "String"

### :Symbol

```ruby
a = :xunda
=> :xunda

b = :xunda
=> :xunda

a.object_id
=> 1154268

b.object_id
=> 1154268
```

Dois **Symbols** com os mesmos caracteres são um **único objeto**

### "String"

```ruby
a = "xunda"
=> "xunda"

b = "xunda"
=> "xunda"

a.object_id
=> 6990405680384

b.object_id
=> 6990405442200
```

Duas **Strings** mesmo tendo os mesmos caracteres são dois **objetos
diferentes**

### Garbage Collector

> Falar o que é Garbage Collector

> Explicar que o Symbol apesar de ser mais rápido e ocupar menos espaço em
memória não é coletado pelo garbage collector o que pode causar um
sistema mais lento caso tenha diversos Symbols em memória que ficam
alocados em memória "para sempre"

### Conclusão

> Use os saiba usa-los de acordo com cada caso

##  Cuidado com Floats

##  Diferenças entre Proc e Lambda

##  Constantes não são constantes

##  Precedência de operadores booleanos
