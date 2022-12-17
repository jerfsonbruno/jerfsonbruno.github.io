# (Ox vs R): Otimização não linear da distribuição Gamma

`Esse trabalho foi apresentado na disciplina de Estatística computacional ministrada pelo Prof. Dr. Francisco Cribari Neto.`


A distribuição gama é útil na modelagem de variáveis aleatórias
contínuas que são maiores que zero. Ela é comumente usada em estudos de
sobrevivência. A distribuição gama também é útil na modelagem de dados
com alta variabilidade. Estimar apropriadamente os parâmetros da
distribuição gama é vital para o estudo desse e de outras distribuições.
Dependendo dos valores de seus parâmetros, a distribuição gama assume
várias formas, incluindo curvas em sino semelhante à distribuição
normal.

Este trabalho contém um estudo de simulação sobre os estimadores
de máxima verossimilhança, usando o método iterativo BFGS para estimação
dos parâmetros da distribuição gama.

A simulação será conduzida para determinar estimativas, intervalos de
confiança, viés relativo e tempo de execução do algoritmo nas linguagens
de programação `R` e `Ox`.

## Revisão da Literatura


Nesta seção será apresentada a teoria básica sobre simulações de Monte
Carlo, método de máxima verossimilhança e método iterativo BFGS. Após
isso, foi usado a distribuição gama para mostrar cada método passo a
passo.

### Monte Carlo

Suponha que se quer estimar $\theta$, o valor esperado de alguma
variável aleatória $X$:

$$\begin{aligned}
\theta = E(X).\end{aligned}$$

Suponha, além disso, que possam ser gerados valores de variáveis
aleatórias independentes com a mesma distribuição de probabilidade de
$X$. Cada vez que for gerado um novo valor, diz-se que uma simulação foi
concluída. Suponha que vão ser realizadas $n$ simulações, assim, serão
gerados $X_1,X_2,\dots,X_n$.

Se

$$\begin{aligned}
\bar{X} = \frac{1}{n} \sum_{i=1}^{n} X_i,\end{aligned}$$

for sua média, então pela lei forte dos grandes números, $\bar{X}$ será
usado como um estimador para $\theta$. Para o valor esperado e variância
tem-se

$$\begin{aligned}
E(\bar{X}) = \frac{1}{n} \sum_{i=1}^{n} E(X_i) = \theta,\end{aligned}$$

e

$$\begin{aligned}
Var(\bar{X}) = \frac{\sigma^2}{n}.\end{aligned}$$

Além disso, decorre do Teorema Central do Limite que, para $n$ grande,
$\bar{X}$ terá uma distribuição normal aproximada. Assim, se
$\sigma/\sqrt{n}$ é pequeno, então $\bar{X}$ tende a estar próximo de
$\theta$ e, quando $n$ for grande, $\bar{X}$ será um bom estimador para
$\theta$. Esta abordagem para estimar um valor esperado é conhecida como
a simulação de Monte Carlo.

### O Método de Máxima Verossimilhança

Segundo, os estimadores de Máxima Verossimilhança desfrutam de
propriedades desejáveis e podem ser usados na construção de intervalos
de confiança e testes de hipóteses.

A ideia da estimação por máxima verossimilhança é de encontrar os
parâmetros que maximizam a probabilidade de que um certo conjunto de
dados sejam gerados por algum processo específico. Agora, imagine que
temos uma amostra aleatória, que não sabemos qual distribuição os dados
seguem, mas vamos supor que eles seguem alguma distribuição específica,
por exemplo, uma Gama. O formato da Gama depende dos parâmetros
$(\alpha,\beta)$, então, a ideia é de encontrar os valores de
$(\alpha,\beta)$ que melhor se aproximam da distribuição
empírica/observada dos dados.

Formalmente, suponhamos $x_1,x_2,...,x_n$ uma amostra aleatória de
tamanho $n$ caracterizada pela função de probabilidade ou densidade
$f(x)$, a função de verossimilhança para o parâmetro $\theta$ é dada
por:

$$\begin{align}
L(\theta|x_i) = \prod_{i=1}^{n} f(x_i|\theta).\end{align}$$

