---
title:  "5 coisas que eu não sabia de Ruby"
date:   2016-07-25 21:44:44
categories: [ruby]
tags: [ruby]
---

### Introdução

##  Diferença entre :Symbol e "String"

A primeira coisa que me deixou bem curioso e que começei a pesquisar um
pouco mais sobre foi a questão de termos o **Symbol** e a **String** que
teoricamente são muito parecidos.

Então quando usar um ou outro?
Sempre utilizei mais Strings no geral
porém será que tem algum momento que seria melhor eu ter utilizado um
Symbol?
Realmente a uma diferença dos dois ou é somente a sintaxe?

Bom, vamos ver com os exemplos abaixo:

**:Symbol**

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

Como pode ver acima através do método `object_id` conseguimos saber
exatamente a posição em memória de cada objeto, no caso as
variáveis *a* e *b* que são os symbols com os mesmos caracteres.

Então concluimos que **Symbols** com os mesmos caracteres mesmo que em diferentes variáveis
ou qualquer outro lugar no código são um **único objeto**


**"String"**

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

Já com as strings o resultado não é o mesmo, vemos que os números de
onde foi alocado o espaço em memória para cada string são diferentes
logo não se trata do mesmo objeto.

Então duas **Strings** mesmo tendo os mesmos caracteres são dois **objetos
diferentes**

#### Legal, mas o que isso muda na minha vida? kkk

Bom sabendo sobre essa diferença conseguimos concluir que toda
vez que criar uma nova string embora seja os mesmos caracteres estará
instanciando um novo objeto em memória e ocupando mais um novo espaço em
memória.

E quando tiver trabalhando com symbols caso seja os mesmos caracteres
você esta economizando espaço em memória já que não instancia o objeto
novamente apenas utiliza o mesmo objeto já instanciado em memória
então não consome mais memória.

Então vou utilizar somente os maravilhos **Symbols** e ser feliz para
sempre???

> Gif Jim Carrey?

Humm, acho que não...

Vamos ver abaixo um pouco mais sobre essa questão de armazenamento em
memória

### Garbage Collector

https://www.infoq.com/br/presentations/entendendo-garbage-collector-ruby

> Falar o que é Garbage Collector

> Explicar que o Symbol apesar de ser mais rápido e ocupar menos espaço em
memória não é coletado pelo garbage collector o que pode causar um
sistema mais lento caso tenha diversos Symbols em memória que ficam
alocados em memória "para sempre"

### Conclusão

> Use os saiba usa-los de acordo com cada caso

##  Cuidado com Floats

Uma coisa que no próprio livro alertou e que até então num tinha visto
que acontecia esse tipo de coisa com o Float.

**Float**

```ruby
0.0004 - 0.0003 == 0.0001
=> false

0.0004 - 0.0003
=> 0.0010000000000000005
```
-------------------------------------------------------------------
![ish](/images/posts/2016-07-25-5-coisas-que-eu-sabia-de-ruby/ish.jpg)

WTF!?!?

-------------------------------------------------------------------

-------------------------------------------------------------------
Quando vi até lembrei desse
video que já ficou famoso a algum tempo atrás kkk

<iframe width="420" height="315" src="https://www.youtube.com/embed/0MR_Jw0QVQ4" frameborder="0" allowfullscreen></iframe>
-------------------------------------------------------------------

Isso ocorre porque a representação de números reais usada é o IEEE-754,
a forma padrão de representar números reais em binários, que não é
somente utilizada pelo Ruby, mas também por C, Java e Javascript. Porém,
este padrão possui um problema perigoso de arredondamento causando esse
tipo de resultado.

Muitas pessoas por desconhecerem esse problema podem usar floats para valores
financeiros, o que certamente vai ocosionar vários desses erros.
Para isso, usa-se a classe BigDecimal, do conjunto de bibliotecas padrão
do Ruby.

**BigDecimal**

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

Apesar de ser muito mais lenta que floats, eles são precisos e
se tratando de cálculos financeiros, performance na maioria dos casos
não é um problema, se comparado com aplicações científicas.

### Conclusão

