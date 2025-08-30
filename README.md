# **Design Acessível: Encontrando o Contraste Perfeito para Modos Claro e Escuro**

## O dilema do modo escuro: como uma análise de dados revelou as cores que funcionam em qualquer lugar.

Imagine a cena: você, dev front-end ou web designer, escolhe a cor primária perfeita para um projeto. Um azul profundo, `(#0000FF, hsl(240,100,50), rgb(0, 0, 255)`. Fica incrível nos botões, links e gráficos sobre o fundo branco. Missão cumprida. 🎨

Até que chega a hora de implementar o modo escuro. E aquele azul profundo... desaparece. Contra o fundo preto, ele se torna quase ilegível, forçando você a criar uma variável de cor totalmente diferente só para o tema escuro.

Esse é um desafio clássico de design e acessibilidade. Uma mesma cor raramente possui um bom contraste tanto em fundos claros quanto escuros. Isso não afeta apenas a estética, mas também a legibilidade e a conformidade com as diretrizes da WCAG (Web Content Accessibility Guidelines).

> Nota: Essa legibilidade pode ser quantificada por uma métrica chamada **taxa de contraste**. A WCAG recomenda uma taxa mínima de `4.5:1` para texto normal. Uma implementação online dessa métrica pode ser encontrada [aqui](https://coolors.co/contrast-checker/).

Eu estava frustrado com um problema semelhante, mas bem menor. Afinal de contas, não trabalho com front end e nem sou designer. Sou um cientista/engenheiro de dados e comumente sinto necessidade de criar documentações visuais e tutoriais com muitas captura de telas customizados, para compartilhar processos, resultados e insights.

Meu problema pode parecer bobo, mas eu buscava o melhor contraste para as cores de setas, círculos e demais elementos visuais que uso para customizar captura de telas usando o software [Sharex](https://getsharex.com/). Um problema realmente bobo, mas que me fez refletir sobre a questão maior do design acessível.

Para resolver esse problema, decidi mergulhar em análise de dados para encontrar uma solução sistemática. A pergunta era simples: **qual é o tom ideal de cada cor essencial para maximizar a legibilidade em fundos pretos e brancos simultaneamente?**

### A Jornada em Busca do Equilíbrio 🕵️‍♂️

O objetivo era encontrar, para cada matiz (hue) no círculo cromático, a combinação perfeita de saturação e brilho que resultasse em um nível de contraste similar contra fundo braco e fundo preto.

Para isso, criei um script que iterou por milhares de variações de cores no espaço de cor **HSB (Hue, Saturation, Brightness)**, calculando para cada uma:
1.  O contraste contra o branco.
2.  O contraste contra o preto.
3.  A diferença absoluta entre os dois valores de contraste.

A primeira abordagem foi óbvia: para cada matiz, escolher a cor com a menor diferença de contraste possível. No entanto, os resultados foram instáveis. Um pequeno ganho de 0.01 na taxa de contraste levava a saltos enormes nos valores de brilho e saturação entre matizes vizinhos, criando uma paleta visualmente inconsistente.

### A Solução: Uma Janela de Tolerância Inteligente 💡

A superação da inconsistência veio de uma observação: Uma variação pouco significante na diferença de contraste poderia ser aceitável, sendo mais importante a estabilidade do brilho. Mas qual limiar de significância escolher? Decidi pela menor tolerância possível desde que fosse aplicada de forma consistente para todas as matizes. Esse valor de tolerância poderia ser encontrado pela busca da cor correspondente ao "pior cenário". A análise mostrou que o amarelo (hue 60) era a cor com a maior diferença inevitável de contraste, mesmo em seu ponto ótimo, com um valor de `0.06`.

Essa foi a chave! Usei esse valor como uma janela de tolerância. A nova estratégia foi:

1.  Para cada matiz, filtre todas as cores cuja diferença de contraste seja **menor ou igual a 0.06**.
2.  Dentro desse grupo de cores "aceitáveis", selecione aquela com o **maior brilho (brightness)**.

Essa abordagem suavizou drasticamente as variações de brilho, resultando em uma paleta de cores muito mais harmônica, sem sacrificar a legibilidade em nenhuma das matizes.

### A Paleta de Contraste Ideal 🏆

O resultado final é uma paleta com 36 tons cuidadosamente selecionados — um para cada 10 graus de matiz — ideais para anotações, highlights ou qualquer elemento de interface que precise se destacar em qualquer modo.

Aqui estão alguns exemplos:
* **Vermelho (hue 0):** rgb(96, 96, 252) `#ED0202 hsb(240, 62, 99) hsl(240, 97, 68) `
* **Verde (hue 120):** rgb(41, 135, 41) `#298729 hsb(120, 70, 53) hsl(120, 54, 34) `
* **Azul (hue 240):** rgb(96, 96, 252) `#6060FC hsb(240, 62, 99) hsl(240, 97, 68) `
* **Magenta (hue 300):** rgb(207, 0, 207) `#CF00CF hsb(300, 100, 81) hsl(300, 100, 40)`

Essas cores garantem um contraste mínimo de `4.5:1`, atendendo aos critérios da WCAG AA para texto normal, seja em fundo um slide de apresentação branco ou preto.

Este estudo mostra que, em vez de criar paletas duplicadas, podemos encontrar um único tom otimizado com um pouco de análise.

Acesse o notebook [contraste.ipynb](./contraste.ipynb) para acompanhar toda a jornada de descoberta das cores que funcionam em qualquer lugar. 😁

![](https://i.imgur.com/T5tSpgY.png)
