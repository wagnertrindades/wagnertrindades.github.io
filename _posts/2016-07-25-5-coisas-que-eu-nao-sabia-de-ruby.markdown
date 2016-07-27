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

### Primeira diferença

#### Proc

```ruby
show = proc { |x, y| puts "#{x}, #{y}" }
=> #<Proc:0x817487288372487@(irb):8>

show.call(1, 2)
1, 2
=> nil

show.call(1)
1,
=> nil

show.call(1, 2, 3)
1, 2
=> nil
```

> Não valida a quantidade de parametros


#### Lambda

```ruby
show = lambda { |x, y| puts "#{x}, #{y}" }
=> #<Proc:0x8174872882137@(irb):4 (lambda)>

show.call(1, 2)
1, 2
=> nil

show.call(1)
ArgumentError: wrong number of arguments (1 for 2)
        from (irb):4:in `block in irb_binding`
        from (irb):6:in `call`
        from (irb):6
        from /home/rd/.rbenv/versions/2.2.2/bin/irb:11:in `<main>`

show.call(1, 2, 3)
ArgumentError: wrong number of arguments (3 for 2)
        from (irb):4:in `block in irb_binding`
        from (irb):7:in `call`
        from (irb):7
        from /home/rd/.rbenv/versions/2.2.2/bin/irb:11:in `<main>`

```

> Valida a quantidade de parametros

### Segunda diferença

#### Proc

```ruby
def proc_stop
  puts "Cheguei..."
  proc { puts "Hey"; return; puts "Ho!" }.call
  puts "Saindo..."
end

proc_stop # Cheguei...; Hey
```

> O return dentro de um Proc faz o retorno do método associado

#### Lambda

```ruby
def lambda_stop
  puts "Cheguei..."
  lambda { puts "Hey"; return; puts "Ho!" }.call
  puts "Saindo..."
end

lambda_stop # Cheguei...; Hey; Saindo...
```

> O return dentro de um Lambda retorna apenas o contexto do Lambda

### Conclusão

> Use de acordo com a situação...


##  Constantes não são constantes

##  Precedência de operadores booleanos
##  Constantes não são constantes

##  Precedência de operadores booleanos
