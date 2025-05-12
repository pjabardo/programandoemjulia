+++
date = '2025-05-12T18:16:00-03:00'
draft = false
title = 'Condução de calor em um termistor: elementos finitos'
+++


Eu fiz um pequeno notebook que usa o método de elementos finitos para simular a condição de calor no corpo de um termistor com cápsula de vidro. Pretendo melhorar isso mais para frente mas isso serve como exemplo.

### MF-58

O MF-58 é um termistor NTC bem comum e bem barato (na China...). Eu tenho usado ele como elemento sensor em termo-anemômetros a temperatura constante. 

Uma foto do MF-58 pode ser visto abaixo.
![Foto do MF-58](/post/2025/05/mf58.jpg)

O elemento termistor está no centro da cápsula de vidro (parte laranja) que é um mau condutor de calor. O termistor também perde calor pelas pernas que devem ser de alguma liga de aço elétrico.

### Notebook com simulação de elementos finitos

[Aqui você encontra um link para o notebook Pluto](/notebooks/2025/05/mf58cond.html)


### Pacotes usados

 * Geração de malha: [Gmsh.jl](https://gmsh.info/). `Gmsh` é um gerador de malhas 3d de código aberto. Possui um interface para Julia.
 * Elementos finitos: [Gridap.jl](https://github.com/gridap/Gridap.jl) é um pacote que permite usar o método de elementos finitos para resolver equações diferenciais parciais. O legal é que permite que você use uma notação parecida com a usada em matemática. É parecido com o [FreeFEM](https://freefem.org/) que tem uma linguagem própria para representar os problemas de elementos finitos. O programa [FEniCS](https://fenicsproject.org/) que permite formular o problema de elementos finitos em Python mas por trás está um código em C++. 
 * O projeto Gridap tem pacotes para ler malhas geradas pelo [Gmsh](https://gmsh.info/):  [GridapGmsh.jl](https://github.com/gridap/GridapGmsh.jl)
 * Para visualizar os resultados, eles podem ser exportados para formato VTK ou visualizados no [Makie](https://www.makie.org) usando o pacote [GridapMakie](https://github.com/gridap/GridapMakie.jl).
