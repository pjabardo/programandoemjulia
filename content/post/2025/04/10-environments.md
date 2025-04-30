+++
date = '2025-04-30T13:08:47-03:00'
draft = false
title = 'Ambientes (environments) e como trabalhar com Julia na prática'
+++


Julia é vendida no varejo como uma linguagem onde você pode programar como Python 
(ou R ou Matlab) e ter desempenho de C++. 
Isso é verdade mas a história é mais complicada. 
Se você programar em Julia do jeito que você programa em Python, algumas coisas podem ser mais rápidas mas em geral o desempenho vai ser comparável. 
Para se conseguir o desempenho de C++, você, provavelmente, terá que fazer as coisas 
de um jeito diferente.

Mas existe um aspecto menos conhecido de programação em Julia que é a capacidade impressionante de reutilizar código de outros pacotes. Então quando você está fazendo algo que não seja trivial, você vai usar um monte de pacotes desenvolvidos por outras pessoas. Isto também acontece em outras linguagens mas normalmente os pacotes básicos são bem maiores e mais maduros (estão por aí faz tempo...). 

Então se começa a instalar tudo quanto é pacote, logo logo você vai perceber alguns problemas de compatibilidade de pacotes. Você tenta instalar um pacote mas ele é incompatível com a versão de outro que já está instalado. Aí você resolve este problema e outra coisa quebra, quando você consegue instalar o pacote. 

Este é um problema comum mas pelas características da linguagem Julia, este problema é um pouco exarcebada. O ecossistema Julia é mais novo e então muda mais e além disso pacotes menores e mais específicos têm um peso maior. 

### `virtualenv`: como Python trata este problema

Python resolveu este problema com o ambiente virtual (*virtual environment*). O que se faz é executar o comando `python -m venv /pasta/onde/você/quer/instalar/o/ambiente`. Este comando vai copiar arquivos básicos do Python para esta pasta e criar um ambiente personalizado para o teu projeto. Aí você instala os pacotes que você vai usar, com as versões específicas que você precisa. 

### A solução Julia

Em Julia temos algo parecido mas é parte do gerenciador de pacotes. Quando você entra no terminal do Julia, você está no ambiente padrão. Abrindo o terminal Julia normalmente, você tem algo assim:

```julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.11.5 (2025-04-14)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> import Pkg

julia> Pkg.status()
Status `/home/pjab/.julia/environments/v1.11/Project.toml`
  [6e4b80f9] BenchmarkTools v1.6.0
  [8f5d6c58] EzXML v1.2.1
  [7073ff75] IJulia v1.27.0
  [5fb14364] OhMyREPL v0.5.31
  [14b8a8f1] PkgTemplates v0.7.55
  [c3e4b0f8] Pluto v0.20.6
  [7f904dfe] PlutoUI v0.7.62
  [f27b6e38] Polynomials v4.0.19
  [438e738f] PyCall v1.96.4
  [d330b81b] PyPlot v2.11.6
  [295af30f] Revise v3.7.5

julia> 

```

A linha `import Pkg` carrega o gerenciador de pacotes. O comando `Pkg.status()`, naturalmente, retorna o estado atual do gerenciador de pacotes. A primeira linha tem o arquivo `Project.tom`. Este arquivo especifica os pacotes que fazem parte do ambiente. Cada pacote aqui foi instalado pelo gerenciador de pacote do Julia. Repare que além do nome do pacote, também vem a versão exata.

> No exemplo acima usei diratemente o gerenciador de pacotes. 
> Também é possível usar o modo gerenciador de pacotes do terminal. Para isso basta
> digitar `]` e você verá como o terminal muda:
>
```julia
(@v1.11) pkg> status
Status `/home/pjab/.julia/environments/v1.11/Project.toml`
  [6e4b80f9] BenchmarkTools v1.6.0
  [8f5d6c58] EzXML v1.2.1
  [7073ff75] IJulia v1.27.0
  [5fb14364] OhMyREPL v0.5.31
  [14b8a8f1] PkgTemplates v0.7.55
  [c3e4b0f8] Pluto v0.20.6
  [7f904dfe] PlutoUI v0.7.62
  [f27b6e38] Polynomials v4.0.19
  [438e738f] PyCall v1.96.4
  [d330b81b] PyPlot v2.11.6
  [295af30f] Revise v3.7.5

(@v1.11) pkg> 
```


