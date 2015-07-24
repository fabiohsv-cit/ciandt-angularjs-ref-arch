# Arquitetura de Refer�ncia CI&T para Projetos AngularJs

> [Yeoman](http://yeoman.io) generator

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

## License

LGPL