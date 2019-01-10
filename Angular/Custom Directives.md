# Custom Directives

**@HostListener** : - This is a *function* decorator that accepts an *event name* as an argument. When that event gets fired on the *host* element it calls the associated function.

```typescript
import { HostListener } from '@angular/core';

@HostListener('mouseover') onHover() {
  window.alert("hover");
}
@HostListener('mouseout') onMouseOut() {
    let part = this.el.nativeElement.querySelector('.card-text');
    this.renderer.setElementStyle(part, 'display', 'none');
 }
```



### @HostBindins

**@HostBinding** :- As well as listening to output events from the host element a directive can also bind to *input properties*in the host element with `@HostBinding`.

This directive can *change* the properties of the host element, such as the list of classes that are set on the host element as well as a number of other properties.

Using the `@HostBinding` decorator a directive can link an internal property to an input property on the host element. So if the internal property changed the input property on the host element would also change.

#### Directive code

```typescript
class CardHoverDirective {
  private ishovering: boolean; 

  constructor(private el: ElementRef,
              private renderer: Renderer) {
  }

  @HostListener('mouseover') onMouseOver() {
    let part = this.el.nativeElement.querySelector('.card-text');
    this.renderer.setElementStyle(part, 'display', 'block');
    this.ishovering = true; 
  }

  @HostListener('mouseout') onMouseOut() {
    let part = this.el.nativeElement.querySelector('.card-text');
    this.renderer.setElementStyle(part, 'display', 'none');
    this.ishovering = false; 
  }
}
```

Now we need to *link* this source property to an input property on the host element, we do this by decorating our `ishovering` boolean with the `@HostBinding` decorator

The `@HostBinding` decorator takes one parameter, the name of the property on the host element which we want to bind to.



```
 @HostBinding('class.card-outline-primary') private ishovering: boolean;
```