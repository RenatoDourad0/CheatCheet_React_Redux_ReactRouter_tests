# cheatSheet Docker

## listando imagens e containers

### imagens
- ``docker images``

### containers
- ``docker container ls`` ou ``docker ps``
  - tag `-a` lista também os container parados
  - tag `-l` lista somente o ultimo container executado

## executando containers
- ``docker container run <flags>? <name-da-imagem> <argumentos>?``
  - a imagem é identificada por ``<imagem>:<tag>``
  - `<flags>?` e ` <argumentos>?` são opcionais
    - a flag ``--name <nome-da-sua-escolha>`` permite nomear o container
    - a flag `--rm` altera o comportamento padrão, removendo o container após sua execução (a imagem do container permanece na maquina)
    - a flag `-d` ou `--detach` permite que o container seja executado em segundo plano
    - a flag `-t` cria uma nova seção do terminal no container
    - a flag `-i` faz com que esse novo terminal seja interativo
    - a flag `-P` mapeia uma porta local com uma porta do container, ambas aleatórias
    - a flag `-p` mapeia uma porta local com uma porta do container. Devem ser informadas no formato `<porta local>:<porta container>`
    - a flag `-v`
- containers já iniciados podem ser executados com o comando ``docker container start <id_do_container>``

## acessando o container
- ``docker exec -it <nome-do-container> <comando-a-ser-executado>``
  - a flag `-t` cria uma nova seção do terminal no container
  - a flag `-i` faz com que esse novo terminal seja interativo
  - ``docker container exec -it <nome-do-container> bash (ou sh)`` entra em uma novo terminal do container sem executar nada
- o comando `exit` sai do terminal e encerra o container
- para sair do terminal sem encerrar o container `cmd p + cmd q`
- o comando ``docker container attach <nome-do-container>`` também entra no container mas em seu terminal principal

## monitoranto processos de um container
- o comando ```docker top <nome-do-container>``` lista os processos que estão sendo rodados dentro do container

## visualizando logs dos processos de um container
- caso o container seja executado em segunda plano seus logs não seram impressos
- o comando ```docker logs <flags>? <nome-do-container>``` permite visualizar os logs dos processos daquele container
- também pode ser usado para ver os logs após a finalização dos processos

## interrompendo a execução de containers
- ``docker stop <flags>? <nome-do-container>``
  - a flag ``-t 0`` faz uma interrupção forçada

## removendo containers e imagens
- ``docker rm <flags>? <nome-ou-id-do-container>``
  - só funciona se o conteiner estiver parado ou terminado a execução
  - a flag `-f` força a remoção inclusive de containers ainda ativos
- outa opção é o ``docker container prune`` que limpa todos os container parados na maquina
- ``docker rmi <nome-ou-id-da-imagem>`` para remover imagens especificas
- ``docker system prune -af`` para remover todos os containers e imagens do sistema

## criando imagens
- ``docker build <flags>? <contexto>``
  - a flag `-t` permite nomear a imagem - ``docker build -t <nome-da-imagem> <contexto>``
    - `<nome-da-imagem>` corresponde a um nome e uma tag no formato `nome:tag`, caso não seja definida a tag ela assumirá o valor latest
  - o `<contexto>` é o caminho para o arquivo dokerfile (normalmente `.`)

## arquivo Dokerfile
- palavras reservadas:
  - `FROM`
    - define imagem base
    - cada `FROM` representa uma nova imagem intermediária
    - pósfixo `AS` permite definir o nome dos estagios para o processo de build
  - `WORKDIR`
  - `COPY`
    - a flag `--from=<nome-do-estagio>` é usada para copiar arquivos de outro estágio para o atual
  - `ADD`
  - `EXPOSE`
  - ``RUN <comando> <argumento1> <argumentoN>``
    - Indica que o comando dado deve ser executado durante a construção da imagem docker
  - ``ENTRYPOINT <comando> <argumento1> <argumentoN>``
    - Indica qual é o comando (e seus argumentos caso tenha) que deve ser executado ao iniciar a imagem Docker como um container
  - ``CMD <comando> <argumento1> <argumentoN>``
    - Indica qual é o comando (e seus argumentos) ou somente os argumentos que PODEm ser executados ao iniciar esta imagem Docker como um container. Somente uma sujestão
  - `ENV <chave>=<valor>`
    - usado para setar variaveis de ambiente

