+++
date = '2025-04-13T13:28:12-03:00'
draft = false
title = 'Exercícios do capítulo 05'
+++


#### Exercício 5-1
Como exercício, desenhe o diagrama de pilha (*stack diagram*) da função `printn` chamado com `s = "Hello"` e `n = 2`.

```julia
function printn(s, n)
    if n ≤ 0
        return
    end
    println(s)
    printn(s, n-1)
end
```

|     |          |
| --- | -------  |
| printn | `s = "Hello"` |
|         | `n = 2` |

|     |          |
| --- | -------  |
| printn | `s = "Hello"` |
|         | `n = 1` |


|     |          |
| --- | -------  |
| printn | `s = "Hello"` |
|         | `n = 0` |


```julia
function do_n(f, n, args...)
    if n <= 0
        return
    end
    f(args...)
    do_n(f, n-1, args...)
end

```
Exemplo:
```julia
julia> do_n(println, 10, "Uma linha")
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
Uma linha
```


#### Exercício 5-2

A função Julia `time` retorna a hora atual no meridiano de Greenwich em segundos desde primeiro de janeiro de 1970.

Escreva um script que lê a hora atual e converte para horas, minutos, segundos e o númoer de dias desde a referência.

```julia

function hora_atual()
    t = time()

    # Número de dias inteiros desde a referância
    segpordia = 24 * 3600
    ndias = Int(div(t, segpordia))
    nsecsnodia = t % segpordia

    nhoras = Int(div(nsecsnodia, 3600))
    nmin = Int(div(nsecsnodia - nhoras*3600, 60))
    nsecs = nsecsnodia - nhoras*3600 - 60 * nmin

    return string(nhoras) * ":" * string(nmin) * ":" * string(nsecs) * ", " * string(ndias)* " dias"
end

```

Exemplo:

```julia
julia> hora_atual()
"16:57:58.126564025878906, 20191 dias"

julia> println(hora_atual()); sleep(10); println(hora_atual())
16:58:15.66285490989685, 20191 dias
16:58:25.68564200401306, 20191 dias
```

#### Exercício 5-3

O último teorema de Fermat diz que não existem inteiros positivos \(a\), \(b\) e \(c\) tal que 

\[ 
a^n + b^n = c^n
\]

para \(n > 2\).

1. Escreva a função `checkfermat` que recebe 4 parâmetros `a`, `b`, `c` e `n` e checa a validade do teormea de Fermat. 

```julia
function checkfermat(a,b,c,n)
    if n > 2 && (a^n + b^n == c^n)
        println("Caramba, Fermat estava errado")
    else
        println("Fermat continua certo...")
    end
end
```

2. Escreva uma função que lê do usuário valores de \(a\), \(b\), \(c\) e \(n\), converte para inteiros e usa a função `checkfermat` para ver se viola o teorema:

```julia
function ler_inteiro(prompt)
    print(prompt)
    return parse(Int, readline())
end

function checkfermat(a,b,c,n)
    if n > 2 && (a^n + b^n == c^n)
        return false
    else
        return true
    end
end

function testando_fermat()
    println("Testador do último teorema de Fermat")
    a = ler_inteiro("Entre com a: ")
    b = ler_inteiro("Entre com b: ")
    c = ler_inteiro("Entre com c: ")
    n = ler_inteiro("Entre com n: ")
    
    if !checkfermat(a,b,c,n)
        println("Caramba, você derrubou o último teorema de Fermat!")
    else
        println("O que você esperava? O último teorema de Fermat continua válido!")
    end
end
```



#### Exercício 5-4

1. 
```julia

function istriangle(l1, l2, l3)

    if l1 > l2 + l3
        println("Não")
        return false
    elseif l2 > l1 + l3
        println("Não")
        return false
    elseif l3 > l1 + l2
        println("Não")
        return false
    else
        println("Sim")
        return true
    end
end

``` 

2.
```julia

function istriangle(l1, l2, l3)

    if l1 > l2 + l3
        return false
    elseif l2 > l1 + l3
        return false
    elseif l3 > l1 + l2
        return false
    else
        return true
    end
end

function ler_num(prompt)
    print(prompt)
    return parse(Float64, readline())
end

function verificar_triângulo()
    
    println("Verificador de triângulos.\nEntre com os comprimentos das arestas:")
    l1 = ler_num("Comprimento da primeira aresta: ")
    l2 = ler_num("Comprimento da segunda aresta: ")
    l3 = ler_num("Comprimento da última aresta: ")
    
    if istriangle(l1, l2, l3)
        println("Dá para formar um triângulo com estas arestas!")
    else
        println("Não é possível formar um triângulo com estas arestas!")
    end
end

```


#### Exercício 5-5

Qual a saída do seguinte programa. Desenhe um diagrama de pilha (*stack diagram*) que mostra o estado do programa quando ele imprime o resultado:

```julia
function recurse(n, s)
    if n == 0
        println(s)
    else
        recurse(n-1, n+s)
    end
end

recurse(3, 0)
````

|     |          |
| --- | -------  |
| recurse | `n = 3` |
|         | `s = 0` |

|     |          |
| --- | -------  |
| recurse | `n = 2` |
|         | `s = 3` |

|     |          |
| --- | -------  |
| recurse | `n = 1` |
|         | `s = 5` |

|     |          |
| --- | -------  |
| recurse | `n = 0` |
|         | `s = 6` |

Imprime o valor 6:

```julia
julia> recurse(3,0)
6

julia> recurse(3,10)
16

julia> recurse(3,100)
106

julia> recurse(4,0)
10

julia> recurse(5,0)
15

```


#### Exercícios 5-6 e 5-7

Farei quando apresentar o turtle graphics. Não darei isto na aula mas farei um post sobre isso.



