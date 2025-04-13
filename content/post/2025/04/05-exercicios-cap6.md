+++
date = '2025-04-13T14:36:04-03:00'
draft = false
title = 'Exercícios do capítulo 6'
+++


#### Exercício 6-1

```julia
function compare(x,y)
    if x > y
        1
    elseif x < y
        -1
    else
        0
    end
end
```

```julia
julia> compare(10,5)
1

julia> compare(10,-5)
1

julia> compare(-10,-5)
-1

julia> compare(-10,-10)
0
```

#### Exercício 6-2
Use desenvolvimento incremental para escrever a função hipotenusa que returna o comprimento a hipotenusa de um triângulo retângulo a partir do comprimento dos catetos.

```julia

# hipotenusa 2 entradas e uma saída
function hipotenusa(a,b)
    return 0.0
end

function hipotenusa(a,b)
    a² = a*a
    b² = b*b
    
    return 0.0
end


function hipotenusa(a,b)
    a² = a*a
    b² = b*b

    h² = a² + b²
    return h²
end


function hipotenusa(a,b)
    a² = a*a
    b² = b*b
    
    h² = a² + b²
    return sqrt(h²)
end


hipotenusa(a,b) = sqrt(a*a + b*b)

```
(observe que existe a função `hypot` na biblioteca padrão da Julia)

#### Exercício 6-3
Escreva uma função `isbetween(x,y,z) que retorna verdadeiro se \(x \le y \le z\) e falso caso contrário

```julia
function isbetween(x,y,z)
    if x <= y && y <= z
        true
    else
        false
    end
end

```

Mas também posso fazer isso:
```julia
isbetween(x,y,z) = x ≤ y ≤ z
```


#### Exercício 6-4
Fazer o diagrama de pilha (*stack diagram*) do seguinte programa

```julia
function b(z)
    prod = a(z, z)
    println(z, " ", prod)
    prod
end

function a(x, y)
    x = x + 1
    x * y
end

function c(x, y, z)
    total = x + y + z
    square = b(total)^2
    square
end

x = 1
y = x + 1
println(c(x, y+3, x+y))
```


|     |          |
| --- | -------  |
| Main | `x=1` |

|     |          |
| --- | -------  |
| Main | `x=1` |
|      | `y = x + 1` |

|     |          |
| --- | -------  |
| println | `c(x,y+3,x+y)` |

|     |          |
| --- | -------  |
| `c` | `x = 1` |
|     | `y = 5` |
|     | `z = 3` |

|     |          |
| --- | -------  |
| `c` | `total = 1 + 5 + 3` = 9|
|     | `b(total)^2` |

|     |          |
| --- | -------  |
| `b` | `z = 9`|


|     |          |
| --- | -------  |
| `b` | `prod = a(z, z)`|
| `b` | `println(z, " ", prod)`|

|     |          |
| --- | -------  |
| `a` | `x = 9`|
|    |  `y = 9` |
|    | `x = 10` |
|    | `return 10*9` |

|     |          |
| --- | -------  |
| `b` | `println(z, " ", prod)`|
|     | 9 90 |

|     |          |
| --- | -------  |
| `b` | `square = 90^2 `|


|     |          |
| --- | -------  |
| `Main` | `println(8100)`)



#### Exercício 6-5

Escreva uma função chamada `ack` que calcula a [função de Ackermann](https://en.wikipedia.org/wiki/Ackermann_function).


```julia
function ack(m,n)
    if m == 0
        n+1
    elseif m > 0 && n == 0
        ack(m-1, 1)
    elseif m > 0 && n > 0
        ack(m-1, ack(m,n-1))
    else
        error("m e n devem ser inteiros não negativos!")
    end
end
```

Repare que estou tratando dos erros! 

Caso `m` e `n` sejam grandes (maior que (3,4)) a coisa demora mesmo para calcular! Por exemplo para `ack(4,1) = 65533` depois de um tempinho. Mas `ack(4,2)` estoura a pilha depois de calcular durante um bom tempo.


#### Exercício 6-6
Um palíndromo é uma palavra que é soletrada igualmente de trás para frente. Recursivamente, uma palavra é um palíndromo caso a primeira e última letra sejam iguais e o meio seja um palíndromo. 

As funções a seguir recebem um argumento string e retornam a primeira, última e letras do meio:

```julia
function first(word)
    first = firstindex(word)
    word[first]
end

function last(word)
    last = lastindex(word)
    word[last]
end

function middle(word)
    first = firstindex(word)
    last = lastindex(word)
    word[nextind(word, first) : prevind(word, last)]
end
```

Teste estas funções.

```julia
julia> first("NOME")
'N': ASCII/Unicode U+004E (category Lu: Letter, uppercase)

julia> last("NOME")
'E': ASCII/Unicode U+0045 (category Lu: Letter, uppercase)

julia> middle("NOME")
"OM"

julia> middle("AB")
""
```

Escreva a função `ispalindrome`  que recebe uma string como argumento e returna `true` se a string é um palíndromo e false caso contrário.

```julia
function ispalindrome(s)
    n = length(s)

    if n <= 1
        return true
    end
    
    f = first(s)
    l = last(s)

    if f != l
        return false
    end

    m = middle(s)
    return ispalindrome(m)
end
```

#### Exercício 6-8
Um número \(a\) é uma potência de b se é divisível por b e \(\frac{a}{b}\) é uma potência de \(b\). Escreva uma função `ispower` que recebe os parâmetros `a` e `b` e retorna `true` caso `a` seja uma potência de `b`.

```julia

function ispower(a,b)

    if a % b == 0
        d = div(a,b)
        if d == 1
            true
        else
            ispower(d, b)
        end
    else
        false
    end
    
end
```


#### Exercício 6-8
O máximo divisor comum (MDC) de dois inteiros \(a\) e \(b\) é o maior inteiro que divide ambos com resto 0.

Uma maneira de calcular o MDC é baseado na observação de que se \(r\) é o resto de \(a\) divido por \(b\), então \(MDC(a,b)= MDC(b,r)\). Como caso base, temos \(MDC(a,0) = a\)



```julia
function mdc(a,b)

    if b == 0
        a
    else
        r = a % b
        mdc(b,r)
    end
end

```
