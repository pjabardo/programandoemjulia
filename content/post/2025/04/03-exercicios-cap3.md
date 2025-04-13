+++
date = '2025-04-13T11:51:52-03:00'
draft = false
title = 'Exercícios do capítuo 3'
+++


#### Exercício 3-1

Programa original:
```julia
function printlyrics()
    println("I'm a lumberjack, and I'm okay.")
    println("I sleep all night and I work all day.")
end

function repeatlyrics()
    printlyrics()
    printlyrics()
end

repeatlyrics()
```

Mova a última linha do programa para o topo de modo que a chamada de função apareça antes das definições. Execute o programa e veja a mensagem de erro que obtemos.

O que acontece quando executamos o programa original:

```sh
 pjabardo@vazao80 > julia script.jl
I'm a lumberjack, and I'm okay.
I sleep all night and I work all day.
I'm a lumberjack, and I'm okay.
 I sleep all night and I work all day.
 ```

Movendo para o topo a última linha, temos o seguinte script:
```julia
repeatlyrics()

function printlyrics()
    println("I'm a lumberjack, and I'm okay.")
    println("I sleep all night and I work all day.")
end

function repeatlyrics()
    printlyrics()
    printlyrics()
end
```
Executando este programa temos o seguinte resultado:
```sh
pjabardo@vazao80 >  julia script.jl
ERROR: LoadError: UndefVarError: `repeatlyrics` not defined in `Main`
Stacktrace:
 [1] top-level scope
   @ ~/Documents/assipt/programandoemjulia/exercicios/script.jl:1
in expression starting at /home/pjabardo/Documents/assipt/programandoemjulia/exercicios/script.jl:1
```

Isso parece óbvio. Estamos chamando uma função antes que ela seja definida. Julia não sabe o que está acontecendo.

Agora vamos mover a chamada de função para o final e inverter a definição das funções `printlyrics` e `repeatlyrics`. A nova versão do programa é:
```julia
function repeatlyrics()
    printlyrics()
    printlyrics()
end

function printlyrics()
    println("I'm a lumberjack, and I'm okay.")
    println("I sleep all night and I work all day.")
end

repeatlyrics()
```

Ao executar este novo programa, temos o seguinte resultado:
```sh
pjabardo@vazao80 > julia script.jl
I'm a lumberjack, and I'm okay.
I sleep all night and I work all day.
I'm a lumberjack, and I'm okay.
I sleep all night and I work all day.
```

Funcionou! Inicialmente parece que tem algo errado. Na verdade isto mostra a natureza dinâmica da linguagem. Julia lê as funções mas apenas executa na última linha. E no momento da execução as funções são compiladas. Neste momento a função está definida. Contraste isso com um programa em C por exemplo. Neste caso, o arquivo inteiro é compilado antes da execução. Então você precisa declarar a função antes de usar.


#### Exercício 3-2
Escreva uma função chamada `rightjustify` que recebe uma sttring `s` como argumento e imprime uma string com espaços na frente de modo a útima letra da string `s` esteja na coluna 70 do terminal. Dica use a função `length` para saber o número de caracteres da string `s`. E lembre-se do operador `^` em strings.

```julia
julia> function rightjustify(s)
           n = length(s)
           espaço = " " ^ (70-n)
           return espaço * s
       end
rightjustify (generic function with 1 method)

julia> s2 = rightjustify("1234567890")
"                                                            1234567890"

julia> length(s2)
70
```

Mas acho que podemos melhorar isso um pouco? E se quisermos um valor diferente de 70? Podemos passar o comprimento como um argumento da função:

```julia
julia> function rightjustify(s, m)
           n = length(s)
           espaço = " " ^ (m-n)
           return espaço * s
       end
rightjustify (generic function with 2 methods)

julia> s2 = rightjustify("1234567890", 12)
"  1234567890"

julia> length(s2)
12
```

Funcionou. Mas não é exatamente o que o exercício pediu. A função deveria ter um argumento. Então vamos modificar esta função de modo que o comprimento total tenha um valor padrão:

```julia
julia> function rightjustify(s, m=70)
           n = length(s)
           espaço = " " ^ (m-n)
           return espaço * s
       end
rightjustify (generic function with 2 methods)

julia> s2 = rightjustify("1234567890")  # Se não dermos um valor, m é igual a 70!
"                                                            1234567890"

julia> length(s2)
70

julia> s3 = rightjustify("1234567890", 12)  # Se não dermos um valor, m é igual a 70!
"  1234567890"

julia> length(s3)
12

```