Este arquivo `Project.toml` está na pasta de ambientes que está dentro da pasta de configuração Julia. No meu caso, em um sistema Linux esta pasta está em `/home/pjaba/.julia`. No windows esta pasta costuma ser `C:\Users\nome_do_usuário\.julia`. Na subpasta `environments

```sh
$ ls ~/.julia/environments 
makie  v1.10  v1.11  v1.12  v1.6
$
```

Cada versão de Julia tem um ambiente padrão dentro da pasta `environments`. Perceba que também tem uma pasta `makie` que é uma pasta com pacotes usados para plotar gráficos. Eu criei este ambiente e já veremos como fazer isso. Neste caso específico, estamos trabalhando com Julia versão 1.11. Na pasta `v1.11` temos:

```sh
$ ls ~/.julia/environments/v1.11 
LocalPreferences.toml  Manifest.toml  Project.toml
``` 

O arquivo `Project.toml` já foi explicado. O arquivo `Manifest.jl` especifica exatamente o que está no ambiente atual. Enquanto que `Project.toml` lista as dependências do teu ambiente, o `Manifest.jl` fornece as versões exatas do ambiente, pacotes e dependência. Com estes 2 arquivos, `Project.toml` e `Manifest.toml` você consegue reproduzir o ambiente exatamente, com as versões específicas dos pacotes usados. 

Se você tem que compartilhar um script ou programa, basta fornecer o programa/script e os arquivos `Project.toml` e `Manifest.toml`.

### Criando um ambiente

Até o momento estivemos usando o ambiente padrão Julia. Se você quiser criar um novo ambiente, 


```julia> import Pkg

julia> Pkg.status()  # Temos o pacote atual
Status `/opt/pjabardo/julia/environments/v1.11/Project.toml`
  [6e4b80f9] BenchmarkTools v1.6.0
  [8f5d6c58] EzXML v1.2.1
  [7073ff75] IJulia v1.27.0
  [5fb14364] OhMyREPL v0.5.31
  [14b8a8f1] PkgTemplates v0.7.55
  [c3e4b0f8] Pluto v0.20.6
  [7f904dfe] PlutoUI v0.7.62
  [f27b6e38] Polynomials v4.0.19
  [438e738f] PyCall v1.96.4
  [d330b81b] PyPlot v2.11.6
  [295af30f] Revise v3.7.5

julia> Pkg.activate(".") # Ativando um ambiente na pasta atual
  Activating new project at `~/temp/projeto`

julia> Pkg.status() # O ambiente está vazio
Status `~/temp/projeto/Project.toml` (empty project)

