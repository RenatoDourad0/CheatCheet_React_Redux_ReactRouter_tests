# cheatSheet Docker

## listando imagens e containers

### imagens
- ``docker images``

### containers
- ``docker container ls`` ou ``docker ps``
- tag `-a` lista também os container parados
- tag `-l` lista somente o ultimocontainer executado

## iniciando containers
- ``docker container run <flags>? <imagem>:<tag> <argumentos>?``
- a imagem é identificada por ``<imagem>:<tag>``
- `<flags>?` e ` <argumentos>?` são opcionais
  - a flag ``--name <nome-da-sua-escolha>`` permite nomear o container
  - a flag `--rm` altera o comportamento padrão, removendo o container após sua execução (a imagem do container permanece na maquina)
  - a flag `-d` ou `--detach` permite que o container seja executado em segundo plano
  - a flag '-t' cria uma nova seção do terminal no container
  - a flag '-i' faz com que esse novo terminal seja interativo
- containers já iniciados podem ser executados com o comando ``docker container start <id_do_container>``

## acessando o terminal do container
- ``docker exec -it <nome-do-container> <comando-a-ser-executado>``
- a flag '-t' cria uma nova seção do terminal no container
- a flag '-i' faz com que esse novo terminal seja interativo
- o comando `exit` sai do terminal
- o comando ``docker container attach <nome-do-container>`` também entra no container

## monitoranto processos de um container
- o comando ```docker top <nome-do-container>``` lista os processos que estão sendo rodados dentro do container

## visualizando logs dos processos de um container
- caso o container seja executado em segunda plano seus logs não seram impressos
- o comando ```docker logs <flags> <nome-do-container>``` permite visualizar os logs dos processos daquele container
- também pode ser usado para ver os logs após a finalização dos processos

## interrompendo a execução de containers
- ``docker stop <flags>? <nome-do-container>``
- a flag ``-t 0`` faz uma interrupção forçada

## removendo containers
- ``docker rm <nome-do-container>``
- só funciona se o conteiner estiver parado ou terminado a execução
- - a flag `-f` força a remoção inclusive de containers ainda ativos
- outa opção é o ``docker container prune`` que limpa todos os container parados na maquina
