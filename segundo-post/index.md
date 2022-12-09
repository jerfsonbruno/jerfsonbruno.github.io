# Introdução a Visualização de dados com ggplot2


Objetivo
========

-   Mostrar um forma de pensar a construção de graficos a partir do
    gramática do ggplot;

-   Aplicar esta teoria usando o pacote ggplot2 e suas extensões
    implementados no software R.

-   Desenvolver grafícos diversos que fujam do tradicional.

-   Contrubuir para escrita de textos acadêmicos, em termos de melhoria
    de qualidade gráfica.

Gramática dos gráficos
======================

-   Gramática significa, em geral, regras para a arte e a ciência.

-   No contexto da analise gráfica, são regras para a construção de
    gráficos matemáticos e sua representação estética.

-   Wilson , Anand e Grossman (2005) escreveram um livro entitulado “The
    Grammar of Graphics”propondo uma gramática que pode ser usada para
    descrever e construir uma ampla gama de gráficos estatísticos a
    partir de múltiplas camadas de informações.

A gramática dos gráficos na prática é uma maneira de se pensar como
construir gráficos ficando claro cada passo que é realizado para
construção do mesmo. Desta teoria um gráfico pode ser construido nas
seguintes camadas:

-   Data: Os dados brutos.

-   Aesthestics: mapeamento de dados para propriedades estéticas (cor,
    forma, tamanho,etc.);

-   Geometries: forma de visualização (pontos, linhas, barras, etc.);

-   Facets: Uso de Múltiplos gráficos categorizados.

-   Statistics: Transformações estatísticas usadas (identidade, média,
    variância, suavizações, regressão linear, etc);

-   Coordinates: Mapeamento dos dados no sistema de coordenadas de um
    plano.

-   Theme: Temas pradronizados de cores, tamanhos, letras, etc.

![](ggplot_files/theme.png)<!-- -->

ggplot2 - O que? e porque usar?
===============================

-   ggplot2 é um pacote do software R baseado na gramática de gráficos
    escrita por Wilkinson(2005).

-   Possui um conjunto simples de princípios fundamentais e muito poucos
    casos especiais para construção de gráficos;

-   Ajuda a produção de gráficos de qualidade de publicação em poucas
    linhas de comandos;

-   Auxilia a concentração na criação de um gráfico que melhor revele as
    mensagens contidas em seus dados.

Conjuntos de dados
==================

Trabalharemos com dois conjuntos de dados:

-   lincoln\_weather: Dados metereologicos diarios da cidade de Lincoln,
    capital de Nebraska- EUA do ano de 2016.

-   rain: Mostra medidas categorizadas de precipitação da ilha vulcânica
    Alofi.

Praticando
==========

Chamando os Pacotes
-------------------

``` r
library(ggplot2)
library(ggridges) # onde está o conjunto lincoln_weather
```

Banco de Dados
--------------

``` r
names(lincoln_weather)
```

    ##  [1] "CST"                          "Max Temperature [F]"         
    ##  [3] "Mean Temperature [F]"         "Min Temperature [F]"         
    ##  [5] "Max Dew Point [F]"            "Mean Dew Point [F]"          
    ##  [7] "Min Dewpoint [F]"             "Max Humidity"                
    ##  [9] "Mean Humidity"                "Min Humidity"                
    ## [11] "Max Sea Level Pressure [In]"  "Mean Sea Level Pressure [In]"
    ## [13] "Min Sea Level Pressure [In]"  "Max Visibility [Miles]"      
    ## [15] "Mean Visibility [Miles]"      "Min Visibility [Miles]"      
    ## [17] "Max Wind Speed [MPH]"         "Mean Wind Speed[MPH]"        
    ## [19] "Max Gust Speed [MPH]"         "Precipitation [In]"          
    ## [21] "CloudCover"                   "Events"                      
    ## [23] "WindDir [Degrees]"            "Month"

Primeiro Gráfico simples - Scatterplot
--------------------------------------

``` r
tmemax = ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                     y = `Max Temperature [F]`))
tmemax+geom_point(colour = "red", size = 3,shape = 21, fill = "blue")
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-3-1.png)<!-- -->


Mapeando meses por Estações.
----------------------------

``` r
# Estações refentes ao hemisfério norte.
Est=c(rep("Inverno",60),rep("Primavera",(153-60)),
      rep("Verão",(241-153)),rep("Outono",(336-241)),
rep("Inverno",(366-336))) ;Estações=as.factor(Est)
tmemaxe=ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                    y = `Max Temperature [F]`,
                                    colour=Estações))