Como visto no video o muleke errando todas as contas de matemática rsrs
E quando perguntam para ele...
Quem te ensinou?...
Meu professor de **ciências**...

Ai está o erro rsrs, sempre pergunte para o professor certo...

Se vai trabalhar com números com poucas casas decimais e não é
necessário tanta precisão com certeza é melhor utilizar os Floats mesmo
assim consegue ter mais performace e é mais comum a utilização. Mas caso
for trabalhar com cálculos financeiros ou caso precise de uma boa
precisão com números de várias casas decimais  com certeza teras que utlizar
BigDecimal.

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

Como podemos ver acima apesar de no `Proc` termos definido trabalhar com
2 parametros no caso *x* e *y*, podemos passar somente um parametro ou 3
parametros que ele irá imprimir o que conseguir sem nem reclamar rsrs.


Já com Lambda é um pouco diferente, vamos ver abaixo:

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

Fazendo o mesmo com um `Lambda` ele já reclama que não consegue executar
caso não passe exatamente os parametros que definiu em sua criação.

Dessa forma vemos que o Proc não valida a quantidade de parametros
passados pois permite passar inumeros parametros e ele
tenta executar com o que foi passado.Já o Lambda não permite isso, ele
valida se o número de parametros é exatamente o mesmo.

Bom essa é a primeira diferença entre eles, mas vamos a segunda:

### Segunda diferença

#### Proc

Bom quando temos um Proc dentro de um método e dentro do Proc há um
`return`, esse return para a execução do método associado ao Proc
fazendo com que a linhas após o `return` do Proc não sejam executadas.
Como pode ver no exemplo abaixo:

```ruby
def proc_stop
  puts "Cheguei..."
  proc { puts "Hey"; return; puts "Ho!" }.call
  puts "Saindo..."
end

proc_stop # Cheguei...; Hey
```

#### Lambda

Já com o Lambda mesmo tendo um `return` ele somente executa o return no
contexto do Lambda. Dessa forma as linhas de código do método após o
Lambda são executadas como pode ver nesse exemplo:

```ruby
def lambda_stop
  puts "Cheguei..."
  lambda { puts "Hey"; return; puts "Ho!" }.call
  puts "Saindo..."
end

lambda_stop # Cheguei...; Hey; Saindo...
```

### Conclusão

Agora que sabemos essas duas diferenças entre os dois podemos usar essas particuliaridades de
cada um de acordo com a necessidade do nosso código.

## Constantes não são constantes

Quando começei a ver sobre as constantes no Ruby que tinha a intenção de
realmente não ser alterada e me deparei com isto:

```ruby
CONSTANT = [:a, :b]
=> [:a, :b]

CONSTANT << :c
=> [:a, :b, :c]
```
Bom na verdade mesmo definindo como uma constante eu posso altera-la a
qualquer momento assim como uma váriavel, minha primeira reação quanto a
isso foi esta...

![ish](/images/posts/2016-07-25-5-coisas-que-eu-sabia-de-ruby/whatafuck.jpg)

Com um pouco de curiosidade para saber se de alguma forma eu poderia
fazer com o que a constante não pudesse ser alterada. Tentei fazer isso
dessa forma...

```ruby
CONSTANT = [:a, :b].freeze
=> [:a, :b]

CONSTANT << :c
RuntimeError: can't modify frozen Array
       from (irb):2
       from /home/rd/.rbenv/versions/2.2.2/bin/irb:11:in `<main>`

CONSTANT
=> [:a, :b]
```

E dessa forma com o método `freeze` eu não posso alterar nesse caso o
array colando mais valores nele e tudo mais...

Achei que tinha descoberto como realmente ter uma constante que nunca
poderá ser mudada rsrs

Mas ao fazer mais experimentos, consegui fazer isso...

Aqui sem o freeze:

```ruby
CONSTANT = "xunda"
=> "xunda"

CONSTANT = "blah"
(irb):2: warning: already initialized constant CONSTANT
(irb):1: warning: previous definition of CONSTANT was here
=> "blah"

CONSTANT
=> "blah"
```

E com o freeze:

