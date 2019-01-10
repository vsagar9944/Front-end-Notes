## [View Encapsulation](https://codecraft.tv/courses/angular/components/templates-styles-view-encapsulation/#_view_encapsulation)

Even though we changed the background color of `.card` and we have multiple cards on the page only the form component card was rendered with a gray background.

Normally if we change a css class the effect is seen throughout an application, something special is happening here and it’s called *View Encapsulation*.

Angular is inspired from Web Components, a core feature of which is the shadow DOM.

The shadow DOM lets us include styles into Web Components without letting them *leak* outside the component’s scope.

Angular also provides this feature for Components and we can control it with the `encapsulation`property.

The valid values for this config property are:

- `ViewEncapsulation.Native`
- `ViewEncapsulation.Emulated`
- `ViewEncapsulation.None`.

The default value is `ViewEncapsulation.Emulated` and that is the behaviour we are currently seeing.

### [ViewEncapsulation.Emulated](https://codecraft.tv/courses/angular/components/templates-styles-view-encapsulation/#_viewencapsulation_emulated)

Let’s inspect the form element with our browsers developer tools to investigate what’s going on.

By looking at the DOM for our `JokeFormComponent` we can see that Angular added some *automatically*generated attributes, like so.

![encapsulation emulated html](https://codecraft.tv/assets/images/courses/angular/4.components/encapsulation-emulated-html.png)

Specifically it added an attribute called `_ngcontent-qwe-3`.

The other components on the page don’t have these automatically generated attributes, only the `JokeFormComponent` which is the only component where we specified some styles.

Again by looking at the styles tab in our developer tools we can see a style is set for `_ngcontent-qwe-3`like so:

![encapsulation emulated css](https://codecraft.tv/assets/images/courses/angular/4.components/encapsulation-emulated-css.png)

#### Note

The css selector `.card[_ngcontent-qwe-3]` targets *only* the `JokeFormComponent` since that is the only component with a html attribute of `_ngcontent-qwe-3`.

In the `ViewEncapsulation.Emulated` mode Angular changes our generic css class selector to one that target just a single component type by using automatically generated attributes.

This is the reason that *only* the `JokeFormComponent` is gray despite the fact that we use the same card class for all the other `JokeComponents` as well.

Any styles we define on a component *don’t leak out* to the rest of the application but with `ViewEncapsulation.Emulated` our component still inherits global styles from twitter bootstrap.

Our `JokeFormComponent` still gets the global card styles from twitter bootstrap and the encapsulated style from the component itself.

### [ViewEncapsulation.Native](https://codecraft.tv/courses/angular/components/templates-styles-view-encapsulation/#_viewencapsulation_native)

If we want Angular to use the *shadow DOM* we can set the encapsulation parameter to use `ViewEncapsulation.Native` like so:

```
Copy@Component({
  selector: 'joke-form',
  templateUrl: 'joke-form-component.html',
  styles: [
    `
    .card {
      background-color: gray;
    }
    `
  ],
  encapsulation: ViewEncapsulation.Native # <!>
})
class JokeFormComponent {
  @Output() jokeCreated = new EventEmitter<Joke>();

  createJoke(setup: string, punchline: string) {
    this.jokeCreated.emit(new Joke(setup, punchline));
  }
}
```

But now if we look at the application although the background color of the `JokeFormComponent` is still gray, we’ve *lost* the global twitter bootstrap styles.

![ViewEncapsulation.Native](https://codecraft.tv/assets/images/courses/angular/4.components/encapsulated-native-screen.png)

With `ViewEncapsulation.Native` styles we set on a component *do not leak outside* of the components scope. The other cards in our application do not have a gray background despite the fact they all still use the card class.

This is great if we are defining a 3rd party component which we want people to use in isolation. We can describe the look for our component using css styles without any fear that our styles are going to leak out and affect the rest of the application.

However with `ViewEncapsulation.Native` our component is also isolated from the global styles we’ve defined for our application. So we don’t inherit the twitter bootstrap styles and have to define all the required styles on our component decorator.

Finally `ViewEncapsulation.Native` requires a feature called the *shadow DOM* which is not supported by all browsers.

### [ViewEncapsulation.None](https://codecraft.tv/courses/angular/components/templates-styles-view-encapsulation/#_viewencapsulation_none)

And If we don’t want to have any encapsulation at all, we can use `ViewEncapsulation.None`.

The resulting application looks like so:

![encapsulated none screen](https://codecraft.tv/assets/images/courses/angular/4.components/encapsulated-none-screen.png)

By doing this all the cards are now gray.

If we investigate with our browser’s developer tools we’ll see that Angular added the `.card` class as a *global style* in the head section of our HTML.

![encapsulated none html](https://codecraft.tv/assets/images/courses/angular/4.components/encapsulated-none-html.png)

We are not encapsulating anything, the style we defined in our card form component has leaked out and started affecting the other components.