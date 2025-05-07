+++
date = '2025-05-07T17:41:00-03:00'
draft = false
title = 'Raíz quadrada, Babilônia e derivadas'
+++

Calcular raízes quadradas é algo que se sabe faz muito tempo. Faz muito tempo mesmo. [Os babilônios tinham um algoritmo para calcular a raíz quadrada de um número](https://pt.wikipedia.org/wiki/Raiz_quadrada#M%C3%A9todo_babil%C3%B4nico). Hoje em dia este algoritmo pode ser obtido diretamente usando o [método de Newton-Raphson](https://pt.wikipedia.org/wiki/M%C3%A9todo_de_Newton%E2%80%93Raphson). 

### Aproximação da raíz quadrada usando o algoritmo da Babilônia

[O notebook Pluto mostra a implementação deste algoritmo](/notebooks/2025/05/babilonia.html). Mas este notebook mostra algumas outras coisas. Você pode baixar e rodar este notebook no teu computador. Mas este notebook tem mais algumas coisas interessantes...

### Cálculo de derivadas: diferenciação algorítmica ou automática

[Neste mesmo notebook](/notebooks/2025/05/babilonia.html) também implementa um método bem diferente para calcular a derivada de funções: [a diferenciação algorítmica, também conhecida como diferenciação automática](https://en.wikipedia.org/wiki/Automatic_differentiation). 

Comparamos este método de cálculo de derivadas com o método de diferenças centradas e a solução analítica. Neste notebook, é utilizada uma técnica de programação que pode ser novo para algumas pessoas: [funções de ordem superior](https://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_de_primeira_classe) onde funções podem ser passadas para outras funções como argumentos, e mais: uma função pode retornar uma nova função!



