# TypeScript

## Instalação e inicialização
- global
        - `npm install -g typescript@4.4`
        - `tsc --init` para gerar arquivo tsconfig.json
        - executar com `tsc nomeDoArquivo.ts` que gera um arquivo .js que pode ser executadocom `node nomeDoArquivo.js`
- local
        - `npm init -y` 
        - `npm i -D @tsconfig/node16@1.0 @types/node@16.11 typescript@4.4` 
                - instala o módulo typeScript e seus tipos, além do pacote de configuração do tsconfig.json para transpilar em node versão 16
        - `npx tsc --init` para gerar arquivo tsconfig.json
        - executar com `npx tsc nomeDoArquivo.ts` que gera um arquivo .js que pode ser executadocom `node nomeDoArquivo.js`