## arquivo docker-compose.yaml
```
version: "3"       // versão dos comandos a serem utilizados no arquivo
services:
  <nome-do-serviço>:
    build: <nome-da-imagem-ou-caminho-Dockerfile>       // imagem a ser utilizada (build no caso de se contruir a imagem na hora a partir do Dockerfile e image se usar uma imagem pronta)
    container_name: <nome-container>
    working_dir: <caminho-padrão>
    restart: <metodo-de-reinicialização>       // método de reinicialização em caso de queda do container (pode ter os valores - no, on-failure, always, unless-stopped)
    ports:        // mapeamento de portas em forma de lista 
      - <porta-local>:<porta-container>
    depends_on:         // define a ordem que os containers devem ser montados
      - <nome-do-serviço>
    volumes:          // mapeamento de volumes em forma de lista 
      - <caminho-local-ou-nome-volume-virtual>:<caminho-container>
    networks:          // mapeamento de redes em forma de lista
      - <nome-da-rede>
    environment:
      - <nome-variavel>=<valor>
   
   # interativo
   stdin_open: true
   tty: true
   # oque roda ao iniciar o container
    command: sh
  database:
    image: betrybe/docker-compose-example-database:v1
    restart: always
    volumes:
      - dados-do-banco:/data/db
    networks:
      - rede-virtual-1
    environment:            // definição de variáveis de ambiente em formato de lista
      - DB_HOST=database
volumes:          // declaração dos volumes virtuais
  dados-do-banco:
networks:        // declaração das redes criadas
  rede-virtual-1:
```
  - ``docker-compose up -d`` na pasta do arquivo docker-compose.yaml para subir os containeres
    - ``docker-compose up --build -d`` para atualizar e inicializar os containeres  
  - ``docker-compose down`` para cancelar processos e desmontar os containeres
    - a flag `--volumes` tambem remove os volumes  
  - ``docker-compose build`` para atualizar os containers

## docker + react
  - usando uma imagem node para build e um servidor nginx para ambiente de produção
      - o caminho do entrypoint deve ser `/` e não deve ter um homepage definido no package.json
 ```
 // .dockerignore
 
node_modules
build
 ```
```
 // nginx.conf
 
server {
  listen 80;

  location / {
    root /usr/share/nginx/html/;
    include /etc/nginx/mime.types;
    try_files $uri $uri/ /index.html;
  }
  error_page 500 502 503 504 /50x.html;
  location = /50x.html {
    root /usr/share/nginx/html/;
  }
}
```
```
 // Dockerfile
 
FROM node:16-alpine as builder
# Set the working directory to /app inside the container
WORKDIR /app
# Copy app files
COPY package.json .
COPY package-lock.json .
# Install dependencies
RUN npm install 
# Build the app
COPY . .
RUN npm run build

# Bundle static assets with nginx
FROM nginx:1.21.0-alpine as production
ENV NODE_ENV production
# Copy built assets from `builder` image
COPY --from=builder /app/build /usr/share/nginx/html
# Add your nginx.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf
# Expose port
EXPOSE 80
# Start nginx
CMD ["nginx", "-g", "daemon off;"]
```
  - usando uma imagem node para ambiente de desenvolvimento
```
// .dockerignore

node_modules
```
```
// Dockerfile

FROM node:16-alpine as builder
# Set the working directory to /app inside the container
WORKDIR /app
# Copy app files
COPY package.json .
COPY package-lock.json .
# Install dependencies
RUN npm install 
# Build the app
COPY . .
EXPOSE 80
RUN npm start
```
  - para iniciar o container `docker run --name <nome-container> -d -p 3000:80 <nome-imagem>`
 
## docker + mysql
  - [imagem mysql](https://hub.docker.com/_/mysql)
  - criar um container executando o servidor mysql
    - `docker run --name <nome-container> -e MYSQL_ROOT_PASSWORD=<senha-sql> -d mysql:<tag>` 
  - criar um segundo container que se conecta ao primeiro
    - `docker run -it --network <nome-rede> --rm mysql mysql -h<nome-host> -u<nome-usuario> -p`
