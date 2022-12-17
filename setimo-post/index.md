# Uma Introdução a análise estatística de forma



**``Este trabalho foi apresentado no CNMAC e Conapesc 2022, e teve orientação da Prof. Dr. Getúlio José Amorim Do Amaral, gjaa@de.ufpe.br.``**

O objetivo deste artigo é dá uma introdução sobre análise estatística de
forma e posteriormente propor um método de classificação não supervisionado
($K$-médias) para dados de tamanho e forma considerando imagens
bidimensionais (formas planas).

## Introdução

Com os avanços da tecnologia, a captura de imagens bidimensionais e
tridimensionais tem se tornado cada vez mais comum no nosso cotidiano.
Essas imagens fornecem diversas informações para estudos estatísticos,
sendo essa área chamada de morfometria. A morfometria é uma das maneiras
de estudar estas imagens que se encontram bem consolidadas com diversas
aplicações, tais como: Medicina, Zoologia, Biologia e outros. Nesse
contexto, existem estudos que tratam da forma, e estudos que tratam o
tamanho e forma, dos objetos capturados nas imagens. No caso de forma,
os efeitos de locação, escala e rotação são removidos. No caso de
tamanho e forma, o efeito de escala não é removido.

No século atual, o amadurecimento da análise estatística de forma como
uma área teórica e aplicada vem crescendo, uma vez que no atual século a
maioria das tecnologias usam reconhecimento facial, ou seja,
propriedades geométricas de tamanho e forma. As aplicações da análise
estatística de forma se estendem por quase todas as áreas científicas e
tecnológicas aplicadas, das menores às maiores escalas.

A análise de de forma ou tamanho e forma dos objetos, pode ser útil para
a tomada de importantes decisões, como a de um médico que precisa
decidir se um câncer é maligno ou benigno, baseado em uma ressonância
magnética digitalizada.


## Desenvolvimento

### Análise Estatística de Forma

Formas de objetos estão disponíveis em todos os lugares, seja em uma
pesquisa na internet, uma ressonância magnética que você faz, ou até
mesmo no desbloqueio de um celular. Essas tomadas de decisões servem
para pesquisar, identificar, classificar e agrupar informações. A
análise estatística de formas é um tópico relativamente recente e está
relacionada com características e comparações de formas de objetos.
descrevem com mais detalhes os conceitos sobre análise de formas em seu
livro.

Uma maneira de descrever uma forma é indicar um subconjunto finito de
pontos no contorno do objeto. O número de pontos não seguem uma
restrição teórica segundo . Esse número finito de pontos de cada
objeto é conhecido como marcos.

Segundo existe três tipos marcos, são eles:

  - Marcos matemáticos: são pontos alocados em um objeto de acordo com
    propriedades matemáticas ou geométricas, por exemplo, pontos de
    máximo, mínimo, inflexão entre outros;

  - Marcos anatômicos: são pontos atribuídos por um especialista que
    correspondem a alguma característica biologicamente significativa;

  - Pseudo-Marcos: são pontos construídos em um objeto, localizados no
    contorno ou entre os marcos anatômicos e/ou matemáticos.

As Figuras mostram um exemplo da utilização desses marcos:

![](t1.png)

![](t2.png)

![](t3.png)

A partir da colocação dos marcos, e obtendo suas coordenadas, podemos
construir a matriz de configuração, e assim obter a forma do objeto.
Para isso, devemos realizar transformações matemáticas para remover
efeitos de escala, locação e rotação. Pois, formas são
definidas como toda informação geométrica que permanece quando os
efeitos de locação, escala e rotação são retirados de um objeto.

Para ver como é feita a retirada desses efeitos, veja minha [dissertação](https://repositorio.ufpe.br/bitstream/123456789/44679/1/DISSERTAÇÃO%20Jerfson%20Bruno%20do%20Nascimento%20Honório.pdf).

A partir da remoção desse efeito, é onde entramos nos três tipos de
configurações mais usados; são eles: Pré-Forma, Forma e Tamanho-e-forma.
A Figura abaixo representa em diagrama essas três configurações, e
como elas se relacionam entre sí:

![](D.png)

