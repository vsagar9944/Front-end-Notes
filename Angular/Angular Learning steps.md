# Angular Learning steps

[Angular Toddmotto.com****](https://toddmotto.com/angular/)

[Angular's NgIf, Else, Then - Explained](https://toddmotto.com/angular-ngif-else-then)

[Handling Observables with NgIf and the Async Pipe](https://toddmotto.com/angular-ngif-async-pipe)

[Step by Step Custom Pipes in Angular](https://toddmotto.com/angular-pipes-custom-pipes)

[Angular ngFor, <ng-template> and the compiler](https://toddmotto.com/angular-ngfor-template-element)

[Polling in Angular](https://nehalist.io/polling-in-angular/)

[Working with models in Angular](https://nehalist.io/working-with-models-in-angular/)

[Uploading files in Angular (2/4) to a REST api](https://nehalist.io/uploading-files-in-angular2/)

[Downloading files from Ajax POST Requests](https://nehalist.io/downloading-files-from-post-requests/)



## Dependency Injection

[Mastering Angular dependency injection with @Inject, @Injectable, tokens and providers](https://toddmotto.com/angular-dependency-injection)

[Dependency Injection in TypeScript](https://nehalist.io/dependency-injection-in-typescript/)



# Basic Rules for developing Angular APP

1. For any Obserable property suffix variable with **$**

2. Taking input to any component

   1. <contact-card [contact]="contact"></contact-card>

   2. ```
      export class ContactCardComponent {
        @Input() contact: Contact;
      }
      ```

3. Returning output

   1. @Output

4. Using trackBy in *ngFOr

   ```
   <li *ngFor="let contact of contacts | async; trackBy: trackById;">
   // This is a function that we’ll add in the component class:
   trackById(index, contact) {
     return contact.id;
   }
   ```

5. Let’s explore `index` and `count`, two public properties exposed to us on each `ngFor` iteration.

6. ngFor properties exposed to us *first*, *last*, *even*, *odd*, *index*, *count*

7. When using an asterisk (`*`) in our templates, we are informing Angular we’re using a structural directive, which is also sugar syntax (a nice short hand) for using the `<ng-template>` element.

8. First off, `<ng-template>` is Angular’s own implementation of the `<template>`