+++
date = '2025-04-28T17:49:59-03:00'
draft = false
title = 'Solução dos exercícios do capítulo 8 - Strings'
+++


#### Exercício 8-1
Escreva uma função que recebe uma string como argumento e mostra as letras na
ordem contrária, uma por linha

```julia

function letrasreverso(s)
    i = lastindex(s)
    while i > 0
        println(s[i])
        i = prevind(s, i)
    end
end

```

```julia
julia> letrasreverso("paulo")
o
l
u
a
p

julia> letrasreverso("áéíóú")
ú
ó
í
é
á
```


#### Exercício 8-2
Realmente não entendi o que se quer aqui...

#### Exercício 8-3

`str[:]` é simplesmente fatiar a string do começo ao fim

```julia
julia> s = "Esta é uma string tosca"
"Esta é uma string tosca"

julia> s[1]
'E': ASCII/Unicode U+0045 (category Lu: Letter, uppercase)

julia> s[:]
"Esta é uma string tosca"

julia> s[begin:end]
"Esta é uma string tosca"
```

#### Exercício 8-4

```julia
function find(word, letter, index)
    
    while index <= sizeof(word)
        if word[index] == letter
            return index
        end
        index = nextind(word, index)
    end
    return -1
    
end
```

Uso:
```julia
julia> find("palavra", 'a', 1)
2

julia> find("palavra", 'v', 1)
5

julia> find("palavra", 'r', 1)
6

julia> find("palavra", 'z', 1)
-1
```

#### Exercício 8-5

```julia
function count(word, letter)

    counter = 0
    for c in word
        if c == letter
            counter += 1
        end
    end
    return counter
end
```
Exemplo:
```julia
julia> count("palavra", 'p')
1

julia> count("palavra", 'a')
3

julia> count("palavra", 'z')
0
```

#### Exercício 8-6

Programa original corrigido:
```julia
function isreverse(word1, word2)
    if length(word1) != length(word2)
        return false
    end
    i = firstindex(word1)
    j = lastindex(word2)
    while j > 0
        if word1[i] != word2[j]
            return false
        end
        j = prevind(word2, j)
        i = nextind(word1, i)
    end
    true
end
```

Dois problemas no programa original

 * Atualizava `j` antes de comparar
 * Condição `j >= 0` deveria ser `j > 0` com a correção anterior.

#### Exercício 8-7

Cada um tem que fazer isso...

#### Exercício 8-8

Para ativar o help, digite `?`:

```julia
help?> count
search: count count! codeunit conj const round cot ismount codeunits copyuntil

  count([f=identity,] itr; init=0) -> Integer

  Count the number of elements in itr for which the function f returns true.
  If f is omitted, count the number of true elements in itr (which should be
  a collection of boolean values). init optionally specifies the value to
  start counting from and therefore also determines the output type.

  │ Julia 1.6
  │
  │  init keyword was added in Julia 1.6.

  See also: any, sum.

  Examples
  ≡≡≡≡≡≡≡≡

  julia> count(i->(4<=i<=6), [2,3,4,5,6])
  3
  
  julia> count([true, false, true, true])
  3
  
  julia> count(>(3), 1:7, init=0x03)
  0x07

  ──────────────────────────────────────────────────────────────────────────

  count(
      pattern::Union{AbstractChar,AbstractString,AbstractPattern},
      string::AbstractString;
      overlap::Bool = false,
  )

  Return the number of matches for pattern in string. This is equivalent to
  calling length(findall(pattern, string)) but more efficient.

  If overlap=true, the matching sequences are allowed to overlap indices in
  the original string, otherwise they must be from disjoint character
  ranges.

  │ Julia 1.3
  │
  │  This method requires at least Julia 1.3.

  │ Julia 1.7
  │
  │  Using a character as the pattern requires at least Julia 1.7.

  Examples
  ≡≡≡≡≡≡≡≡

  julia> count('a', "JuliaLang")
  2
  
  julia> count(r"a(.)a", "cabacabac", overlap=true)
  3
  
  julia> count(r"a(.)a", "cabacabac")
  2

  ──────────────────────────────────────────────────────────────────────────

  count([f=identity,] A::AbstractArray; dims=:)

  Count the number of elements in A for which f returns true over the given
  dimensions.

  │ Julia 1.5
  │
  │  dims keyword was added in Julia 1.5.

  │ Julia 1.6
  │
  │  init keyword was added in Julia 1.6.

  Examples
  ≡≡≡≡≡≡≡≡

  julia> A = [1 2; 3 4]
  2×2 Matrix{Int64}:
   1  2
   3  4
  
  julia> count(<=(2), A, dims=1)
  1×2 Matrix{Int64}:
   1  1
  
  julia> count(<=(2), A, dims=2)
  2×1 Matrix{Int64}:
   2
   0
```