Então para determinar a melhor distribuição para a amostra, é preciso
determinar o valor de $\theta$ que maximiza $L(\theta)$. Porém,
maximizar a $L(\theta)$ muitas vezes não é uma tarefa fácil. Dessa
forma, foi criado uma método chamado de log-verossimilhança, onde
maximizar o $l(\theta|x_i)=\log(L(\theta|x_i))$ é o mesmo que maximizar
o $L(\theta|x_i)$.

#### Propriedades dos Estimadores de Máxima Verossimilhança

A teoria dos estimadores de máxima verossimilhança nos garante que eles
são consistentes (i.e. que eles aproximam o valor verdadeiro dos
parâmetros) e normalmente assintóticos (a distribuição assintótica segue
uma distribuição normal) desde que algumas condições de regularidade
sejam atendidas.

A consistência dos estimadores $\hat{\theta}_{MV}$ significa que eles se
aproximam dos valores verdadeiros do parâmetros $\theta_0$ à medida que
aumenta o tamanho da amostra. Isto é, se tivermos uma amostra grande
então podemos ter confiança de que nossos estimadores estão muito
próximos dos valores verdadeiros dos parâmetros.

\begin{align} \hat{\theta}_{MV} \rightarrow \theta_0 \end{align}

Dizemos que os estimadores de máxima verossimilhança são normalmente
assintóticos porque a sua distribuição assintótica segue uma normal
padrão.

Portanto, ao utilizarmos o método de Máxima Verossimilhança para estimar
os parâmetros de alguma distribuição, na verdade estamos calculando
estimadores pontuais. Por sua vez, também e possível obter intervalos de
confiança para os parâmetros estimados. Para tanto, faz-se necessário o
uso de propriedades adequadas para grandes amostras dos estimadores.
Frente a isso, e considerando a normalidade assintótica dos estimadores
de Máxima verossimilhança, obtém-se os intervalos de confiança para os
parâmetros da distribuição, ou seja:

\begin{align}
IC(\theta,(1-\alpha)) = \left( \hat{\theta} - z_{\alpha/2}\sqrt{Var(\hat{\theta})} ; \hat{\theta} + z_{\alpha/2}\sqrt{Var(\hat{\theta})}\right)\end{align}


em que $(1-\alpha)\%$ representa a porcentagem de confiança e
$z_{\alpha/2}$ representa o quantil da distribuição normal.

### Distribuição Gama

Se $X$ é uma variável aleatória com distribuição Gama e parâmetros
$\alpha > 0$ e $\beta > 0$, denotando-se
$X \sim \ \text{Gama}(\alpha,\beta)$, sua função densidade é dada por

$$f(x)= \dfrac{x^{\alpha - 1}e^{-\beta x}\beta^\alpha}{\Gamma(\alpha)} \hspace{0.5cm} \text{quando} \hspace{0.5cm}  x \geq 0$$


A Figura abaixo, representa a distribuição gama com alguns parâmetros. A mesma mostra que
os parâmetros $\alpha$ e $\beta$ servem como parâmetros de escala e
forma respectivamente.

![](/gamma.png)

A variável Gama foi criada como uma extensão da exponencial para
descrever o tempo de espera até que um certo número de eventos ocorram,
dada uma taxa constante de ocorrência. Devido à sua flexibilidade,
posteriormente foi adotada como modelo heurístico para descrever
variáveis com distribuições de probabilidade assimétricas.

#### Verossimilhança e log-Verossimilhança para a Distribuição Gama

A função de verossimilhança para a distribuição gama é dada por:

$$L(\alpha,\beta|x_i)=\prod^n_{i=1}f(x_i;(\alpha,\beta))=\prod^n_{i=1}\frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}_i e^{-\beta x_i}$$

Com isso, a função log-verossimilhança é dada por:

$$\begin{aligned}
l(\theta|x_i)=\log L(\alpha,\beta)=n\alpha\log\beta -n\log \Gamma(\alpha)-\beta \sum^n_{i=1}x_i+(\alpha-1)\sum^n_{i=1}\log x_i\end{aligned}$$

Para distribuição Gama a função escore é dada por:

$$\begin{aligned}
U(\theta)=\dfrac{\partial \log L(\theta)}{\partial \theta}=\left(\dfrac{\partial \log L(\alpha,\beta)}{\partial \alpha}, \dfrac{\partial \log L(\alpha,\beta)}{\partial \beta}\right)^\top, \end{aligned}$$

