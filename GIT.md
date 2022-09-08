* [curso udemy](https://www.udemy.com/course/git-completo-do-basico-ao-avancado/learn/lecture/6526432#overview) 

## checkout

* pode ser feito pelo hash do commit, pelo nome da branch ou pelo nome do arquivo/pasta
  * quando feito pelo hash do commit cria uma branch com o conteúdo do repositorio no momento do commit
  * quando feito pelo nome do arquivo/pasta reverte o conteudo daquele arquivo/pasta para versão do ultimo commit
  *quando feito pelo nome da branch cria uma nova branch ou muda para uma branch existente de mesmo nome

## diff

* `git -diff (--staged)`
  * compara a versão dos arquivos da área de trabalho com as versões do último commit
  * a flag `staged` faz a comparação entre as versões dos arquivos da área staged com as do último commit

## log

* `git log (--oneline) (-{quantidade})`
  * imprime o histórico de commits (hash, data, autor, mensagem)
  * a flag `oneline` resume as informações (somente hash e mensagem)
  * é possível passar uma quantidade de commits

* `git log (--patch)`
  * imprime o histórico de commits e a difrenença (diff) entre um commit e seu prévio

* `git log (--stat)`
  * imprime o histórico de commits e o nome dos arquivos modificados

## amend

* `git commit --amend (-m) (--no-edit)`
  * permite alterar o ultimo commit, tanto a mensagem quanto o conteudo
  * para alterar arquivos basta adiciona-los a staged area (git add) e commitar com o amend
  * a flag `-m` é utilizada para atualizar a mensagem
  * a flag `--no-edit` é usada para alterar o conteudo do commit mantendo a mesma mensagem
* obs: o commit original é substituido por um novo commit. Seu hash não existe mais

## parar de rastrear um arquivo

* caso tenha sido adicionado um arquivo que não deveria ser versionado é possivel parar de observa-lo atraves dos comandos abaixo
* o arquivo permanece nas versoes anteriores a execucao do comando. Não é apagado do git, somente não é mais observado a partir daquele momento
* parar de rastrear: ``` git update-index --skip-worktree {nome-do-arquivo} ```
* votar a rastretar: ``` git update-index --no-skip-worktree {nome-do-arquivo} ```