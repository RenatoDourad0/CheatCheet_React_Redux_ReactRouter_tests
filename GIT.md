* [curso udemy](https://www.udemy.com/course/git-completo-do-basico-ao-avancado/learn/lecture/6526432#overview) 

## checkout

* pode ser feito pelo hash do commit, pelo nome da branch ou pelo nome do arquivo/pasta
  * quando feito pelo hash do commit faz uma detatched head com o conteúdo do repositorio no momento do commit, pode se criar uma nova branch a partir disso
  * quando feito pelo nome do arquivo/pasta reverte o conteudo daquele arquivo/pasta para versão do ultimo commit
  * quando feito pelo nome da branch cria uma nova branch ( -b ) ou muda para uma branch existente de mesmo nome
  * pode ser feito pelo nome da tag

## switch

* troca de branch
* ( -c ) cria nova branch e troca para ela
* ( - ) volta para branch anterior

## push 

* origin é o repositorio remoto
* ( -u ) cria a versão remota de uma branch local. Set-upstream
* ( --delete ) apaga uma branch remota

## branch

* ( -d (nome)) deleta a branch local
* ( -m (nome_antigo) (nome_novo)) renomeia branch local
* ( --no-merged ) mostra as branchs que não foram mergeadas com a branch que estou
* ( --merged ) mostra as branchs que foram mergeadas com a branch que estou

## marge

* deve ser feito na branch que vai receber as alterações
* ( --abort ) cancela um processo de merge iniciado

## diff

* `git diff (--staged)`
  * compara a versão dos arquivos da área de trabalho com as versões do último commit
  * a flag `staged` faz a comparação entre as versões dos arquivos da área staged com as do último commit
  *  pode ser usado para comparar as versões entre dois commits `git -diff ( hash commit 1 ) ( hash commit 2 )`
  * pode ser usado para comparar as versões entre duas tags `git -diff ( tag1 ) ( tag2 )`

## log

* `git log (nome_da_branch) (--oneline) (-{quantidade})`
  * imprime o histórico de commits (hash, data, autor, mensagem)
  * a flag `oneline` resume as informações (somente hash e mensagem)
  * é possivel especificar uma branch
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

## tag

* É uma marcação em um commit
* ( git tag ( nome_da_tag ) ( numero_do_commit ) ) cria uma nova tag sem anotações, pode ser especificado o commit a ser usado
* ( -a -m "( mensagem_da_tag )" ( nome_da_tag ) ( numero_do_commit ) ) cria uma nova tag com anotações
* `git show ( nome_da_tag )` mostra as informações da tag
* ( git tag -l ) lista as tags
* ( git tag -n ) lista as tags e suas descrições
* `git push origin ( nome_da_tag )` para enviar para o remoto
* `git tag -d ( nome_da_tag )` para remover localmente e `git push --delete origin (nome_da_tag)`

## stash

* `git stash` na branch desejada para salvar alterações
* `git stash list` para visualizar stashes. O 0 é o mais recente
* `git stash apply ( stash@{numero_do_stash} )` na branch desejada para aplicar as mudanças salvas no stash
* 
## parar de rastrear um arquivo

* caso tenha sido adicionado um arquivo que não deveria ser versionado é possivel parar de observa-lo atraves dos comandos abaixo
* o arquivo permanece nas versoes anteriores a execucao do comando. Não é apagado do git, somente não é mais observado a partir daquele momento
* parar de rastrear: ``` git update-index --skip-worktree {nome-do-arquivo} ```
* votar a rastretar: ``` git update-index --no-skip-worktree {nome-do-arquivo} ```

## revertendo mudanças

* Para retornar um arquivo para sua versão original (remota) usar o `git checkout` com o nome do arquivo
* Para retornar toda a pasta para versão original (remota) usar o `git checkout `
* Para apagar um arquivo criado no ambiente de trabalho que ainda não tem uma versão remota usar o `git clean -f` (remove arquivos não rastreados)
* Para reverter um arquivo staged para a área de trabalho seguir orientações do terminal e depois é possivel o git checkout
