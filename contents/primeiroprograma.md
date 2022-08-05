# Meu Primeiro Programa em Julia {#sec:primeiro}

Neste capítulo vamos introduzir os conceitos básicos de programação em Julia.
Começaremos usando Julia como uma calculadora, introduziremos variáveis e seus tipos e vamos ver como se escreve *scripts* em Julia. Ao final faremos um *tour* da REPL.

## Julia como calculadora e o REPL {#sec:pr-calculadora}

Para usar Julia como calculadora, basta clicar no ícone na área de trabalho (se você escolheu essa opção na instalação) ou selecione Julia no menu. Para a turma mais *hardcore* (quem gosta de linha de comando e terminais), digite `julia` (se estiver no `PATH` se não estiver você vai ter que digitar o path completo). Você verá algo assim:


```
[pjabardo@durruti ~]$ julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.7.3 (2022-05-06)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> 

```

Pronto, você já pode usar Julia como uma calculadora:

```julia-repl
julia> 1+1
2

julia> sqrt(2)
1.4142135623730951

julia> sin(pi/4)
0.7071067811865475
```

Esta interface interativa em modo texto é conhecida como *REPL* - **R**ead-**E**val-**P**rint-**L**oop, laço de leitura, interpretação e impressão (se alguém tiver uma tradução menos horríve, aceito sugestões...). Isso é a descrição básica do que ocorre nesta interface modo texto:

 1. Ler um conjunto de caracteres (**R**ead)
 2. Interpretar estes caracteres (**E**val)
 3. Imprimir o resultado no terminal (**P**rint)
 4. Esperar por mais caracteres para continuar operando (**L**oop)

No exemplo acima podemos ver que operadores matemáticos são aceitos (`1+1`) assim como chamadas de funções (`sqrt(2)`). A maioria das funções matemáticas comuns e operadores estão disponíveis no REPL. Mas muitas coisas a mais estão disponíveis na [biblioteca padrão](https://docs.julialang.org/en/v1/) ou [pacotes externos](https://juliapackages.com/) (veremos mais sobre isso daqui a pouco). 

Os operadores matemáticos mais comuns podem ser vistos a seguir:
```julia-repl
julia> 2+3
5

julia> 3-2
1

julia> 2*3
6

julia> 4/2
2.0

julia> 2^3
8
```
Para tirar a potência de um número, use `^`.


## Diferentes tipos de números {#sec:pr-tipo}

No exeamplo anterior, repare na divisão. Existe algo diferente em relação aos outros exemplos.

```julia-repl
julia> 2*3
6

julia> 6/3
2.0
```

Esse exemplo mostra que números podem ter diferentes tipos. No exemplo acima, `2` é um número inteiro de 64 bits e `2.0` é um número de ponto flutuante de 64 bits. Para se certificar disso, pode-se usar a função `typeof` para ver o tipo de qualquer objeto Julia:


```julia-repl
julia> typeof(3)
Int64

julia> typeof(3.0)
Float64
```

`Int64` é um número inteiro com sinal de 64 bits. Já `Float64` é um número de ponto flutuante de 64 bits.

### Números inteiros

Pode-se intuir que existem outros tipos de números. Os números inteiros podem ter diferentes tamanhos, 8, 16, 32, 64, 128 bits ou tamanho arbitrário. També pode ter sinal.

 


