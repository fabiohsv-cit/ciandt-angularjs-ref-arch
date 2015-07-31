# Angular Jedi Project :: Arquitetura de Refer�ncia para Projetos AngularJs

## Estrutura base

### Recursos gerais
	assets - componentes externos ou n�o vinculados aos componentes angular do projeto
	assets\libs - bibliotecas externas de terceiros (ex: jquery, bootstrap, etc)
	assets\css - css de terceiros
	assets\css\app.css - css de customiza��o do projeto (o �nico que deve ser modificado)
	assets\fonts - fontes utilizadas pelo projeto
	assets\img - imagens utilizadas na montagem do site
	assets\js - scripts n�o angular customizados ou criados no projeto

### Componentes Gerais Angular
	app - configura��o e componentes angular espec�ficas do projeto
	app\app.js - script com as configura��es b�sicas angular da aplica��o
	app\common - foundation da arquitetura angular que suportar�o todos os m�dulos do projeto (directives, filters, components, etc)
	app\common\common-app.js - script para configura��o do m�dulo common, onde ser�o importados as depend�ncias globais do sistema
	app\common\env\common-env.js - json com variaveis de ambiente gerais da aplica��o
	app\common\i18n\resources_[en|pt|*].js - json de resources para i18n dos componentes deste m�dulo
	app\common\components - componentes angular especializados para o projeto
	app\common\components\[component] - contendo js, html, etc.
	app\common\components\header - componente de tela para montagem do header
	app\common\components\navegation - componente de tela para montagem do menu
	app\common\features\[submodule]\[feature] - funcionalidades globais do sistema, como telas de login, etc.
	app\common\features\auth - telas globais relacionadas com autentica��o
	
- as rota para as funcionalidades dever�o seguir o seguinte padr�o: /common/[submodule]/[feature]

### Estrutura por m�dulo
	app\[module] - m�dulo espec�fico, contendo funcionalidades de neg�cio
	app\[module]\[module]-app.js - script para configura��o do m�dulo, onde ser�o importados as depend�ncias espec�ficas deste m�dulo
	app\[module]\env\[module]-env.js - json com variaveis de ambiente do m�dulo
	app\[module]\i18n\resources_[en|pt|*].js - json de resources para i18n dos componentes e funcionalidades deste m�dulo
	app\[module]\components\[component]\[component].[type] - componentes angular especializados para o m�dulo (js, html, etc)
	app\[module]\features\[submodule*]\[feature] - funcionalidade de neg�cio do m�dulo
	app\[module]\features\[submodule*]\[feature]\[feature]-ctrl.js - controller
	app\[module]\features\[submodule*]\[feature]\[feature]-service.js - service
	app\[module]\features\[submodule*]\[feature]\[feature]-directive.js - directive
	app\[module]\features\[submodule*]\[feature]\[feature]-filter.js - filter
	app\[module]\features\[submodule*]\[feature]\[feature].html - p�gina
	app\[module]\features\[submodule*]\[feature]\[feature]\view\*.html - se houver mais de um arquivo html, organiz�-los na pasta view
	
- as rota para as funcionalidades dever�o seguir o seguinte padr�o: /[module]/[submodule]/[feature]

## Padr�es e Restri��es da Arquitetura

### Gerais

