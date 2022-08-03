# Introdução {#sec:intro}

[Julia](https://julialang.org) é uma linguagem relativamente nova que tem crescido ultimamente, principalmente em áreas relacionadas a computação científica. Nessa área, Julia compete com Matlab e outros ambientes parecidos. Mas Julia é uma linguagem genérica que pode ser utilizada em diferentes aplicações. Nesse sentido, Julia compete com Python (que, hoje, também compete com Matlab). Um dos aspectos mais celebrados de Julia é que consegue ter alto desempenho competindo assim com C/C++/Fortran.

Parafraseando o anuncio da linguagem Julia na web em 2012 (<https://julialang.org/blog/2012/02/why-we-created-julia/>), Julia propõe ser uma linguagem que combina a flexibilidade e dinamismo do Python, usando uma notação matemática como Matlab, permitindo usar macros como lisp, processando strings como Perl, tão potente para álgebra linear quanto Matlab, permitindo interfacear programas como Shell, sendo fácil de aprender mas não pegando nos nervos de programadores avançados e sendo interativo e compilado. Jé mencionei que a linguagem deve ser tão rápida quanto C (ou C++ ou Fortran)?

Passados 10 anos deste anuncio, é possível dizer que, em grande parte, estes objetivos foram alcançados.

## Por quê Julia? {#sec:porquejulia}

As características acima parecem boas demais para serem verdade. Mesmo se forem, escolher uma nova linguagem vai muito além das características positivas da linguagem. Vários outros aspectos são importantes, tanto para programadores experientes quanto para iniciantes. Aprender uma nova linguagem não é um investimento trivial e deve ser uma decisão bem pensada (a não ser que você se divirta aprendendo novas linguagens de programação...). Assim, algumas coisas devem ser levadas em consideração:

 * Julia apresenta caracterísiticas positivas? Se eu não menti, acho que combinar as melhores características de Matlab, Python, C++ me parece interessante.
 * Julia funciona no meu computador? Estou escrevendo isso no Linux, regularmente interfaceio com hardware em Windows e no Raspberry pi, muito do desenvolvimento é feito no Mac e dá para rodar no FreeBSD. [Visite e dê uma olhada ná página de download!](https://julialang.org/downloads/)
 * Quanto é que isso vai custar? Nada! Julia é software livre e o código está disponível no github <https://github.com/JuliaLang/julia>. O código usa licensa MIT, bem liberal - dá para fazer praticamente qualquer coisa, só precisa mencioanar que está usando código Julia (na dúvida, leia a licensa, <https://github.com/JuliaLang/julia/blob/master/LICENSE.md>).
 * Tem ferramentas para resolver o problema XYZ? O ecossistema tem crescido bastante. Não chega no nível do Python mas com a tua ajuda chegaremos lá! 😉. Na dúvida dê uma olhada <https://juliapackages.com/> que lista os pacotes *registrados*
 * Julia tem futuro? Novas versões com melhorias saem regularmente, a quantidade de pacotes tem crescido bastante e a velocidade deste cresciemento também aumentado. O uso de Julia na indústria também tem crescido (<https://juliacomputing.com/case-studies/>).
 * E tudo o que eu já fiz, tenho que começar do zero? Para pessoas que já programam faz algum tempo este é um problema sério. Interfacear com C/Fortran usando a biblioteca padrão é bem fácil e direto (<https://docs.julialang.org/en/v1/manual/calling-c-and-fortran-code/>). Se quiser algo mais automático, dê uma olhada em [CBinding](https://github.com/analytech-solutions/CBinding.jl) ou [Clang](https://github.com/JuliaInterop/Clang.jl). Existem pacotes para interfacear com [C++](https://github.com/JuliaInterop/CxxWrap.jl), [Python](https://github.com/JuliaPy/PyCall.jl), [R](https://github.com/JuliaInterop/RCall.jl) e outras linguagens (dê uma olhada em <https://github.com/JuliaInterop>).
 * E processamento paralelo? Julia permite programação assíncrona e paralela usando multi-threading, processamento distribuído e até GPU (Julia é particularmente bom nessa área).

Tá, você me convenceu. E documentação? A documentação oficial está localizada em <https://docs.julialang.org/>. O manual é uma mistura de guia de usuário e tutorial. Existem livros e tutoriais que pode podem ser acessados em <https://julialang.org/learning/>. Infelizmente a maior parte está em inglês e esta é a motivação para este livro.

E se eu precisar de ajuda, o que faço? Em inglês, existem vários canais: <https://julialang.org/community/>. Em particular, vale ressaltar o fórum, <https://discourse.julialang.org/>, que é bem movimentado e com excelente nível. Realmente recomendo que visite. Infelizmente em português, as opções são limitadas. Talvez alguém se proponha a fazer isso?


## Certamente Julia tem defeitos também, né?

Eu tentei vender uma imagem onde Julia parece ser algo revolucionário. Acho que não menti mas tenho que avisar que Julia também tem alguns defeitos. Na minha opinião, existem dois que são percebidos rapidamente.

### *First time to Plot*
O primeiro problema é conhecido na comunidade como *First time to plot*, tempo para o primeiro gráfico. Ou seja, quando o tempo para se gerar um gráfico *pela primeira vez é longo*. Depois de plotar o gráfico pela primeira vez, o desempenho é alto.

Na realidade isso ocorre com qualquer código que é executado pela primeira. Esse tempo excessivo ocorre pois na primeira vez que se executa esse código, o código precisa ser compilado antes de ser executado. E essa compilação leva algum tempo. Em algumas aplicações isso pode ser quase imperceptível. Mas em outras, como quando se plota um gráfico em um pacote todo escrito em Julia, cada chamada de função deve ser compilada. Vejamos como exemplo o seguinte código que calcula o seno de uma sequência numérica e plota o gráfico.

```julia-repl
julia> using GLMakie

julia> @time begin; x = 0:0.01:1; y = sin.(2π.*x); lines(x,y); end
 31.091122 seconds (82.86 M allocations: 4.430 GiB, 3.00% gc time, 95.87% compilation time)
```


31 segundos! Um horror! Mas se este mesmo código for executado novamente,

```julia-repla
julia> @time begin; x = 0:0.01:1; y = sin.(2π.*x); lines(x,y); end
  0.014505 seconds (74.12 k allocations: 4.199 MiB)
```

o código leva uma fração de segundo, como seria de se esperar em um computador moderno.

Isso tem melhorado e existem soluções para contornar isso, algumas das quais serão mostradas neste livro.


### Mensagens de erro

Se por um lado, o *first-time-to-plot* é sentido no primeiro dia, existe um outro problema que a longo prazo é um pouco mais chato: mensagens de erro de difícil interpretação. Aqui, novamente, a causa é a flexibilidade e reúso de código tão comum e incentivado em Julia. Novamente, isso tem melhorado e com experiência você aprende a interpretar as mensagens de erro.

### Outras dificuldades

Cada um tem sua reclamação. As duas mencionadas acima são as minhas. Outra mencionada com frequência na comunidade é a dificuldade de se descobrir como fazer algo.

Isso é resultado da documentação que ainda precisa melhorar mas também das características da linguagem que a torna tão atrativa: reúso de código extensivo em todo o ecossistema. Em Matlab, quando você abre o programa, está praticamente tudo disponível e com a documentação ali. Em Python, se você estiver trabalhando em alguma aplicação científica ou de ciência de dados, você carrega as bibliotecas numpy, scipy, matplotlib e pandas e tá tudo lá. Em Julia não existe o equivalente destas grandes bibliotecas. A mesma funcionalidade está distribuída em um número muito maior de pacotes. Aí fica mais difícil como fazer algo.

Alguém pode perguntar se isso é necessário ou a melhor alternativa. Pode parecer que não mas as coisas são mais complicadas. Em alguma aplicação específica, alguém pode criar um tipo de vetor com características específicas para esta aplicação. Em Matlab ou Python, seria mais complicado interfacear estas bibliotecas com a matplotlib por exemplo. Mas em Julia isso não só é possível (se o criador do pacote tiver trabalhado de maneira inteligente) como é estimulado.

À medida que livros, tutoriais e documentação específica para diferentes áreas aparecerem, este problema deve diminuir. Existem propostas para melhorar isso mas só o tempo dirá se isso será resolvido.

Outras pegadinhas da linguagem serão descritas ao longo deste livro.


## Instalando Julia {#sec:instalação}

O jeito mais fácil é simplesmente [baixar os binários para a sua plataforma](https://julialang.org/downloads/) e instalar. Você não precisa ser administrador para instalar e pode usar localmente. Até recomendo isso para iniciantes.

### Instalando no Windows

Existem algumas opções para a instalação de Julia no Windows. Existem versões de 32 bits e 64 bits. Em geral, o recomendado instalar a versão de 64 bits. O suporte é, inclusive melhor. É lógico que existem algumas circunstâncias onde você precisará usar uma versão de 32 bits - quem nunca teve que usar uma dll antigona? 32 bits só se você tiver uma necessidade especial.

Mesmo para Windows 64 bits (x86_64) existem duas versões: uma versão de instalação (*installer*) e outra portável (*portable*). A versão portável é muito útil se você quiser ter Julia em um *pendrive* e usar em qualquer computador. Já a versão de instalação, adiciona Julia ao menu iniciar e pode criar ícones na área de trabalho. Novamente, você pode instalar como usuário normal, o que é recomendado.

Durante a instalação, o instalador vai perguntar se você quer colocar Julia nna variável de ambiente  `PATH`. Em geral isso não é recomendado mas para os fins deste livro, é mais simples adicionar a pasta de executáveis Julia no `PATH`. 


**Importante** Se você estiver usando Windows 7, será necessário instalar algo chamado [Microsoft Management Framework](https://www.microsoft.com/en-us/download/details.aspx?id=54616). Sem isso talvez consiga instalar e fazer algo mas pode ter algumas limitações.

Ao executar Julia pela primeira vez, será criada uma pasta `.julia` no teu `HOME`. Nesta pasta estarão instalados os pacotes e suas dependências. Esta pasta pode crescer bastante. Em um sistema multi-usuário pode ser interessante compartilhar esta pasta entre diferentes usuários. Isso não será abordado neste livro.

### Instalando no Linux

Se você for mais aventureiro, pode compilar o código fonte mas dificilmente há motivo para isso. Algumas coisas em Julia são implementadas em C/C++ e muita gente acha que usar um compilador otimizado para o seu sistema permitirá obter mais um pouquinho de desempenho. Esqueça! O código Julia *que você escreve* é compilado usando [LLVM](https://llvm.org/) e usar um outro compilador não vai melhorar o desempenho. Talvez em alguma plataforma específica seja necessário compilar do zero (num cluster ou algo parecido) mas este é um livro introdutório né?

Use os binários disponíveis. Existem binários para processadores intel com arquitetura x86 (32 bits) e x86_64 (ou x64 ou AMD64, 64 bits). Também existem binários para ARM, tanto 64 bits quanto 32 bits: sim, é possível rodar Julia no Raspberry Pi!

Para linux x86_64, existe duas possibilidades: glibc e musl. Se você não sabe o que é isso ou qual a melhor opção, não há dúvida, escolha glibc!

Após descompactar os binários, você pode colocar em qualquer lugar. Basta adicionar a pasta `julia-1.x.y/bin` à variável de ambiente `PATH` (onde `x` e `y` é a versão específica que foi baixada). No meu micro, eu costumo copiar esta pasta para `/opt/` e faço um link simbólico em `/usr/bin/julia`:

```bash
sudo ln -s /opt/julia-1.7.3/bin/julia /usr/local/bin/julia
```

Se você quer manter tudo local, pode copiar a pasta para `~/.local/share` e fazer um link simbólico para `~/.local/bin`:

```bash
cp -r julia-1.7.3 ~/.local/share
ln -s ~/.local/share/julia-1.7.3/bin/julia ~/.local/bin/julia
```

### Instalando Julia no Mac

Não tenho um Mac, mas a internet é tua amiga.

### Terminais melhores

O terminal do Windows não é a oitava maravilha do mundo. E no Windows 7, o terminal é um horror. Neste caso, é interessante usar algo melhor. A solução mais simples é usar um terminal chamado de Mintty. Uma boa notícia é que o Git (programa de controle de versões) para windows já vem com o Mintty: <https://gitforwindows.org/>. Após instalar, procure por Git Bash. O bom desta opção é que se você for usar Julia mais a sério, você vai ter que usar o Git.

Outra possibilidade é usar o ConEmu <https://conemu.github.io/> que ainda permite usar mais de um terminal em abas.


### Fontes

Com a possibilidade de usar caracteres Unicode, é preciso ter uma fonte que tenha estes caracteres. [A família de fontes JuliaMono](https://github.com/cormullion/juliamono), criada por membros da comunidade Julia, é bem completa neste quesito.

## O REPL e Usando Julia como calculadora {#sec:repl}

O ambiente básico de programação Julia é o REPL (*R*ead-*E*val-*P*rint-*L*oop - laço de leitura, avaliação e impressão). Basicamente nesse ambiente você digita código e vê o resultado em tempo real. Este ambiente é extremamente útil ao desenvolver programas mas é muito bom para se usar como uma calculadora.

Executando Julia, um terminal será aberto. Neste ponto você pode começar a brincar com Julia. Você pode usar este ambiente como uma excelente calculadora:



## Editores e ambiente de desenvolvimento {#sec:editores}

O REPL descrito acima é importante e útil mas no final das contas é impossível (ou quase, não tenho dúvida de que alguém conseguiria me provar errado...) desenvolver um programa ou aplicativo sem usar um editor de texto ou ambiente de desenvolvimento. Quando eu falo editor de texto, **não** estou falando  de Word ou coisa parecida. O ambiente tem que permitir a edição de textos em formato ASCII ou UTF-8.


Vários editores reconhecem a sintaxe Julia e auxiliam na edição de arquivos Julia.

No windows o `notepad` pode ser usado não recomendo de modo algum. Se quiser algo bem simples que usa poucos recursos mais bastante útil com bastante funcionalidade, pode-se usar o [Notepad++](https://notepad-plus-plus.org/). Mesmo se você não for usar este programa para trabalhar com Julia, é sempre útil.

Naturalmente, [Emacs](https://github.com/JuliaEditorSupport/julia-emacs)  e [Vi](https://github.com/JuliaEditorSupport/julia-vim) têm suporte avançado para Julia.

Hoje, talvez o ambiente mais popular e com maior funcionalidade para Julia seja o [Visual Studio Code](https://www.julia-vscode.org/). O visual studio code se vende como uma alternativa moderna e fácil de usar de ambientes clássicos como Emacs e Vi já citados acima. 

Antes do desenvolvimento do editor atom <https://atom.io> parar, este era o ambiente de desenvolvimento Julia mais usado: <https://junolab.org>. Ainda funciona e é uma boa opção para quem já está familiarizado com o Atom. Para novos usuários, o Visual Studio Code é recomendado pois é aí que está havendo mais desenvolvimento.

Julia incentiva o uso de caracteres Unicode e no terminal (REPL) tem [comandos específicos para a entrada de vários caracteres Unicode que lembra LaTeX](https://docs.julialang.org/en/v1/manual/unicode-input/). Um editor que reconheça estes comandos facilita a vida. Visual Studio Code, Atom, Emacs, Vi, todos, se configurados corretamente, reconhecem estes comandos.

## Interfaces tipo Notebook {#sec:notebook}   

As interfaces tipo notebook são uma inovação recente em ambiente de programação. Elas procuram mesclar as melhores características dos ambientes interativos tipo REPL com os editores mencionados acima.

Dependendo da aplicação, os editores de texto terão que ser usados, principalmente quando o que se está desenvolvendo é um código. Mas é muito comum, principalmente em computação científica, o que se quer "vender" não é um código específico mas sim uma análise de dados ou algo parecido. Nesta situação, a abordagem tradicional era escrever alguns scripts (programas pequenos que são usados poucas vezes) que são executados a partir do REPL de maneira interativa gerando gráficos e tabelas que ajudam a analisar os resultados e escrever relatórios.

Neste tipo de aplicação, é muito comum se fazer o que eu chamaria de computação exploratória. Quando você começa, você não sabe exatamente o que vai fazer. Pode variar dependendo dos dados de entrada e resultado. Você calcula algo, faz um gráfico, volta atrás, muda algum parâmetro até chegar onde quer (quem já não ficou louco procurando o comando no histórico do REPL?).

Esta é a situação perfeita para o uso de Notebooks! Notebooks misturam código, documentação (principalmente matemática) e saída (gráficos, tabelas, etc) de maneira interativa. É uma interface realmente revolocionária para este tipo de aplicação.

Os Notebooks surgiram com o [Wolfram Mathematica](https://www.wolfram.com/mathematica/) mas realmente se tornaram popular com o [Jupyter](https://jupyter.org/). Inicialmente desenvolvido para ser uma interface notebook para python (se chamava IPython notebook originalmente), membros da comunidade Julia perceberam que este ambiente podia ser usado com [Julia](https://github.com/JuliaLang/IJulia.jl) e o nome mudou para refletir esta flexibilidade: Ju(lia) - Py(thon) e R. Hoje é possível usar os notebooks Jupyter com uma variedade imensa de linguagens (até [C++](https://xeus-cling.readthedocs.io/en/latest/) ou [Fortran](https://lfortran.org/)).


[Pluto](https://github.com/fonsp/Pluto.jl) é uma nova interface notebook para Julia, um pouco diferente do Jupyter. Tem algumas vantagens e desvantagens, dependendo do gosto, mas é certamente interessante. Veremos isso mais para frente.

O uso e operação destas interfaces notebook serão descritas em um capítulo posterios.

## Instalando Python {#sec:python}

*Python?* em um livro de Julia??? Python é uma linguagem legal e tem bastante coisa implementada que pode ser útil. Em particular, a interface notebook Jupyter é implementada em Python e deverá ser instalada para que possa ser usada com Julia.

No Linux, em geral Python já está instalado. Basta instalar o Jupyter (e depois o pacote `IJulia` - veremos isso mais adiante). Mas este não é o caso no Windows. A solução mais simples é instalar a [distribuição de Python Anaconda](https://www.anaconda.com/products/distribution) que já vem com o Jupyter instalado. O Anaconda é grande. Então pode ser interessante instalar o [miniconda](https://docs.conda.io/en/latest/miniconda.html) que é uma versão do Anaconda minimalista. Aí você instala o que você precisar diretamente. [O pacote Julia PyCall](https://github.com/JuliaPy/PyCall.jl) permite que Julia chame código Python diretamente. Em Windows, este pacote instala o miniconda. Se você quiser usar um Python já instalado no teu sistema, basta especificar o executável python na variável de ambiente `PYTHON`. Vale realçar que o pacote  `PyCall` só instalará o miniconda no Windows. No Linux e outras plataformas usará a versão instalado no sistema.


**Usuários do Windows 7:** As versões de Python 3.9 e 3.10 não rodam em Windows 7! Você vai precisar da versão 3.8 no máximo. Caso você esteja usando a versão instalada automaticamente com o pacote `PyCall`, você terá problemas: a instalação não reclama que o Windows 7 é incompatível com Python 3.9 (ou acima). Mas sabendo do problema, fica fácil solucionar.

## Estrutura do livro

O livro está dividido em 3 partes:

 1. Usando Julia, onde os fundamentos de programação em Julia são explorados
 2. Resolvendo problemas em Julia onde o ecossistema Julia é explorado, em particular a geração de gráficos.
 3. Julia avançado que mostra recursos mais sofisticados e como gerar pacotes.