tmemaxe+geom_point()
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-4-1.png)<!-- -->

Acrescentando um modelo linear por estação.
-------------------------------------------

``` r
tmemaxe + geom_point() + geom_smooth(method = 'lm')
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-5-1.png)<!-- -->

Acrescentando um modelo linear geral + Regressão Quantílica
-----------------------------------------------------------

``` r
tmemax+geom_point(aes(color = Estações)) + geom_smooth(method = 'lm') +geom_quantile(color="red")
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-6-1.png)<!-- -->

Para uma variável Quantitativa - Opções básicas
-----------------------------------------------

Observando: - Histograma, Densidade, Dotplot e Poligono de Frequência
Aplicando a velocidade média em milha por hora.

``` r
library(gridExtra) # Para ver diversos gráficos.
tme=ggplot(lincoln_weather, aes(x = `Mean Wind Speed[MPH]`))
g1 <- tme+ geom_histogram()
g2 <- tme+ geom_density(color="darkblue", fill="lightblue")
g3 <- tme+ geom_dotplot(color="blue")
g4 <- tme+ geom_freqpoly(color="brown4")
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-7-1.png)<!-- -->

Para uma variável categórica - Opções básicas
---------------------------------------------

Observando: - Colunas, Barras, Coluna Única com proporções e Setores.
Aplicando preciptação de chuva na ilha Alofi.

``` r
library(markovchain) # onde esta os dados de chuva da ilha vulcância Alofi
data(rain)
r1=ggplot(rain, aes(x =rain)) # dados de contagem
r2=ggplot(rain, aes(x ="",fill=rain)) # dados proporção
g1 <- r1+ geom_bar()
g2 <- r1+ geom_bar(fill=c("darkturquoise","firebrick2","gold1"))+coord_flip()
g3 <- r2 + geom_bar(position="fill", width=1)
g4 <- r2 + geom_bar(position="fill", width=1) + coord_polar(theta="y")
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-8-1.png)<!-- -->

Para uma variável contínua em categorias - Opções básicas
---------------------------------------------------------

Observando: - Densidades, Dotplots, Box-plots e violino-plot. Média de
velocidade do vento por estação.

``` r
tmest=ggplot(lincoln_weather, aes(y=`Mean Wind Speed[MPH]`,x=Estações,fill=Estações))
tmest2=ggplot(lincoln_weather, aes(x=`Mean Wind Speed[MPH]`,fill=Estações))
g1 <- tmest2+ geom_density(alpha=0.55)
g2 <- tmest+ geom_dotplot(binaxis = "y",stackdir = "center")
g3 <- tmest+ geom_boxplot()
g4 <- tmest+ geom_violin()
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-9-1.png)<!-- -->

Trabalhando com Escalas - Limites e Tranformações.
--------------------------------------------------

Voltando ao primeiro gráfico. Limites e transformações implementadas.

``` r
gra1=tmemax+geom_point(colour = "red", size = 3, shape = 21, fill = "blue")
g1 <- gra1 + scale_x_continuous(limits = c(0, 75))
# limitado de 0 a 75 os valores de x
g2 <- gra1 + scale_y_continuous(limits = c(25, 100))
# limitado de 25 a 100 os valores de y
g3 <- gra1 + scale_y_log10()# Aplicando escala logaritma na base 10 em y
g4 <- gra1 + scale_x_sqrt()# Aplicando raiz quadrada em x
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-10-1.png)<!-- -->

Trabalhando com Escalas - Formato e tamanho
-------------------------------------------

Relação entre escalas formatos e tamanhos a dados discretos e contínuos.

``` r
tmemaxE=ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                    y = `Max Temperature [F]`,
                                    shape=Estações))
tmemaxS=ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                    y = `Max Temperature [F]`,
                                    size=`Min Temperature [F]`))

g1 <- tmemaxE+geom_point(aes(color=Estações))+scale_shape()
g2 <- tmemaxS+geom_point(aes(color=Estações)) +scale_size()

grid.arrange(g1, g2)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-11-1.png)<!-- -->

Coordenadas
-----------

Dados de chuva categorizados.

``` r
r1f=ggplot(rain, aes(x =rain,fill=rain)) # dados de contagem + fill
g1 <- r1f + geom_bar() +coord_cartesian(ylim=c(0,600)) # limitado y 0 a 500 Cord. Carteziana
g2 <- r1f +geom_bar() + coord_fixed(ratio = 1/150) # Razão entre escalas y/x
g3 <- r1f +geom_bar() + coord_flip() # girando (flip) as coordenadas
g4 <- r1f +geom_bar() + coord_polar(theta = "x", direction=1) # Coordenadas polares
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-12-1.png)<!-- -->