em que $$\begin{aligned}
\dfrac{\partial \log L(\alpha,\beta)}{\partial \alpha}&= n\log \beta-n\frac{\Gamma^\prime (\alpha)}{\Gamma (\alpha)}+\sum^n_{i=1}\log x_i,\\
\dfrac{\partial \log L(\alpha,\beta)}{\partial \beta}&= \frac{n\alpha}{\beta}-\sum^n_{i=1}x_i.\end{aligned}$$

Com isso, tomamos $\widehat{U}(\theta)=0$, ou seja,
$\frac{\partial \log L(\alpha,\beta)}{\partial \alpha} =0$ e
$\frac{\partial \log L(\alpha,\beta)}{\partial \beta}=0$. Obtemos os
estimadores para $\alpha$ e $\beta$:

$$\hat{\alpha}=\bar{x}\hat{\beta}$$




$$\hat{\beta}= \exp \left(  \dfrac{\Gamma'(\alpha)}{\Gamma(\alpha)} - \sum_{i=1}^n \dfrac{\log x_i}{n} \right)$$


O sistema de equações precedente não admite forma fechada, sendo
necessário a utilização de métodos numéricos para resolve-lo. Na
literatura existem diversos algoritmos para maximização que envolvem uma
estimativa inicial $\theta_0$ para $\theta$ e um processo iterativo que
constrói uma sequencia de estimadores que convergem para $\theta$. Em
nosso caso em particular, computaremos as estimativas dos parâmetros
pela maximização numérica por meio do algoritmo de otimização não linear
quasi-Newton, conhecido como BFGS (Broyden-Fletcher-Goldfarb-Shanno).

O algoritmo BFGS é similar ao método de Newton-Raphson, distinguindo-se
apenas pelo fato de utilizar uma sequencia de matrizes simétricas e
positivas definidas $B^k$ em vez da oposta da hessiana $-H^{-1}$, de tal
forma que

$$\begin{aligned}
\lim_{k\rightarrow \infty} B^k = -H^{-1}.\end{aligned}$$

Normalmente, utiliza-se a matriz identidade de mesma ordem como matriz
inicial $B^0$, isso porque uma matriz identidade é sempre positiva
definida e simétrica, propagando-se para aproximações $B^k$ positivas
definidas e simétricas.

## Metodologia


Neste trabalho foram consideradas algumas estratégias para avaliar os
estimadores de máxima verossimilhança com o método iterativo BFGS. Foram
realizadas 10.000 replicas de Monte Carlo para esse estudo, e em cada
replica, foi verificado a estimativa dos parâmetros, erro padrão,
intervalos de confiança e viés. Todo o algoritmo foi feito nas
linguagens de programação `R` e `Ox` e as mesmas se encontram no
apêndice.

Das informações relacionadas as versões dos softwares para as simulações
são: versão 4.0.2, utilizando a função `optim` da biblioteca `stats`. A
versão do `Ox`, , é a 8.02, e foi utilizada a função `MaxBFGS` da
biblioteca `maximize`.

A respeito dos geradores de números aleatórios, optou-se por considerar
os geradores *defaults* de ambas linguagens. Sendo `rgamma()` para o `R`
e `rangamma` para o `Ox`.

## Estudo de Simulação

Nessa seção serão apresentados os resultados das simulações para
obtenção das estimativas obtidas pelo método iterativo BFGS com o
estimador de máxima verossimilhança. Foram gerados amostras de tamanho
$n=(10,20,30,50,100,200,300,500,1000)$ da distribuição gama variando os
parametros de escala e forma.

Na Tabela abaixo estão apresentados os resultados para estimativas do parâmetro de escala
$\alpha$.

Para este cenário fixamos o parâmetro de forma em $\beta = 1$, e
variamos o valor de $\alpha$ em 1 e 2, com isso realizamos $10.000$
replicas de Monte Carlo. Note pela Tabela que quando aumentamos
o tamanho da amostra o viés relativo decresce rapidamente para próximo
de zero, isso em ambas linguagens de programação. Podemos ainda
considerar que para amostras maiores que $20$ o método de iteração BFGS
obteve boa aproximação para o valor verdadeiro. Foi construindo um
intervalo de confiança com $95\%$ de confiança sobre a distribuição
normal, pois virmos que quando o $n$ tende a um valor muito grande os
estimadores de máxima verossimilhança se aproximam da distribuição
normal, veremos isso com mais clareza na próxima figura.


##### Simulação no R

|n| $\alpha$   | $\hat{\alpha}$ | IC$(\hat{\alpha})$ | Viés Relativo $(\hat{\alpha})$ |
|:----------:|:----------:|:--------------:|:------------------:|:------------------------------:|
|n=10| 1 | 1.3461         | [0.2755 ; 2.4167]  | 34.61\%                        |
|| 2 | 2.7649         | [0.4687 ; 5.0612]  | 38.25\%                        |
|n=20| 1 | 1.1410         | [0.5095 ; 1.7725]  | 14.10\%                        |
|| 2 | 2.3180         | [0.9705 ; 3.6655]  | 15.90\%                        |
|n=30| 1 | 1.0889         | [0.5992 ; 1.5787]  | 8.89\%                         |
|| 2 | 2.2028         | [1.1606 ; 3.2449]  | 10.14\%                        |
|n=50| 1 | 1.0535         | [0.6878 ; 1.4191]  | 5.35\%                         |
|| 2 | 2.1153         | [1.3422 ; 2.8883]  | 5.76\%                         |
|n=100| 1 | 1.0277         | [0.7762 ; 1.2793]  | 2.77\%                         |
|| 2 | 2.0523         | [1.5230 ; 2.5816]  | 2.62\%                         |
|n=200| 1 | 1.0128         | [0.8378 ; 1.1879]  | 1.28\%                         |
|| 2 | 2.0272         | [1.6578 ; 2.3965]  | 1.35\%                         |
|n=300| 1 | 1.0089         | [0.8666 ; 1.1512]  | 0.89\%                         |
|| 2 | 2.0172         | [1.7172 ; 2.3171]  | 0.86\%                         |
|n=500| 1 | 1.0047         | [0.8950 ; 1.1144]  | 0.47\%                         |
|| 2 | 2.0122         | [1.7805 ; 2.2440]  | 0.61\%                         |
|n=1000| 1 | 1.0027         | [0.9253 ; 1.0801]  | 0.27\%                         |
|| 2 | 2.0060         | [1.8427 ; 2.1693]  | 0.30\%                         |


As figuras abaixo representa a tabela acima.

![](11aR.png)

![](21aR.png)

##### Simulação no Ox

|n| $\alpha$   | $\hat{\alpha}$ | IC$(\hat{\alpha})$ | Viés Relativo $(\hat{\alpha})$ |
|:----------:|:----------:|:--------------:|:------------------:|:------------------------------:|
|n=10| 1 | 1.4099         | [-0.5363 ; 3.3561] | 40.99\%                        |
|| 2 | 2.7733         | [0.9286 ; 4.6181]  | 38.67\%                        |
|n=20| 1 | 1.1762         | [0.7726 ; 1.5797]  | 17.62\%                        |
|| 2 | 2.3250         | [1.8151 ; 2.8349]  | 16.25\%                        |
|n=30| 1 | 1.1157         | [0.6223 ; 1.6092]  | 11.57\%                        |
|| 2 | 2.1980         | [1.2346 ; 3.1614]  | 9.90\%                         |
|n=50| 1 | 1.0720         | [0.5982 ; 1.5457]  | 7.20\%                         |
|| 2 | 2.1166         | [1.5333 ; 2.7000]  | 5.83\%                         |
|n=100| 1 | 1.0397         | [0.8051 ; 1.2744]  | 3.97\%                         |
|| 2 | 2.0579         | [1.4068 ; 2.7090]  | 2.89\%                         |
|n=200| 1 | 1.0237         | [0.8582 ; 1.1892]  | 2.37\%                         |
|| 2 | 2.0299         | [1.6824 ; 2.3774]  | 1.50\%                         |
|n=300| 1 | 1.0174         | [0.8740 ; 1.1609]  | 1.74\%                         |
|| 2 | 2.0209         | [1.6906 ; 2.3512]  | 1.05\%                         |
|n=500| 1 | 1.0127         | [0.8950 ; 1.1303]  | 1.27\%                         |
|| 2 | 2.0126         | [1.7935 ; 2.2317]  | 0.63\%                         |
|n=1000| 1 | 1.0082         | [0.9336 ; 1.0829]  | 0.82\%                         |
|| 2 | 2.0078         | [1.8484 ; 2.1673]  | 0.39\%                         |


As figuras abaixo representa a tabela acima.


![](/11aOx.png)

![](/21aOx.png)

Podemos ver que a medida que o tamanho da amostra aumenta as estimativas se aproximam do
valor verdadeiro (linha vermelha). Além disso, todos os intervalos de
confiança contém o valor do parâmetro verdadeiro.

Como falado anteriormente, as estimativas dos parâmetros tem
distribuição assintoticamente normal. Pela Figura abaixo, podemos ver com
clareza que a estimativa do parâmetro $\hat{\alpha}$, a medida que a
amostra aumenta tem distribuição assintoticamente normal.

![](assin.png)

As mesmas estimativas foram feitas para o parâmetro $\beta$

### Tempo de Execução

Nota-se pela Tabela abaixo que o tempo de
execução na linguagem de programação em `R` é superior a do `Ox`,
tornando a assim uma linguagem menos atraente para simulações mais
pesadas. Vale ressaltar que o `Ox` é aproximadamente 2.1x mais rápido
que o `R`.

| Tamanho | R | Ox |
|:----------------:|:----------:|:-----------:|
| 10      | 29.63      | 4.40        |
| 20      | 29.52      | 4.75        |
| 30      | 28.81      | 4.96        |
| 50      | 29.07      | 5.50        |
| 100     | 29.65      | 7.02        |
| 200     | 32.71      | 10.37       |
| 300     | 35.12      | 14.75       |
| 500     | 42.55      | 20.90       |
| 1000    | 60.20      | 38.97       |

A figura abaixo representa a tabela acima.

![](/te.png)

## Conclusão
---------

O trabalho mostrou toda a parte teórica sobre estimadores de máxima
verossimilhança através de simulações. Simulada replicas de Monte Carlo,
vimos que os estimadores de máxima verossimilhança através do método
BFGS são bastante eficientes para a estimativa do parâmetro, com
amostras grandes os valores estimados chegam muito próximo do valor
verdadeiro. Observamos também na prática, que os parâmetros estimados
seguem assintoticamente a distribuição normal.

Em relação as linguagens de programação `R` e `Ox`, podemos dizer que o
a programação em R é mais eficiente em encontrar o valor verdadeiro,
tendo que o viés relativo sempre ficou menor em comparação com o `Ox`.
Já o `Ox` é mais eficiente em relação ao tempo de execução, tornando a
linguagem mais atraente para simulações mais pesadas.




## R

``` r
# Maximizando a distribuição gama em R
# Computacional-Mestrado (UFPE)
# Jerfson Bruno

library(ggplot2)
library(xtable)
library(gridExtra)

max_gama<-function(a,b,n){
dados <- data.frame(amostra = rgamma(n, a, b))
n_obs <- nrow(dados)
llf<- function(x){
  a <- x[1]
  b <- x[2]
  lgama<-((a-1)*sum(log(dados$amostra))-b*sum(dados$amostra)-n_obs*a*log(1/b)-n_obs*log(gamma(a)))
  return(-lgama)
}
q <- optim(c(1.0, 1.0), llf, hessian = TRUE)
return(q)
}

set.seed(89)
rm<-10000

a=1
b=1

n<-c(10,20,30,50,100,200,300,500,1000)
estimativas_n<-data.frame(n=n,alpha=rep(NA,length(n)),beta=rep(NA,length(n)))
for (i in 1:length(n)){
  contador=1
  estimativas<-data.frame(alpha=rep(NA,rm),beta=rep(NA,rm))
  while(contador<=rm){
    z<-max_gama(a,b,as.numeric(n[2]))
    if(z$convergence==0){
      estimativas$alpha[contador]<-z$par[1]
      estimativas$beta[contador]<-z$par[2]
      estimativas$dp_alpha[contador]<-sqrt(diag(solve(z$hessian)))[1]
      estimativas$dp_beta[contador]<-sqrt(diag(solve(z$hessian)))[2]
      contador=contador+1
      write.csv(estimativas,paste('11-',paste(as.character(n[i]),'.txt',sep = '')), row.names = FALSE)
    }
  }
  estimativas_n$alpha[i]<-mean(estimativas$alpha)
  estimativas_n$beta[i]<-mean(estimativas$beta)
  estimativas_n$ep_alpha[i]<-mean(estimativas$dp_alpha)
  estimativas_n$ep_beta[i]<-mean(estimativas$dp_beta)
  estimativas_n$aIC_I[i]<-mean(estimativas$alpha)-1.96*mean(estimativas$dp_alpha)
  estimativas_n$aIC_S[i]<-mean(estimativas$alpha)+1.96*mean(estimativas$dp_alpha)
  estimativas_n$bIC_I[i]<-mean(estimativas$beta)-1.96*mean(estimativas$dp_beta)
  estimativas_n$bIC_S[i]<-mean(estimativas$beta)+1.96*mean(estimativas$dp_beta)
  estimativas_n$Vies_a[i]<-(mean(estimativas$alpha) - a)/a*100
  estimativas_n$Vies_b[i]<-(mean(estimativas$beta) - b)/b*100
}

write.csv(estimativas_n, 'gama11_R.txt', row.names = FALSE)

```
## Ox
``` ox

###Programação em Ox

// Maximizando a distribuição gama em Ox
// Computacional-Mestrado (UFPE)
// Jerfson Bruno

#include<oxstd.h>
#import<maximize> //
#include<oxprob.h>

decl n = 1000;
decl Rmc = 10000;

static decl vetor_gamma;

gamma(const vetor_parametros, const adFunc, const escore, const hessiana){
decl t, cont;
decl a1 = vetor_parametros[0];
decl a2 = vetor_parametros[1];
decl vone = ones(1, n);

adFunc[0] = (a1-1)*(vone*(log(vetor_gamma)))-a2*(vone*(vetor_gamma))
-n*a1*log(1/a2)-n*log(gammafact(a1));

if(escore){
(escore[0])[0] = n*log(a2) - n*(gammafact(a1)/gammafact(a1)) +
(vone*(log(vetor_gamma)));
(escore[0])[1] = (n*a1)/a2 -(vone*(vetor_gamma));
}

if(isnan(adFunc[0])||isdotinf(adFunc[0]))
return 0;
else
return 1;
}

main(){

decl maxgama, i, var1, variancia, dp, vp, dfunc, mhess, vies;
decl alpha = 2.0, beta = 2.0;
decl v = <2.0, 2.0>;
decl cont = 0;
decl exectime;
decl ep;
decl v_estimativa = zeros(Rmc, 2);
decl media;

exectime = timer();
ranseed("MWC_32");
ranseed(89);

for(i = 0; i < Rmc; i++){
vetor_gamma = rangamma(n, 1, alpha, beta);
vp = <1; 1>;  
maxgama = MaxBFGS(gamma, &vp, &dfunc, 0, TRUE);

if(maxgama == MAX_CONV || maxgama == MAX_WEAK_CONV){
v_estimativa[i][] = vp';
media = meanc(v_estimativa);
variancia = varc(v_estimativa);
dp = sqrt(variancia);
}
else{
i--;
cont++;
}
}

Num2Derivative(gamma, vp, &mhess);
vies = media - v;
ep = sqrt(diagonal(invertsym(-mhess)));
decl a1 = vies[0]/v'[0], a2 = vies[1]/v'[1];
decl Linf = double(dropc(media,1) - (1.96)*dropc(ep,1));
decl Lsup = double(dropc(media,1) + (1.96)*dropc(ep,1));
decl Linfbeta = double(dropc(media,0) - (1.96)*dropc(ep,0));
decl Lsupbeta = double(dropc(media,0) + (1.96)*dropc(ep,0));

println("Convergência: ", MaxConvergenceMsg(maxgama));
println("Amostras:\n ", n);
println("Número de réplicas de Monte Carlo:\n ", Rmc);
println("Número de falhas:\n ", cont);
println("Parâmetro verdadeiro de alpha e beta: \n ", "[", alpha, " ; ", beta,"]");
println("Valores estimados para alpha e beta:\n ", "%10.4f", media);
println("Viés relativo para alpha e beta: \n ","[", a1*100," ; ", a2*100,"]");
println("Intervalo de confiança para alpha:\n ","[", Linf, " ; ", Lsup,"]");
println("Intervalo de confiança para beta:\n ", "[", Linfbeta, " ; ", Lsupbeta,"]");
println("Variância das estimativas:\n ", "%10.4f", variancia);
println("Desvio padrão das estimativas:\n ", "%10.4f", dp);
println("Erro padrão assintótico:\n ", "%10.4f", ep);
println("Tempo de execução:\n ", timespan(exectime));
}

```


