//https://docs.google.com/document/u/1/d/1XXMvReO8-Awi1EZXAXS4PzDzdNvV6pGcuaF4Q9821Es/pub
//https://github.com/yeoman/generator-angular/issues/109

1. Angularjs App Struture
	DemoApp
		package.json
		node_modules/
		karma.config.js
		protractor.config.js
		gulpfile.js
		gulp/
		e2e/
		bower.json
		bower_components/
		src/
			app
				assets
				uiapp
					core
						src/
							e2e/
							constants/
								core.url.constants.js
								core.templates.constants.js
								core.state.constants.js
							controllers/
								*.controller.js
								*.controller.spec.js  //Unit testing specification file for karma
							services/
								*.service.js
							directives/
								*.directives.js
							filters/
								*.filter.js
							views/
							layouts/
								layout.module.js
							components/
								components.module.js
							core.module.js
							core.config.js
					modules/
						administration
							src/
								constants/
									administration.url.constants.js
									administration.templates.constants.js
									administration.state.constants.js
								controllers/
								services/
								directives/
								filters/
								views/
								layouts/
								components/
								administration.module.js
								administration.config.js
								
								
2.	Naming conventions
	1. Each filename should describe the file's purpose by including the component or view sub-section that it's in, and the type of object that it is as part of the name. For example, a datepicker directive would be in components/datepicker/datepicker-directive.js.
	2. Controllers, Directives, Services, and Filters, should include controller, directive, service, and filter in their name.
	3. File names should be lowercase, following the existing JS Style recommendations. HTML and css files should also be lowercase.
	4. Unit tests should be named ending in *.spec.js, hence  "foo-controller.spec.js" and "bar-directive.spec.js.js"
	5. We prefer, but don't require, that partial templates be named something.html rather than something.ng. (This is because, in practice, most of the templates in an Angular app tend to be partials.)
		
