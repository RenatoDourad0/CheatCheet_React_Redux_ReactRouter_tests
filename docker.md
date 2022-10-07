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
- containers já iniciados podem ser executados com o comando ``docker container start <id_do_container>``

## acessando o terminal do container
- ``docker exec -it <nome-do-container> <comando-a-ser-executado>``
  - a flag `-t` cria uma nova seção do terminal no container
  - a flag `-i` faz com que esse novo terminal seja interativo
  - o comando `exit` sai do terminal
- o comando ``docker container attach <nome-do-container>`` também entra no container

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
