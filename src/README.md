# Desafio Kubedev
Objetivo deste desafio e transformar esta aplicação em uma imagem Docker seguindo os padrões de documentação.
## Etapa para criar uma imagem
  * Identificar em qual linguagem foi desenvolvida e sua versão caso seja necessário;
  * Identificar dependências além do arquivos de dependências da aplicação;
  * Definir se é uma aplicação stateless ou stateful;
  * Identificar requisitos não funcionais ou serviço de apoio;
    * ex: Vai utilizar um padrão sidecar com logs.
    * ex: vai necessita de banco de dados.
  * Identificar se será necessário variáveis de ambiente;
    * Definir se será variáveis em momento de construção ou momento de execução.
  * Definir porta padrão de trabalho;
    * Definir se é portão será para serviço interno ou externo.
### As etapas anteriores são baseadas nas recomendações 12factor link abaixo.
  [12factor](https://12factor.net/)

## Descrição da Aplicação
  - É uma aplicação desenvolvida em node.js.
    - Não tem definição de versão, irá utilizar a mais atual.
    - **Package.json** define as dependências da aplicação.
    * **server.js** Arquivo que inicia a aplicação e define a porta de trabalho. 
  - Porta de padrão de trabalho *8080*.
  - Sem necessidade de variáveis na criação do container.
  - Aplicação tipo stateless.
  - Sem requisitos funcionais e de apoio.
  - Necessário instalar as dependências, utilizar npm ou yarn.

## Recomendações
  - Trabalhar com layer de dependências separadas da aplicação, e assim ganhar performance no momento de build.
  - Se possível trabalhar com versão da imagem Alpine.
  - Criar arquivo *.dockerignore*  para evitar de copiar arquivos desnecessários.
  - Criar arquivo *Dockerfile* seguindo as recomendações oficiais Dockerinc.
    - [Best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
    - [exemplo de Dockerfile](https://docs.docker.com/language/nodejs/build-images/)
## Exemplo de DockerFile e comentário
  - Abaixo está um exemplo de DockerFile simples
```sh
FROM node:17-alpine 
WORKDIR /app
# Na etapa abaixo realizamos a build em uma layer separada como no requisito. 
COPY ["package.json", "package-lock.json*", "./"]
RUN npm install --production
COPY . .
EXPOSE 8080
CMD [ "node", "server.js" ]
  ```

  - Abaixo está um exemplo de DockerFile multistage 
    - Com a imagem abaixo temos um ganho de 10MB
```sh
FROM node:17 AS build
WORKDIR /app
COPY ["package.json", "package-lock.json*", "./"]
RUN npm install --production
FROM node:17-alpine AS deploy
COPY --from=build /app/node_modules ./node_modules
COPY . .
EXPOSE 8080
CMD [ "node", "server.js" ]
``` 