```

### Ambientes empilhados (*stacked environment*)

Um aspecto interessante é que o ambiente é empilhado. O que isso quer dizer? Repare que no ambiente padrão tem um pacote `EzXML`, que serve para trabalhar com arquivos `XML`. Dentro deste novo ambiente novo que criei, ainda posso usar este pacote:

```julia
julia> import EzXML
```

Carreguei o pacote `EzXML` sem problemas. Se eu listar os qruivos neste ambiente:
```julia
julia> readdir(".")
String[]
```

Nada! Pois ainda não fiz nada neste ambiente. Posso instalar um pacote novo:

```julia
julia> Pkg.add("FFTW")
    Updating registry at `~/.julia/registries/General.toml`
   Resolving package versions...
    Updating `~/temp/projeto/Project.toml`
  [7a1cc6ca] + FFTW v1.8.1
    Updating `~/temp/projeto/Manifest.toml`
  [621f4979] + AbstractFFTs v1.5.0
  [7a1cc6ca] + FFTW v1.8.1
  [692b3bcd] + JLLWrappers v1.7.0
  [21216c6a] + Preferences v1.4.3
  [189a3867] + Reexport v1.2.2
  [f5851436] + FFTW_jll v3.3.11+0
  [1d5cc7b8] + IntelOpenMP_jll v2025.0.4+0
  [856f044c] + MKL_jll v2025.0.1+1
  [1317d2d5] + oneTBB_jll v2022.0.0+0
  [0dad84c5] + ArgTools v1.1.2
  [56f22d72] + Artifacts v1.11.0
  [2a0f44e3] + Base64 v1.11.0
  [ade2ca70] + Dates v1.11.0
  [f43a241f] + Downloads v1.6.0
  [7b1f6079] + FileWatching v1.11.0
  [4af54fe1] + LazyArtifacts v1.11.0
  [b27032c2] + LibCURL v0.6.4
  [76f85450] + LibGit2 v1.11.0
  [8f399da3] + Libdl v1.11.0
  [37e2e46d] + LinearAlgebra v1.11.0
  [56ddb016] + Logging v1.11.0
  [d6f4376e] + Markdown v1.11.0
  [ca575930] + NetworkOptions v1.2.0
  [44cfe95a] + Pkg v1.11.0
  [de0858da] + Printf v1.11.0
  [9a3f8284] + Random v1.11.0
  [ea8e919c] + SHA v0.7.0
  [fa267f1f] + TOML v1.0.3
  [a4e569a6] + Tar v1.10.0
  [cf7118a7] + UUIDs v1.11.0
  [4ec0a83e] + Unicode v1.11.0
  [e66e0078] + CompilerSupportLibraries_jll v1.1.1+0
  [deac9b47] + LibCURL_jll v8.6.0+0
  [e37daf67] + LibGit2_jll v1.7.2+0
  [29816b5a] + LibSSH2_jll v1.11.0+1
  [c8ffd9c3] + MbedTLS_jll v2.28.6+0
  [14a3606d] + MozillaCACerts_jll v2023.12.12
  [4536629a] + OpenBLAS_jll v0.3.27+1
  [83775a58] + Zlib_jll v1.2.13+1
  [8e850b90] + libblastrampoline_jll v5.11.0+0
  [8e850ede] + nghttp2_jll v1.59.0+0
  [3f19e933] + p7zip_jll v17.4.0+2

julia> readdir(".")
2-element Vector{String}:
 "Manifest.toml"
 "Project.toml"
```
Além do pacote `FFTW`, o gerenciador de pacotes instalou todas as dependências. E criou os arquivos `Project.toml` e `Manifest.toml`. O arquivo `Project.toml` é curtinho:

```
[deps]
FFTW = "7a1cc6ca-52ef-59f5-83cd-3a7055c09341"
``` 

Só tem um pacote instalado né? Mas o `Manifest.toml` tem muito mais coisas. Aqui só vou mostrar parte do arquivo.

```
# This file is machine-generated - editing it directly is not advised

julia_version = "1.11.5"
manifest_format = "2.0"
project_hash = "f0a796fb78285c02ad123fec6e14c8bac09a2ccc"

[[deps.AbstractFFTs]]
deps = ["LinearAlgebra"]
git-tree-sha1 = "d92ad398961a3ed262d8bf04a1a2b8340f915fef"
uuid = "621f4979-c628-5d54-868e-fcf4e3e8185c"
version = "1.5.0"

    [deps.AbstractFFTs.extensions]
    AbstractFFTsChainRulesCoreExt = "ChainRulesCore"
    AbstractFFTsTestExt = "Test"

    [deps.AbstractFFTs.weakdeps]
    ChainRulesCore = "d360d2e6-b24c-11e9-a2a3-2a2ae2dbcce4"
    Test = "8dfed614-e22c-5e08-85e1-65c5234f0b40"


