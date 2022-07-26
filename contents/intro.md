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

**Importante** Se você estiver usando Windows 7, será necessário instalar algo chamado [Microsoft Management Framework](https://www.microsoft.com/en-us/download/details.aspx?id=54616). Sem isso talvez consiga instalar e fazer algo mas não poderá instalar pacotes (veremos como é muito importante) e várias coisas podem dar errado.

Ao executar Julia pela primeira vez, será criada uma pasta `.julia` no teu `HOME`. Nesta pasta estarão instalados os pacotes e suas dependências. Esta pasta pode crescer bastante. Em um sistema multi-usuário pode ser interessante compartilhar esta pasta entre diferentes usuários. Isso não será abordado neste livro.

## O REPL e Usando Julia como calculadora {#sec:repl}

O ambiente básico de programação Julia é o REPL (*R*ead-*E*val-*P*rint-*L*oop - laço de leitura, avaliação e impressão). Basicamente nesse ambiente você digita código e vê o resultado em tempo real. Este ambiente é extremamente útil ao desenvolver programas mas é muito bom para se usar como uma calculadora.

Executando Julia, um terminal será aberto. Neste ponto você pode começar a brincar com Julia. Você pode usar este ambiente como uma excelente calculadora:



## Editores e ambiente de desenvolvimento {#sec:editores}

O REPL descrito acima é importante e útil mas no final das contas é impossível (ou quase, não tenho dúvida de que alguém conseguiria me provar errado...) desenvolver um programa ou aplicativo sem usar um editor de texto ou ambiente de desenvolvimento. Quando eu falo editor de texto, **não** estou falando  de Word ou coisa parecida. O ambiente tem que permitir a edição de textos em formato ASCII ou UTF-8.

### Editores e IDE

Vários editores reconhecem a sintaxe Julia e auxiliam na edição de arquivos Julia.

No windows o `notepad` pode ser usado não recomendo de modo algum. Se quiser algo bem simples que usa poucos recursos mais bastante útil com bastante funcionalidade, pode-se usar o [Notepad++](https://notepad-plus-plus.org/). Mesmo se você não for usar este programa para trabalhar com Julia, é sempre útil.

Um outro editor multi-plataforma, leve e fácil de usar e bem útil é o <https://micro-editor.github.io/>. É um editor modo texto mas com bastante funcionalidade e pequeno.

Outro editor multi-plataforma leve que serve para editar arquivos Julia é o Geany: <https://www.geany.org/>. Versões recentes reconhecem Julia.

Naturalmente, [Emacs](https://github.com/JuliaEditorSupport/julia-emacs)  e [Vi](https://github.com/JuliaEditorSupport/julia-vim) têm suporte avançado para Julia.

Hoje, talvez o ambiente mais popular e com maior funcionalidade para Julia seja o [Visual Studio Code](https://www.julia-vscode.org/). O visual studio code se vende como uma alternativa moderna e fácil de usar de ambientes clássicos como Emacs e Vi já citados acima.

### Interfaces tipo *Notebook*

As interfaces tipo notebook são uma inovação recente em ambiente de programação. Elas procuram mesclar as melhores características dos ambientes interativos tipo REPL com os editores mencionados acima.

Dependendo da aplicação, os editores de texto terão que ser usados, principalmente quando o que se está desenvolvendo é um código. Mas é muito comum, principalmente em computação científica, o que se quer "vender" não é um código específico mas sim uma análise de dados ou algo parecido. Nesta situação, a abordagem tradicional era escrever alguns scripts (programas pequenos que são usados poucas vezes) que são executados a partir do REPL de maneira interativa gerando gráficos e tabelas que ajudam a analisar os resultados e escrever relatórios.

Neste tipo de aplicação, é muito comum se fazer o que eu chamaria de computação exploratória. Quando você começa, você não sabe exatamente o que vai fazer. Pode variar dependendo dos dados de entrada e resultado. Você calcula algo, faz um gráfico, volta atrás, muda algum parâmetro até chegar onde quer (quem já não ficou louco procurando o comando no histórico do REPL?).

Esta é a situação perfeita para o uso de Notebooks! Notebooks misturam código, documentação (principalmente matemática) e saída (gráficos, tabelas, etc) de maneira interativa. É uma interface realmente revolocionária para este tipo de aplicação.

Os Notebooks surgiram com o [Wolfram Mathematica](https://www.wolfram.com/mathematica/) mas realmente se tornaram popular com o [Jupyter](https://jupyter.org/). Inicialmente desenvolvido para ser uma interface notebook para python (se chamava IPython notebook originalmente), membros da comunidade Julia perceberam que este ambiente podia ser usado com [Julia](https://github.com/JuliaLang/IJulia.jl) e o nome mudou para refletir esta flexibilidade: Ju(lia) - Py(thon) e R. Hoje é possível usar os notebooks Jupyter com uma variedade imensa de linguagens (até [C++](https://xeus-cling.readthedocs.io/en/latest/) ou [Fortran](https://lfortran.org/)).


[Pluto](https://github.com/fonsp/Pluto.jl) é uma nova interface notebook para Julia, um pouco diferente do Jupyter. Tem algumas vantagens e desvantagens, dependendo do gosto, mas é certamente interessante.