Faces - Saída dividia por categoria
-----------------------------------

Divisão da saída gráfica em função de outra variável.

``` r
rtemp=data.frame(temp_max=lincoln_weather$`Max Temperature [F]`,
temp_med=lincoln_weather$`Mean Temperature [F]`,Estações=Estações,Meses=lincoln_weather$`Month`)
t45=ifelse(rtemp$temp_med>=45," Maior ou igual à 45 ","Menor que 45") ; rtemp$t45=t45
tememax=ggplot(rtemp, aes(x = temp_max, y= temp_med,group=t45))
g1 <- tememax +geom_point(aes(color=Estações))+facet_grid(Estações ~ .) # Em linhas
g2 <- tememax +geom_point(aes(color=Estações))+facet_grid(.~Estações) # Em colunas
g3 <- tememax +geom_point(aes(color=Estações))+facet_grid(t45~ Estações) # linhas e colunas
g4 <- tememax +geom_point(aes(color=Estações))+facet_wrap(~Estações) # retângulo
grid.arrange(g1, g2, g3, g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-13-1.png)<!-- -->

Faces - usando pacote ggridges
------------------------------

Divisão da saída gráfica usando o pacote ggridges

``` r
library(ggridges)
ggplot( lincoln_weather, aes(x = `Mean Temperature [F]`, y = `Month`)) +
geom_density_ridges_gradient(aes(fill = ..x..), scale = 3, size = 0.3) +
scale_fill_gradientn(colours = c("#0D0887FF", "#CC4678FF", "#F0F921FF"),name = "Temp. [F]" )+
labs(title = 'Temperatura em Lincoln NE',x='Temperatura Média',y='Meses' )
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-14-1.png)<!-- -->

Temas personalizados
--------------------

Há diversos temas pré-definidos. Vejamos alguns

``` r
tmemaxe=ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                    y = `Max Temperature [F]`,
                                    color=Estações))
g1 <- tmemaxe+geom_point() + theme_bw()
g2 <- tmemaxe+geom_point() + theme_classic()
g3 <- tmemaxe+geom_point() + theme_dark()
g4 <- tmemaxe+geom_point() + theme_light()
grid.arrange(g1, g2, g3,g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-15-1.png)<!-- -->

Temas personalizados com o pacote ggthemes
------------------------------------------

``` r
library(ggthemes)
g1 <- tmemaxe+geom_point() + theme_fivethirtyeight()
g2 <- tmemaxe+geom_point() + theme_igray()
g3 <- tmemaxe+geom_point() + theme_wsj()
g4 <- tmemaxe+geom_point() + theme_stata()

grid.arrange(g1, g2, g3,g4)
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-16-1.png)<!-- -->

Novos gráficos com GGally
-------------------------

Matriz de correlação

``` r
library(GGally)
lincoln_weather$Est=Estações # atribuindo variável Est com estações aos dados
gpar<- ggpairs(lincoln_weather, mapping = aes(color = Est),
columns = 2:4,columnLabels = names(lincoln_weather)[2:4])
gpar
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-17-1.png)<!-- -->

Matriz de correlação acrescentando uma variável categórica

``` r
ggpairs(lincoln_weather, mapping = aes(color = Est), columns = c(2,3,4,25),
columnLabels = names(lincoln_weather)[c(2,3,4,25)])
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-18-1.png)<!-- -->

Alicando zoom usando ggforce
----------------------------

Voltando a primeiro gráfico aplicando zoom em Primavera

``` r
library(ggforce)
tmemaxe=ggplot(lincoln_weather, aes(x = `Mean Temperature [F]`,
                                    y = `Max Temperature [F]`,color=Est))
tmemaxe+geom_point()+facet_zoom(x = Est == "Primavera")
```
![](ggplot_files/figure-markdown_github/unnamed-chunk-19-1.png)<!-- -->

Considerações
=============

Conclusões com relação ao ggplot2:

-   Aplica de maneira prática e eficaz a gramática dos gráficos;

-   É Gratuito e tem comandos de fácil aplicação;

-   Há diversos gráficos básicos e outras extensões com tutuoriais
    disponíveis online;

-   São elegantes e atrativos ao olhar;

-   Existe uma comunidade bem inclinada a construção de novas extensões
    sob a mesma idéia da gramática dos gráficos;

-   Extraem informações úteis inclusive para exploração das técnicas
    existentes.