```ruby
CONSTANT = "xunda".freeze
=> "xunda"

CONSTANT = "blah"
(irb):2: warning: already initialized constant CONSTANT
(irb):1: warning: previous definition of CONSTANT was here
=> "blah"

CONSTANT
=> "blah"
```

O que na verdade quando reiniciamos a constante ela até avisa que você
está redefinindo uma constante mas muda o valor da mesma forma com
freeze ou sem freeze nesse caso o comportamento é o mesmo.

## Conclusão

O Ruby não o proibe de modificar as constantes dependendo do tipo do
objeto ele te avisa que está modificando uma constante porém deixa livre
para muda-la.

Ele trata essa questão muito mais na parte conceitual mesmo do que até
na pratica.

> Eu consigo fazer com que uma constante não seja alterada porém não
> consigo fazer com que ela não seja reinicializada

! Pesquizar alguma formar de deixa-la inalterada porque o Jonatas falou
que tem um jeito sim

##  Precedência de operadores booleanos

Quando começei a ver Ruby e vi que tinha o **and**, **&&**, **or** e
**||** a principio pensei que o **and** e **&&** fossem iguais, assim
como o **or** e **||** também.

Porém vendo um pouco mais sobre Ruby descobri que muito pelo contrario,
são bem diferentes!

Vamos ver a seguir ;)

### and != &&

#### &&

```ruby
do_something = "123"
=> "123"

do_other_stuff = "abc"
=> "abc"

if var = do_something && do_other_stuff
  puts "var is #{var}"
end
var is abc
=> nil
```

No caso do **&&** o Ruby faz a comparação entre as duas variáveis dessa
forma:

`var = ( do_something && do_other_stuff )`

Por isso ele retorna o `var` como abc neste caso, por que ele verifica
as duas e retorna a última no caso da duas serem **true**.

#### and

```ruby
do_something = "123"
=> "123"

do_other_stuff = "abc"
=> "abc"

if var = do_something and do_other_stuff
  puts "var is #{var}"
end
var is 123
=> nil
```

Já com o **and** é bem diferente, como vemos acima ele retorna a
variavel como *123* porque faz esse tipo de execução:

`( var = do_something ) and do_other_stuff`

Então ele já inicia sua execução atribuindo o **do_something** ao `var`
desse forma ele já retornado porque os dois são diferentes.

E com o **or** e **||** acontece a mesma coisa, a execução segue o mesmo
padrão, veja abaixo...

#### ||

`var = ( do_something || do_other_stuff )`

#### or

`( var = do_something or do_other_stuff )`

Vendo isso a minha primeira conclusão foi em sempre utilizar o **&&** e
o **||** e esquecer que existe o **and** e **or** senão... kkk

<iframe src="//giphy.com/embed/sCKxksj4A8Tg4" width="480" height="310" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>


### Avdi Grimm

> Aqui começo explicando que começei a pesquisar se tinha algum caso bom
> para usar and e or e achei no blog do Avdi Grimm o post [Using "and"
> and "or" in Ruby](http://www.virtuouscode.com/2010/08/02/using-and-and-or-in-ruby/)
> Onde ele explica sobre a precedencia dos operadores booleanos e como
> usar da forma correta

> Explico sobre como funciona a precedencia e que veio do Perl e talz

### or

```ruby
class Post
  def have_posts?
    false
  end
end

post = Post.new
post.have_posts? or raise "Não tem posts!"
#=> RuntimeError: Não tem posts!

raise "Não tem posts!" unless post.have_posts?
#=> RuntimeError: Não tem posts!
```

### and

```ruby
class Post
  def initialize(name)
    @name = name
  end

  def publish!
    puts "Post #{@name} publicado!"
  end
end

post = Post.new("5 dicas de Ruby") and post.publish!
#=> Post 5 dicas de Ruby publicado!

post.publish! if post = Post.new("5 dicas de Ruby")
#=> Post 5 dicas de Ruby publicado!
```

**No rails**

```ruby
post = Post.find_by_name("5 dicas de Ruby") and post.publish!
#=> Post 5 dicas de Ruby publicado!
```

## Conclusão

> Use a ferramenta da maneira correta...

## Thanks brow
