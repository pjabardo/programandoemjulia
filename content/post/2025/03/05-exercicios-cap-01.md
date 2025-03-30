+++
date = '2025-03-30T13:47:44-03:00'
draft = false
title = 'Exercícios da aula 01 - capítulos 1 e 2'
+++

Na segunda aula vou fazer uma recapitulação.


Vamos começar pelo capítulo 1.

## Exercícios do capítulo 1
### Exercício 1-1

Este exercício consiste em brincar com a sintaxe básica de Julia. Eu fiz um screencast com estas brincadeiras. Não é um vídeo no senso comum da palavra. Eu usei uma ferramenta chamada [`asciinema`](https://asciinema.org) caso alguém tenha interesse.

[![asciicast](https://asciinema.org/a/710683.svg)](https://asciinema.org/a/710683)




### Exercício 1-2

1. Quantos segundos há em 42 minutos e 42 segundos?
julia> 42*60 + 42
2562


2. Quantas milhas há em 10 km?
```julia
julia> 10 #= km =# * 1_000 #= m/km =# * 1 #= pé =# / 0.3048 #= m =# * 1 #= milha =# / 5280 #= pés =#
6.2137119223733395
```
Repare `#= ... =#` representa um comentário que pode ser colocado do código. Caso usássemos apenas `#...` o que vem depois de `#` até o final da linha é um comentário. Com `#= ... =#` podemos fazer um comentário de várias linhas: não interessa o que vem no meio, até mesmo outra seção `#= ... =#`. Isso pode ser usado para isolar uma seção de código.

3. Se você corre 10 km em 37 minutos e 48 segundos, qual a passada média (tempo por milha em minutos e segundos)? Qual a sua velocidade média em milhas por hora?

Já sabemos que 10 km são 6,21 milhas. Mas para saber o tempo total em segundos, já fizemos algo parecido. Temos um tempo total de 2268 segundos ou 37,8 minutos. Assim temos uma passada de 365 s/milha ou 6,08 min/milha:

```julia

julia> (10 * 1000/0.3048 / 5280)  # 10 km em milhas
6.2137119223733395

julia> 37 * 60 + 48  # 37 minutos e 48 segundos em segundos
2268

julia> 37 + 48/60 # 37 minutos e 48 segundos em minutos
37.8

julia> (37*60 + 48) / (10 * 1000/0.3048 / 5280)  # segundos/milha
364.9992192

julia> (37 + 48/60) / (10 * 1000/0.3048 / 5280) # minutos/milha
6.0833203199999994

julia> (37 + 48/60) / 10  # Agora em unidades mais comuns no Brasil minutos/km
3.78

julia> (37*60 + 48) / 3600  # Tempo em horas
0.63

julia> # Velocidade média milhas/hora

julia>  (10 * 1000/0.3048 / 5280) / ( (37*60 + 48) / 3600 )  
9.863034797417999

```

Mas já fizemos usando o capítulo 2, podemos deixar isso mais legível:

```julia
julia> pés_por_milha = 5280
5280

julia> m_por_km = 1000
1000

julia> m_por_pé = 0.3048
0.3048

julia> s_por_min = 60
60

julia> s_por_h = 3600
3600
```

1. 
```julia
julia> 42*s_por_min + 42
2562
```

2. 
```julia
julia> 10 * m_por_km / m_por_pé / pés_por_milha
6.2137119223733395
```

3. 
```julia
julia> L_km = 10
10

julia> L_milha = L_km * m_por_km / m_por_pé / pés_por_milha
6.2137119223733395

julia> tempo_min = (37 + 48/s_por_min)
37.8

julia> passada = tempo_h * 60/ L_milha
6.0833203199999994

julia> tempo_h = (37*s_por_min + 48) / s_por_h
0.63

julia> vel_media = L_milha / tempo_h
9.863034797417999

```


## Exercícios do capítulo 2

### Exercício 2-1
Screencast:

[![asciicast](https://asciinema.org/a/710696.svg)](https://asciinema.org/a/710696)

### Exercício 2-2

Novamente fiz um screencast: 

[![asciicast](https://asciinema.org/a/710695.svg)](https://asciinema.org/a/710695)

### Exercício 2-3

1. O volume de uma esfera de raio \( V = \frac{4}{3} \pi r^3 \). Qual o volume de uma esfera de raio 5?

```julia
julia> r = 5
5

julia> V = 4/3 * π * r^3
523.5987755982989

julia> V = 4/3 * pi * r^3
523.5987755982989
```

Repare que podemos usar letras gregas em Julia. Na verdade podemos usar qualquer caracter unicode. Tá mas não tenho um teclado grego para digitar isso. No terminal Julia e em ambientes de programação preparados para Julia, muitos caracteres unicode podem ser usados com comandos parecidos com LaTeX. A lista de caracteres e como usá-los pode ser encontrado em <https://docs.julialang.org/en/v1/manual/unicode-input/>. No entanto cuidado! Se o ambiente não reconhecer estes caracteres, ferrou. Outro problema comum é a fonte que você está usando não ter uma representação de alguns destes caracteres. 

Vou fazer um screencast mostrando alguns destes caracterse unicodes.

2. Admitida que o preço de tabela de um livro é de R$ 24,95 mas a livraria tem um desconto de 40%. A remessa custa R$ 3,00 para a primeira cópia e R$ 0,75 para cada cópia adicional. Qual o custo para 60 cópias no atacado?

```julia
julia> L = 24.95
24.95

julia> preço = 24.95
24.95

julia> desconto = 0.4 * preço
9.98

julia> remessa_1a = 3.0
3.0

julia> remessa_adicional = 0.75
0.75

julia> N_cópias = 60
60

julia> custo_total = (L - desconto) * N_cópias + remessa_1a + (N_cópias-1)*remessa_adicional
945.4499999999999

julia> 

```


Então o custo total será de R$ 945,45

3. Se eu deixo minha casa às 6:52 e corro 1 milha a um passo razoável (8:15 por milha) e 3 milhas em um ritmo mais forte (7:12 por milha) e 1 milha no passo razoável, a que horas chego em casa para o café da manhã?

```julia
julia> t1 = 8*60 + 15
495

julia> t2 = 3 * (7*60 + 12)
1296

julia> t3 = 8*60 + 15
495

julia> t_total = t1 + t2 + t3
2286

julia> t_total ÷ 60
38

julia> t_total % 60
6

```

O tempo total de corrida é de 38 min e 6 segundos. Como saí às 6:52, chego de volta às 7:30:06.

Aqui usamos dois operadores que alguns de vocês podem não conhecer: 

 * Divisão inteira (÷) (caracter unicode \div+TAB) que pode ser escrito usando a função `div`:
 ```julia
 julia> 5 ÷ 2
2

julia> div(5,2)
2
```
* Operador resto (%) que retorna o resto de uma divisão.



