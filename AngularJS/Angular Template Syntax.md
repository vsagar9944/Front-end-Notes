Property BIndings		flow data in one direction: from a component to an element.
	[src]="filePath"		or bind-src
	[disabled]="isUnchanged"
	[ngClass]="classes"
	[hero]="currentHero"
	
	People often describe property binding as one-way data binding because it flows a value in one direction, from a component's data property into a target element property.
	You cannot use property binding to pull values out of the target element. You can't bind to a property of the target element to read it.
	Similarly, you cannot use property binding to call a method on the target element.
	The target name is always the name of a property
	
	Property binding or interpolation
	src="{{heroImageUrl}}"
	[src]="heroImageUrl"
	bind-src="heroImageUrl"
	
	Interpolation is a convenient alternative to property binding in many cases.
			
	Attribute, class, and style bindings
		Attribute binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix attr, followed by a dot (.) and the name of the attribute. You then set the attribute value, using an expression that resolves to a string.
		<td [attr.colspan]="1 + 1">One-Two</td>
		<button [attr.aria-label]="actionName">{{actionName}} with Aria</button>
		
		Class binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix class, optionally followed by a dot (.) and the name of a CSS class: [class.class-name].
		[class.special]="!isSpecial"
		While this is a fine way to toggle a single class name, the NgClass directive is usually preferred when managing multiple class names at the same time.
		
		Style binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix style, followed by a dot (.) and the name of a CSS style property: [style.style-property].
		[style.background-color]="canSave ? 'cyan': 'grey'" 
		<button [style.font-size.em]="isSpecial ? 3 : 1" >Big</button>
		<button [style.font-size.%]="!isSpecial ? 150 : 50" >Small</button>
	
Event binding ( (event) )
	flow of data in the opposite direction: from an element to a component.
	Event binding syntax consists of a target event name within parentheses on the left of an equal sign, and a quoted template statement on the right.
	<button (click)="onSave()">Save</button>
	<button on-click="onSave()">On Save</button>
	
Two-way binding [(x)]
	Fortunately, the Angular NgModel directive is a bridge that enables two-way binding to form elements.
	
Built in Directives
1. attribute directives 			2. structural directives.

	Many NgModules such as the RouterModule and the FormsModule define their own attribute directives
	NgClass - add and remove a set of CSS classes
	NgStyle - add and remove a set of HTML styles
	NgModel - two-way data binding to an HTML form element				FormsModule is required to use ngModel
	
Built-in structural directives
	Structural directives are responsible for HTML layout. They shape or reshape the DOM's structure, typically by adding, removing, and manipulating the host elements to which they are attached.
	
	This section is an introduction to the common structural directives:
		NgIf - conditionally add or remove an element from the DOM

		NgForOf - repeat a template for each item in a list,NgForOf is a repeater directive — a way to present a list of items.
			<div *ngFor="let hero of heroes">{{hero.name}}</div>
			The index property of the NgForOf directive context returns the zero-based index of the item in each iteration
			<div *ngFor="let hero of heroes; trackBy: trackByHeroes">

			With no trackBy, both buttons trigger complete DOM element replacement.
			With trackBy, only changing the id triggers element replacement.
			
		NgSwitch - a set of directives that switch among alternative views
			<div [ngSwitch]="currentHero.emotion">
				  <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="currentHero"></app-happy-hero>
				  <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="currentHero"></app-sad-hero>
				  <app-confused-hero *ngSwitchCase="'confused'" [hero]="currentHero"></app-confused-hero>
				  <app-unknown-hero  *ngSwitchDefault           [hero]="currentHero"></app-unknown-hero>
			</div>
			NgSwitchCase adds its element to the DOM when its bound value equals the switch value.
			NgSwitchDefault adds its element to the DOM when there is no selected NgSwitchCase.
			
Template reference variables (#var)
	A template reference variable is often a reference to a DOM element within a template. It can also be a reference to an Angular component or directive or a web component.
	
	<input #phone placeholder="phone number">
	<!-- phone refers to the input element; pass its `value` to an event handler -->
	<button (click)="callPhone(phone.value)">Call</button>
	
	The scope of a reference variable is the entire template. Do not define the same variable name more than once in the same template. 
	You can use the ref- prefix alternative to #. This example declares the fax variable as ref-fax instead of #fax.
	
	
	
Binding to a different component
	You can also bind to a property of a different component. In such bindings, the other component's property is to the left of the (=).
	
	<app-hero-detail [hero]="currentHero" (deleteRequest)="deleteHero($event)">
	</app-hero-detail>
	
	Declaring Input and Output properties
	@Input()  hero: Hero;
	@Output() deleteRequest = new EventEmitter<Hero>();
	
	or 
	@Component({
		inputs: ['hero'],			//Input properties usually receive data values
		outputs: ['deleteRequest'],		//Output properties expose event producers, such as EventEmitter objects.
	})
	
	Aliasing input/output properties
	@Output('myClick') clicks = new EventEmitter<string>(); //  @Output(alias)
	  outputs: ['clicks:myClick']  // propertyName:alias
	  

Template expression operators
	pipe and safe navigation operator.
	The result of an expression might require some transformation before you're ready to use it in a binding. 
	
	Pipes
		Angular pipes are a good choice for small transformations such as these. Pipes are simple functions that accept an input value and return a transformed value. They're easy to apply within template expressions, using the pipe operator (|)
		<div>Title through uppercase pipe: {{title | uppercase}}</div>
		 {{title | uppercase | lowercase}}
		 
		 <!-- pipe with configuration argument => "February 25, 1970" -->
		<div>Birthdate: {{currentHero?.birthdate | date:'longDate'}}</div>
		
		<div>{{currentHero | json}}</div>
	
	The safe navigation operator ( ?. ) and null property paths
		The Angular safe navigation operator (?.) is a fluent and convenient way to guard against null and undefined values in property paths. 
		<div *ngIf="nullHero">The null hero's name is {{nullHero.name}}</div>
		or
		The null hero's name is {{nullHero && nullHero.name}}
	
		The null hero's name is {{nullHero?.name}}
		a?.b?.c?.d.
	
The non-null assertion operator ( ! )
The $any type cast function ($any( <expression> ))
	<div>
		The hero's marker is {{$any(hero).marker}}
	</div>