[[deps.FFTW]]
deps = ["AbstractFFTs", "FFTW_jll", "LinearAlgebra", "MKL_jll", "Preferences", "Reexport"]
git-tree-sha1 = "7de7c78d681078f027389e067864a8d53bd7c3c9"
uuid = "7a1cc6ca-52ef-59f5-83cd-3a7055c09341"
version = "1.8.1"

[[deps.FFTW_jll]]
deps = ["Artifacts", "JLLWrappers", "Libdl"]
git-tree-sha1 = "6d6219a004b8cf1e0b4dbe27a2860b8e04eba0be"
uuid = "f5851436-0d7a-5f13-b9de-f02708fd171a"
version = "3.3.11+0"

[[deps.FileWatching]]
uuid = "7b1f6079-737a-58dc-b8bc-7a2ca5c1b5ee"
version = "1.11.0"


[[deps.Pkg]]
deps = ["Artifacts", "Dates", "Downloads", "FileWatching", "LibGit2", "Libdl", "Logging", "Markdown", "Printf", "Random", "SHA", "TOML", "Tar", "UUIDs", "p7zip_jll"]
uuid = "44cfe95a-1eb2-52ea-b672-e2afdf69b78f"
version = "1.11.0"

    [deps.Pkg.extensions]
    REPLExt = "REPL"

    [deps.Pkg.weakdeps]
    REPL = "3fa0cd96-eef1-5676-8a61-b3b8758bbffb"

[[deps.Preferences]]
deps = ["TOML"]
git-tree-sha1 = "9306f6085165d270f7e3db02af26a400d580f5c6"
uuid = "21216c6a-2e73-6563-6e65-726566657250"
version = "1.4.3"

[[deps.Printf]]
deps = ["Unicode"]
uuid = "de0858da-6303-5e67-8744-51eddeeeb8d7"
version = "1.11.0"

```

Isso é apenas uma parcela do arquivo `Manifest.toml`. Este arquivo especifíca a versão exata do ambiente Julia e dos pacotes dependentes. É importante lembrar que apesar do gerenciador de pacotes instalar um monte de pacotes quando eu instalei o pacote `FFTW`, estes pacotes não são acessíveis por mim diretamente sem que eu explicitamente instale o pacote. Com a excessão dos pacotes que estavam instalados no ambiente anterior (o ambiente padrão). 

Enviando estes dois arquivos, pode-se reconstruir o ambiente exato.

### Ambientes compartilhados 

No exemplo anterior, criamos um ambiente dentro de uma pasta específica. Mas e se eu quiser ter um ambiente específico com um conjunto de pacotes que eu uso no meu dia a dia? Posso ter um ambiente com os pacotes usados para acessar a instrumentação do laboratório e um ambiente para acessar os programas de processamentos de dados dos ensaios por exemplo. Lembra da pasta `environments` que mostrei acima?

```sh
$ ls ~/.julia/environments 
makie  v1.10  v1.11  v1.12  v1.6
$
```

Esta pasta `makie` é um ambiente com vários pacotes que eu uso para processar dados em geral:

```sh
$ cat ~/.julia/environments/makie/Project.toml
[deps]
AbstractFFTs = "621f4979-c628-5d54-868e-fcf4e3e8185c"
Bonito = "824d6782-a2ef-11e9-3a09-e5662e0c26f8"
CSV = "336ed68f-0bac-5ca0-87d4-7b16caf5d00b"
CairoMakie = "13f3f980-e62b-5c42-98c6-ff1f3baf88f0"
CoolProp = "e084ae63-2819-5025-826e-f8e611a84251"
CurveFit = "5a033b19-8c74-5913-a970-47c3779ef25c"
DSP = "717857b8-e6f2-59f4-9121-6e50c889abd2"
DataFrames = "a93c6f00-e57d-5684-b7b6-d8193f3e46c0"
Dates = "ade2ca70-3891-5945-98fb-dc099432e06a"
DelimitedFiles = "8bb1440f-4735-579b-a4ab-409b98df4dab"
Downloads = "f43a241f-c20a-4ad4-852c-f6b1247861c6"
FFTW = "7a1cc6ca-52ef-59f5-83cd-3a7055c09341"
GLMakie = "e9467ef8-e4e7-5192-8a1a-b1aee30e663a"
HDF5 = "f67ccb44-e63f-5c2f-98bd-6dc0ccc4ba2f"
Interpolations = "a98d9a8b-a2ab-59e6-89dd-64a1c18fca59"
JLD2 = "033835bb-8acc-5ee8-8aae-3f567f8a3819"
LinearAlgebra = "37e2e46d-f89d-539d-b4ee-838fcccc9c8e"
LsqFit = "2fda8390-95c7-5789-9bda-21331edee243"
Makie = "ee78f7c6-11fb-53f2-987a-cfe4a2b5a57a"
Polynomials = "f27b6e38-b328-58d1-80ce-0feddd5e7a45"
Psychro = "9516f557-4a54-5a79-b954-c272e753c77a"
PyCall = "438e738f-606a-5dbb-bf0a-cddfbfd45ab0"
RCall = "6f49c342-dc21-5d91-9882-a32aef131414"
Random = "9a3f8284-a2c9-5f02-9a11-845980a1fd5c"
Serialization = "9e88b42a-f829-5b0c-bbe9-9e923198166b"
SparseArrays = "2f01184e-e22b-5df5-ae63-d93ebab69eaf"
SpecialFunctions = "276daf66-3868-5448-9aa4-cd146d93841b"
Statistics = "10745b16-79ce-11e8-11f9-7d13ad32a3b2"
WGLMakie = "276b4fcb-3e11-5398-bf8b-a0c2d153d008"
```

Para ativar este ambiente, basta colocar `@` antes do nome:
```julia
julia> import Pkg

