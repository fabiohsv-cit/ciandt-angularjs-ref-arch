# Angular Jedi Project :: Arquitetura de Referência para Projetos AngularJs :: DEMO

## Prerequisitos

* Instalar as seguintes ferramentas:
	- [Node.JS](https://nodejs.org/download)
	- [Git Bash](https://git-scm.com/downloads)

## Executando

* Rodar o seguinte comando via bash

```bash
npm run start
```

* Comando irá carregar todas as dependências do projeto, startar as mocks e subir a aplicação no endereço: http://localhost:8080

* Logue com os usuários
	- admin/pass
	- user/pass

* Problemas com instalação de componentes bower:
	- caso dê problema no clone dos componentes no git, rodar comando abaixo:
	```bash
	git config --global url."https://".insteadOf "git://"
	```

* Testes:
	- Com a aplicação executando, use o seguinte comando para executar testes e2e:
	```bash
	grunt e2e
	```
