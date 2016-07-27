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

### Float

```ruby
0.0004 - 0.0003 == 0.0001
=> false

0.0004 - 0.0003
=> 0.0010000000000000005
```
![ish](/images/posts/2016-07-25-5-coisas-que-eu-sabia-de-ruby/ish.jpg)

> Explicar o porque ocorre isso com o Float

### BigDecimal

```ruby
require 'bigdecimal'
=> true

num1 = BigDecimal.new("0.0004")
=> #<BigDecimal:7f6961d47328, '0.4E-3',9(18)>

num2 = BigDecimal.new("0.0003")
=> #<BigDecimal:7f6961d98237, '0.3E-3',9(18)>

result = num1 - num2
=> #<BigDecimal:7f6961d83vc3, '0.1E-3',9(18)>

result.to_s('F')
=> "0.0001"
```

> Explicar o porque o BigDecimal faz certo e o Float não

### Conclusão

> Pergunte ao cara certo...
> Aqui posso colocar o video do muleke que não sabia a conta de
> matemática e comparar com esse contexto falando que o Float não é o
> professor correto para fazer esse tipo de conta da mesma forma que o
> muleke perguntou sobre matematica para o professor de ciências

##  Diferenças entre Proc e Lambda

##  Constantes não são constantes

##  Precedência de operadores booleanos
