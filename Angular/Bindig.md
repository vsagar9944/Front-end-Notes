 one-way data binding
 
Input Property Binding


There is mostly a 1-to-1 mapping between the names and values of HTML attributes and their equivalent DOM properties, but not always.

So hidden="true" hides the element but confusingly so does hidden="false" in HTML we just need to add hidden to hide the element.

The DOM representation of the hidden attribute is a property also called hidden, which if set to true hides the element and false shows the element.

Angular doesn’t manipulate HTML attributes, it manipulates DOM properties because the DOM is what actually gets displayed.
So when we write [hidden] we are manipulating the DOM property and not the HTML attribute.


<p class="card-text" [hidden]="true">{{joke.punchline}}</p>
The target inside [] is the name of the property.


We can only use this type of binding to change the value of the target. We can’t use it to get notified when the target’s value changes, to do that we need to use something called Output Event Binding, 


<a class="btn btn-primary"
   (click)="joke.hide = !joke.hide">Tell Me
</a>
We have some new syntax with (). The target inside the () is an event we want to listen for, we are listening for the click event.
The text to the right of = is some javascript which will be called every time a click event occurs.


With the [] we are binding to an input of a Component.

With the () we are binding to an output of a Component.





Even though our JokeComponent has a joke property we can’t bind to it using the [] syntax, we need to explicitly mark it as an Input property on our JokeComponent.

This @Input now becomes part of the public interface of our component.

Lets say at some future point we decided to change the joke property of our JokeComponent to perhaps just data, like so:

class JokeComponent {
  @Input() data: Joke;
}
Because this input is part of the public interface for our component we would also need to change all the input property bindings every where our component is used, like so:

<joke *ngFor="let j of jokes" [data]="j"></joke>
Not a great thing to ask the consumers of your component to have to do.

This is a common problem so to avoid expensive refactors the @Input annotation takes a parameter which is the name of the input property to the outside world, so if we changed our component like so:

class JokeComponent {
  @Input('joke') data: Joke;
}
To the outside world the input property name is still joke and we could keep the JokeListComponent template the same as before:

<joke *ngFor="let j of jokes" [joke]="j"></joke>


To create a custom output event on our component we need to do two things:

Create an EventEmitter property on the JokeFormComponent class.

Similar to when we created a custom input property binding, we need to annotate that property with the @Output decorator.

 @Output() jokeCreated = new EventEmitter<Joke>();
 The name between the <> on the EventEmitter is the type of thing that will be output by this property. The syntax above is called generics and we’ll cover them in more detail on the section on TypeScript.
 
 @Output() jokeCreated = new EventEmitter<Joke>();		// This property should of the broadcasting compoennt
 
 
Template Variables
<input type="text"
       class="form-control"
       placeholder="Enter the setup"
       #setup>
setup is only available as a variable in the template, we don’t automatically see the variable setup inside the javascript code of our JokeFormComponent class.