julia> Pkg.activate("makie", shared=true)
  Activating project at `~/.julia/environments/makie`
```

Usando o modo gerenciador de pacotes, é mais simples:

```julia
julia> # Pressionando `]` para entrar no modo gerenciador de pacote

(@v1.11) pkg> activate @makie
  Activating project at `~/.julia/environments/makie`

(@makie) pkg> 
```

Repare no `@`!


Para criar ambientes compartilhados, basta
```julia
julia> import Pkg

julia> Pkg.activate("amb-compart-1", shared=true)
  Activating new project at `~/.julia/environments/amb-compart-1`
```

ou usando o modo gerenciador de pacotes:
```julia
(@v1.11) pkg> activate @amb-compart-2
  Activating new project at `~/.julia/environments/amb-compart-2`

(@amb-compart-2) pkg> 
```

As pastas (e os arquivos `Project.toml` e `Manifest.toml` só serão criados quando você adicionar pacotes ao ambiente.

Em outro *post* vou mostrar em detalhes como adicionar pacotes a um ambiente.


### Onde estão os pacotes instalados

Criamos ambientes, adicionamos pacotes mas,  no máximo, o que vemos são os arquivos `Project.toml` e `Manifest.toml`. Mas onde estão os pacotes propriamente ditos? Estão na pasta `.julia/packages/nome_do_pacote/hash_da_versão`. Cada pacote é instalado dentro de uma pasta com o nome do pacote. Cada versão instalada está em uma subpasta.


### Especificando o ambiente pela linha de comando

Quando você abre o terminal Julia você pode especificar o ambiente exato:

```
julia --activate=/pasta/onde/está/o/arquivo/Project.toml
```

Se você quiser criar um ambiente (ou usar) na pasta atual, basta
```
julia --activate=.
```

### Visual Studio Code

Conforme a figura abaixo, quando o terminal Julia estiver aberto, clique na região indicada em vermelho. Aí na parte de cima você pode selecionar o ambiente.

{{< figure src="../vscode-env1.svg" >}}





