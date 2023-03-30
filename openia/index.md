# Utilizando a OpenAI (chatGPT) com R

`Todo este exemplo foi criado usando o ChatGPT, uma poderosa linguagem de inteligência artificial desenvolvida pela OpenAI.`

A OpenAI é uma empresa que trabalha com inteligência artificial e aprendizado de máquina, e possui diversas ferramentas e modelos pré-treinados disponíveis para uso através de sua API. Neste post, vamos mostrar como utilizar a API da OpenAI em R com o pacote openai.

Se você quiser criar uma conta na plataforma, basta acessar o site oficial e seguir as instruções para começar a utilizar essa ferramenta incrível. 
## Instalação

Para utilizar o pacote openai, você pode instalar a versão de desenvolvimento diretamente do GitHub utilizando o devtools:

```{r}
install.packages("devtools")
devtools::install_github("samterfa/openai")
```

## Configuração

Para realizar chamadas à API da OpenAI, é necessário utilizar as suas chaves de acesso, que podem ser obtidas em sua conta na plataforma. É possível definir essas chaves como variáveis de ambiente utilizando o seguinte código:

```{r}
Sys.setenv(openai_organization_id = {seu_organization_id})
Sys.setenv(openai_secret_key = {sua_secret_key})
```

Você pode encontrar mais informações sobre a autenticação na API da OpenAI [aqui]{https://platform.openai.com/docs/api-reference/authentication}.

##  Exemplos
gora que já configuramos o ambiente, vamos ver alguns exemplos de como utilizar a API da OpenAI com o R e o pacote openai.

### Listando os modelos disponíveis

Podemos listar os modelos disponíveis na API utilizando a função `list_models()`:

```{r}
library(openai)
library(purrr)
list_models() %>% 
  pluck('data') %>% 
  map_dfr(compact)
```

### Gerando texto com o modelo davinci

Podemos gerar texto utilizando o modelo pré-treinado davinci com a função `create_completion()`:

```{r}
create_completion(
  model = 'davinci', 
  max_tokens = 30,
  temperature = .5,
  top_p = 1,
  n = 1,
  stream = F, 
  prompt = 'Era uma vez') %>% 
  pluck('choices') %>% 
  map_chr(~ .x$text)
```

### Gerando imagens com base em um prompt

Podemos gerar imagens utilizando a função `create_image()` com um prompt de descrição:

```{r}
create_image(
  prompt = "Um zebra patinando", 
  n = 1, 
  response_format = "url")
```

### Colaborando com o modelo utilizando o addin do RStudio

Por fim, podemos utilizar o addin do RStudio para codificar de forma colaborativa com um modelo da OpenAI. O addin pode ser acessado pelo menu Addins ou por Tools -> Addins -> Browse Addins e pesquisando por "openai".

![](openai RStudio demo.gif)

Com o pacote openai, é possível explorar diversas funcionalidades da API da OpenAI diretamente em R. Experimente utilizar os exemplos apresentados neste post e explorar outras funções disponíveis na documentação da OpenAI!