- Utilizar requirejs para controlar carregamento sob demanda dos arquivos javascripts
- Utilizar mecanismo de hash nos nomes dos arquivos para evitar cache do browser. Recomenda-se utilizar o componente [grunt-filerev](https://www.npmjs.com/package/grunt-filerev)
- Utilizar mecanismo [factory](https://github.com/ng-jedi/factory) para declarar controllers, services, filters, directives, modais, etc.
- Sempre incluir depend�ncias externas pelo bower, incluindo no main.js e na configura��o shim
```bash
bower install [nome_dep] --save
```
- Todo arquivo javascript deve ser carregado pelo [module]-app.js de seu m�dulo
- Recursos ligados a uma funcionalidade devem ser criados na estrutura de pastas da funcionalidade
```bash
app\[module]\features\[submodule*]\[feature]\[recursos da feature]
```
- Componentes devem ser criados na estrutura abaixo e n�o devem ser telas, devem ser componentes que comp�em tela, trechos parciais de html ou simplesmente filters, utilit�rios, etc:
```bash
app\[module]\componentes\[component]\[recursos do componente]
```
- Css devem ser codificados/customizados apenas no arquivo assets\css\app.css, demais css s�o de terceiros e n�o devem ser alterados diretamente
- Scripts de terceiros n�o devem ser alterados, em vez disso tentar criar uma vers�o nova e publicar no bower, no pior caso criar no projeto na pasta assets\js\
- Valores hardcode que representam diret�rios ou informa��es que podem ser alteradas de acordo com o ambiente, devem ser transferidos para o script [module]-env.json do m�dulo espec�fico e acessada:
```bash
envSettings[.module].[variable]
```
- Todos os textos dos htmls devem fazer uso da diretiva [i18n](https://github.com/ng-jedi/i18n), para possibilitar a internacionaliza��o posterior ou mesmo durante o projeto.
- M�todos, classes, vari�veis, etc... sempre escritos em ingl�s.
- M�todos, par�metros de m�todos e vari�veis sempre no formato camelCase.
- Nome do recurso (controller, modal, service, etc.) sempre no formato PascalCase.
- Para evitar conflitos, todos os componentes/recursos angular devem ter o namespace no seguinte padr�o:
```bash
app.[module].[submodule].[feature*].[component], ex: app.security.auth.userprofile.UserProfileCtrl
```
- Sempre usar a declara��o 'use strict'; ao in�cio de todo arquivo .js
- Nomes de pastas e arquivos devem ser tudo em min�sculo.
- Todos os componentes angular devem ter dependencias injetadas pelo nome, evitar declarar apenas no construtor do componente, uma vez que a minifica��o encurtar� os nomes dos par�metros.
- Fazer uso de logs atravez do componente $log em vez do console.log
- N�o usar a function �alert� nativa do js, em vez disso usar o componente [dialogs](https://github.com/ng-jedi/dialogs)
- Para camada de servi�o, utilizar componente de abstra��o [Restangular](https://github.com/mgonto/restangular)

### Controllers

- Utilizar mecanismo [factory](https://github.com/ng-jedi/factory), m�todo factory.newController
- Utilizar padr�o VM para declara��o dos atributos e m�todos do controlador
- Nomenclatura:
	- Pasta f�sica: app\\[module]\features\\[submodule]\\[feature]\
	- Nome Controller: app.[module].[submodule].[feature*].[feature]Ctrl
	- Model: [feature]Model
- No corpo do controle deve-se seguir a seguinte ordem de declara��o:
* Declara��o dos servi�os
```javascript
	var service = SecurityRestService.all('admin/feature');
```
* Declara��o do vm (view model)
```javascript
	var vm = this;
```
* Declara��o do model e demais vari�veis de controle
```javascript
	vm.featureRegistrationModel = { id: 1, name: 'teste 1' };
	vm.maxFileCount = 0;
	vm.pageSize = $rootScope.appContext.defaultPageSize;
```
* Bind dos m�todos
```javascript
	vm.filter = filter;
	vm.remove = remove;
	vm.clear = clear;
```
* Execu��es de m�todos, carregamentos de dados ou qualquer execu��o na inicializa��o da tela
```javascript
	loadSystems(function (systems) {
		vm.featureRegistrationModel.systems = systems;
	});
```
* Declara��o dos m�todos e seu statement
```javascript
	function loadSystems(success) {
		console.log('Recuperando systems');
		SecurityRestService.all('admin/system').getList().then(success);
	}
```
- N�o deve haver regra de neg�cio nos controllers, o mesmo dever� estar presente no escopo das APIs apenas.
- Servi�os n�o devem ser expostos no vm nem em nenhum outro atributo, devem sempre passar por m�todos do controller.
- Todos os atributos da tela que forem relacionados ao modelo devem ser declarados no vm.[feature]Model, ex:
```javascript
vm.featureRegistrationModel = { id: 1, name: 'teste 1' };
```

### Views
- Nomenclatura:
	- Pasta f�sica: app\\[module]\features\\[submodule]\\[feature]\
	- Nome p�gina: [feature].html
- Sempre constru�do com html puro, seguindo os padr�es estruturais do twitter bootstrap, sem javascript e usando apenas diretivas angular
- ng-repeat deve sempre ser declarado com track by, para evitar problemas de performance
- Utilizar componentes [layout](https://github.com/ng-jedi/layout), em especial o app-input na declara��o dos campos da tela, para mantr todos no mesmo padr�o visual
- Em tabelas de consultas, usar por padr�o a diretiva [at-table](https://github.com/mateusmcg/angular-table-restful) com pagina��o via api rest
- Na declara��o do controller da tela, usar alias em formato camelCase, ex:
```html
ng-controller="app.framework.imports.importfiles.ImportFilesCtrl as importFilesCtrl"
```
- N�o declarar styles nos elementos html, em vez disso usar classe dos css de terceiros ou os declarados no arquivo assets\css\app.css
- Todo texto em html dever� fazer uso da diretiva jd-i18n
```html
<jd-i18n>Texto qualquer<jd-i18n>

Ou

<a jd-i18n>Texto qualquer<\a>
```

### Directives
- Utilizar mecanismo [factory](https://github.com/ng-jedi/factory), m�todo factory.newDirective
- Diretivas sempre declaradas com o nome do m�dulo e subm�dulo, para evitar duplicidade e sobreposi��o em caso de projetos grandes e distribu�dos
- Nomenclatura:
	- **Se geral para o m�dulo**
	- Arquivo: app\\[module]\components\\[component]\\[component]-directive.js
	- Nome diretiva: app-[module]-[component]-[diretiva]
	- **Se for de uma feature**
	- Arquivo: app\\[module]\features\\[feature]\\[feature]-directive.js
	- Nome diretiva: app-[module]-[submodule]-[feature]-[diretiva]

### Filters
- Utilizar mecanismo [factory](https://github.com/ng-jedi/factory), m�todo factory.newFilter
- Nomenclatura:
	- **Se geral para o m�dulo**
	- Arquivo: app\\[module]\components\\[component]\\[component]-filter.js
	- Nome diretiva: app[module][component][filter]
	- **Se for de uma feature**
	- Arquivo: app\\[module]\features\\[feature]\\[feature]-filter.js
	- Nome diretiva: app[module][submodule][feature][filter]

### Modais
- Utilizar mecanismo [factory](https://github.com/ng-jedi/factory), m�todo factory.newModal
- Seguir mesmas regras do controller + directive, modal utiliza os dois tipos de defini��o juntas.

## Refer�ncias:

### ng-jedi scaffold:
- https://github.com/ng-jedi/generator-ng-jedi-ref-arch

### ng-jedi components:
- https://github.com/ng-jedi/breadcrumb
- https://github.com/ng-jedi/dialogs
- https://github.com/ng-jedi/factory
- https://github.com/ng-jedi/i18n
- https://github.com/ng-jedi/layout
- https://github.com/ng-jedi/loading
- https://github.com/ng-jedi/utilities

### Fontes externas de pesquisa:
- https://scotch.io/tutorials/angularjs-best-practices-directory-structure
- https://github.com/johnpapa/angular-styleguide