##### Exercício 3-3
Um objeto de função (*function object*) é um valor que você pode atribuir a um argumento. Por exemplo, `dotwice` (`fazerduasvezes`) é uma função que recebe um objecto de função a chama seu argumento duas vezes:

```julia
function dotwice(f)
    f()
    f()
end
```
Aqui temos um exemplo que usa `dotwice` para chamar a função `printspam` duas vezes:
```julia
function printspam()
    println("spam")
end

dotwice(printspam)
```

 1. Faça um script e teste

Criamos um arquivo com um nome qualquer, `script.jl` por exemplo e executamos o programa.

Arquivo `script.jl`:
```julia
function dotwice(f)
    f()
    f()
end

function printspam()
    println("spam")
end

dotwice(printspam)
```

Executando o programa:

```
pjabardo@vazao80 > julia script.jl
spam
spam
```

2. 3 e 4 Modifique a função `dotwice` de modo que a função receba dois argumentos, um objeto de função e um valor e chama a função duas vezes passando o valor como argumento:

```julia
julia> function dotwice(f, val)
           f(val)
           f(val)
       end
dotwice (generic function with 1 method)

julia> function printtwice(bruce)
           println(bruce)
           println(bruce)
       end
printtwice (generic function with 1 method)

julia> dotwice(printtwice, "spam")
spam
spam
spam
spam
```

5. 

```julia
julia> function dofour(f, val)
           dotwice(f, val)
           dotwice(f, val)
       end
dofour (generic function with 1 method)

julia> dofour(println, "spam")
spam
spam
spam
spam

```

#### Exercício 3-4
1. Escreva a função `printgrid` que desenha uma grade como mostrado abaixo:
```
julia> printgrid()
+ - - - - + - - - - +
|         |         |
|         |         |
|         |         |
|         |         |
+ - - - - + - - - - +
|         |         |
|         |         |
|         |         |
|         |         |
+ - - - - + - - - - +
```

A solução mais simples é escrever tudo sequencialmente. Na minha solução vou usar um outro recurso da linguagem: argumentos varíaveis. Repare nos ... abaixo:
```julia 
function dotwice(f, val...)
    f(val...)
    f(val...)
end
function printcell(sborda, sint, nint=4)
    print(sborda, sint^nint)
end


function printgrid()
    
    dotwice(printcell, "+", "-", 4); println("+")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "+", "-", 4); println("+")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "|", " ", 4); println("|")
    dotwice(printcell, "+", "-", 4); println("+")
    
end
    
```
Exemplo de uso:

```julia
julia> printgrid()
+----+----+
|    |    |
|    |    |
|    |    |
|    |    |
+----+----+
|    |    |
|    |    |
|    |    |
|    |    |
+----+----+
```

2. Escreva um função que desenha uma grade simular com 4 linhas e 4 colunas.

Vou seguir o que fiz antes mas vou usar as funções `dotwice` e `dofour` que fizemos antes. Estas soluções são chatas pois não estamos usando recursos mais vantajosos da linguagem como recursão ou iteração, algo que veremos mais adiante.

```julia 
function dotwice(f, val...)
    f(val...)
    f(val...)
end
function dofour(f, val...)
    dotwice(f, val...)
    dotwice(f, val...)
end

# Imprime os caracteres de uma parte de uma linha da célula (falta o último caracter
# que é o primeiro da célula seguinte à direita).
function printchars(sborda, sint)
    print(sborda, sint^4)
end

# Completa a linha inteira. `rfun` é a função de repetição e no final escrevemos o
# último caracter.
function printcharline(rfun, sb, si)
    rfun(printchars, sb, si)
    println(sb)
end

# Esta função desenha uma linha de células chamando as funções acima
function printrow(rfun)
    printcharline(rfun, "+", "-")
    printcharline(rfun, "|", " ")
    printcharline(rfun, "|", " ")
    printcharline(rfun, "|", " ")
    printcharline(rfun, "|", " ")
end

function printgrid()
    printrow(dofour)
    dofour(printrow, dofour)
    printcharline(dofour, "+", "-")
end
    
``` 

uso da função `printgrid`
```julia

julia> printgrid()
+----+----+----+----+
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
+----+----+----+----+
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
+----+----+----+----+
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
+----+----+----+----+
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
+----+----+----+----+
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
|    |    |    |    |
+----+----+----+----+

```
