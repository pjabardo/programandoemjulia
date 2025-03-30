+++
date = '2025-03-30T16:36:39-03:00'
draft = false
title = 'Caracteres unicode'
+++

No começo da era dos computadores, seu uso principal era calcular coisas. Mas logo se percebeu que o uso do computador poderia ser muito maior. Uma das necessidades era representar texto no computador. A solução encontrada foi atribuir um número a cada caracter. Naturalmente cada fabricante usou uma codificação diferente. Então resolveram padronizar isso e surgiu o [ASCII](https://pt.wikipedia.org/wiki/ASCII) que codificava os caracteres usando 7 bits. Infelizmente isso era suficiente para os caracteres usados em informática e os caracteres da língua inglesa.

Quando outros idiomas eram usados, o bit sobrando era usado para representar os caracteres adicionais. Quem não lembra de ver coisas como ISO-8859-1 ou ISO-LATIN-1? Isso funcionava bem para o português por exemplo mas não para o Chinês que precisava de um número muito maior de caracteres. Outro problema era quando mais de um idioma era usado no mesmo documento. Aí criou-se um negócio chamado [Unicode](https://pt.wikipedia.org/wiki/Unicode) que permite representar e manipular um monte de sistemas de escrita. 

No unicode, existem diferentes maneiras de representar textos. Na realidade este é um problema bem mais difícil do que parece, dada a riqueza de sistemas de escrita existentes no mundo. 

A codificação mais comum hoje em dia é o UTF-8 onde caracteres são representados por 1, 2, 3 ou 4 bytes. Julia usa este padrão tanto para o código fonte quanto para as strings. 
Em matemática é comum usar letras gregas e outros símbolos. O problema é como digitar isso diretamente. O terminal do Julia permite usar um conjunto razoável de caracteres unicodes. Julia usa algo parecido com LaTeX,  então para digitar a letra grega `β` no terminal (e editores preparados), você digita `\beta`  e pressiona TAB até completar. 

A lista de caracteres unicode configurados no terminal (REPL) Julia pode ser encontrado em <https://docs.julialang.org/en/v1/manual/unicode-input/>. Esta tabela fornece o caracter e como entrar com este caracter no terminal Julia.

Um problema que pode acontecer é que a fonte do ambiente em que se está trabalhando não tenha o caracter unicode correspondente a aí vai usar um símbolo padrão o que pode gerar confusão. A fonte JuliaMono <https://juliamono.netlify.app/> tem todos os caracteres unicode definidos na tabela do manual.

O screencast a seguir mostra como é usado os caracteres unicode no terminal Julia:

[![asciicast](https://asciinema.org/a/710699.svg)](https://asciinema.org/a/710699)

No exemplo do beta, digitei `\b` e então TAB. Aí apareceu uma lista grande de outros caracteres unicode disponíveis no terminal Julia. Digitando mais uma letra `\be` esta lista diminui consideravelmente. Geralmente a lista é pequena ou até mesmo inexistente. Também pode-se usar sufixos e superfixos (é este o nome???) mas logo logo você percebe que existem limitações: poucos caracteres estão disponíveis e não faz muito sentido: posso digitar `u\_x` + TAB para obter `uₓ` mas quando tento fazer `u\_z` + não consigo nada!

Não, isso não é o pessoal do Julia querendo ser chato. É que o unicode não tem estes caracteres. Para eles mesmo `uₓ` não deveria existir. Existe uma diferença entre caracter e como a coisa aparece. Para o pessoal do unicode, o unicode deveria se preocupar com os caracteres apenas e não na renderização como sufixo por exemplo. Para isso serve LaTeX e Word... Julia se limita aos caracteres fornecidos pelo unicode apenas. 

Esta maneira de se digitar os caracteres unicode é coisa do terminal Julia, não é um padrão nem nada... Os editores com modo Julia bons geralmente reconhecem isso. Visual Studio Code, Pluto, Jupyter, Emacs, Vi (argh...) e outros permitem usar isso. Mas é algo limitado ao mundo Julia. Então, se você está fazendo um programa de computador que outros vão usar, evite usar estes caracteres esquisitos. Eu diria até para evitar usar acentuação normal do português pois alguém nos EUA não vão conseguir digitar isso potencialmente.


Alguns caracteres unicode parecem bastante com outros então cuidado:

```julia
julia> c1 = 'B'  # Caracter B - 2a letra do alfabeto
'B': ASCII/Unicode U+0042 (category Lu: Letter, uppercase)

julia> c2 = 'Β'  # Caracter Beta maiúscula
'Β': Unicode U+0392 (category Lu: Letter, uppercase)

julia> c1*c2
"BΒ"

julia> c1*c2 == "BB"
false

julia> c1 == c2
false
```



