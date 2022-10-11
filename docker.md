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
    restart: <metodo-de-reinicialização>       // método de reinicialização em caso de queda do container (pode ter os valores - no, on-failure, always, unless-stopped)
    ports:        // mapeamento de portas em forma de lista 
      - <porta-local>:<porta-container>
    depends_on:         // define a ordem que os containers devem ser montados
      - <nome-do-serviço>
    volumes:          // mapeamento de volumes em forma de lista 
      - <caminho-local-ou-nome-volume-virtual>:<caminho-container>
    networks:          // mapeamento de redes em forma de lista
      - <nome-da-rede>
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