Esta função é bem genérica e tem várias versões. Alguns exemplos de uso:

```julia
julia> count('a', "banana")
3

julia> count(==('a'), "banana")
3
```


#### Exercício 8-9
```julia
julia> fruit = "banana"
"banana"
julia> fruit[1:2:6]
"bnn"
```

Usando indexação com passo de -1:

```julia
ispalindrome(s) = s == s[end:-1:1]
```
 ```julia
 julia> ispalindrome("aeiea")
true
```

**Cuidado!** Esta função só funciona com caracteres ASCII.

```julia
julia> ispalindrome("áeieá")
ERROR: StringIndexError: invalid index [2], valid nearby indices [1]=>'á', [3]=>'e'
Stacktrace:
 [1] string_index_err(s::String, i::Int64)
   @ Base ./strings/string.jl:12
 [2] getindex_continued(s::String, i::Int64, u::UInt32)
   @ Base ./strings/string.jl:472
 [3] getindex
   @ ./strings/string.jl:464 [inlined]
 [4] (::Base.var"#451#452"{String, StepRange{Int64, Int64}})(io::IOBuffer)
   @ Base ./strings/basic.jl:193
 [5] sprint(::Function; context::Nothing, sizehint::Int64)
   @ Base ./strings/io.jl:114
 [6] sprint
   @ ./strings/io.jl:107 [inlined]
 [7] getindex
   @ ./strings/basic.jl:192 [inlined]
 [8] ispalindrome(s::String)
   @ Main ./REPL[13]:1
 [9] top-level scope
   @ REPL[15]:1
``` 

Porque?



#### Exercício 8-10

```julia
function anylowercase1(s)
    for c in s
        if islowercase(c)
            return true
        else
            return false
        end
    end
end
```

Esta função vai retornar `true` se a primeira letra for minúscula, `false` se for maiúscula. Ainda existe um outro caso de um string vazio `""` e neste caso, a função retorna `nothing`

```julia
function anylowercase2(s)
    for c in s
        if islowercase('c')
            return "true"
        else
            return "false"
        end
    end
end
```

Esta função retorna `true` para todos os casos pois `c` é minúscula. A única exceção é quando a string é vazia `""`. Neste caso a função retorna `nothing`.

```julia

function anylowercase3(s)
    for c in s
        flag = islowercase(c)
    end
    flag
end
```
Esta função vai sempre resultar em erro caso não exista uma variável global `flag`. A variável `flag` dentro do laço `for` é uma variável local! Então `flag` só pode estar definida fora do corpo da função o que não é algo que a gente quer (neste caso) ou recomenda (em geral).


```julia
function anylowercase4(s)
    flag = false
    for c in s
        flag = flag || islowercase(c)
    end
    flag
end
```

Esta função parece estar correta. Mas vai percorrer toda a string mesmo que não precise. Achou uma letra minúscula, pode retornar verdadeiro.

```julia
function anylowercase5(s)
    for c in s
        if !islowercase(c)
            return false
        end
    end
    true
end
```

Esta função não funciona pois no momento que achar uma letra que não é minúscula, ela retorna `false` mesmo que existam letras minúsculas mais para frente.


#### Exercício 8.11

Vou fazer uma função `rotatechar` que faz a operação para uma letra. Depois implemento a função `rotateword` que aplica a função para todas as letras.

Algo que deve ser notado é que `n` pode ser qualquer número inteiro (positivo ou negativo, um número enorme, etc) e o programa deve considerar letras maiúsculas ou minúsculas. 

O programa aqui só funciona com letras ASCII ('a'-'z' e 'A'-'Z').


```julia
function rotatechar(c, n)
    
    if islowercase(c)
        ia0 = Int('a')
    elseif isuppercase(c)
        ia0 = Int('A')
    end

    idx = (Int(c) - ia0 + n) % 26
    if idx < 0
        idx += 26
    end
    

    return Char(ia0 + idx)
end


function rotateword(w,n)
    s = ""
    for c in w
        s = s * rotatechar(c,n)
    end
    return s
end
```

Exemplo:
```julia
julia> rotateword("cheer", 7)
"jolly"

julia> rotateword("CHEER", 7)
"JOLLY"

julia> rotateword("CHeer", 7)
"JOlly"

julia> rotateword("HAL", 1)
"IBM"

julia> rotateword("hal", 1)
"ibm"

julia> rotateword("Hal", 1)
"Ibm"

julia> rotateword("melon", -10)
"cubed"

julia> rotateword("Melon", -10)
"Cubed"
```


        
    
