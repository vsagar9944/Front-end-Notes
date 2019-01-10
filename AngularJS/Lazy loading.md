# Lazy loading

In this guide, we will explore lazy loading in UI-Router.

Lazy loading is the practice of loading expensive resources on-demand. This can greatly reduce the initial startup time for single page web applications. Instead of downloading all the application code and resources before the app starts, they are fetched just-in-time, as needed.

## General purpose lazy loading

UI-Router provides basic lazy loading capabilities using the `lazyLoad` property on a state definition. Before the state is entered, its `lazyLoad` function is invoked. The router waits until the promise returned by the function succeeds. After the successful promise has resolved, the state is entered as usual.

```
var mystate = {
  name: 'messages',
  url: '/messages',
  component: messages,
  lazyLoad: function() {
    return System.import('moment');
  }
}

```

Load moment.js before activating the `messages` state

The general purpose `lazyLoad` property has the following behavior:

- The `lazyLoad` function is invoked when the state it belongs to is being entered.
- The function receives the current `Transition` and the state object (`StateDeclaration`).
- The transition waits until the promise returned by the `lazyLoad` function is successful.
- The transition fails if the promise is rejected.
- Once the promise returned by `lazyLoad` is successful, the function will not be invoked again, even when the state is entered a second time.
- If the `lazyLoad` promise is not complete, but the state is about to entered a second time, the function is not invoked again. The existing promise is used instead. This avoids lazy loading the same code multiple times when, e.g., the user double clicks a link and starts two Transitions to the same state.

## Lazy Loading Feature Modules

In addition to general purpose lazy loding, UI-Router also provides lazy loading of application modules.

An normal app (which doesn’t use lazy loading) often bundles all its code and libraries together into a single javascript file.

![img](https://ui-router.github.io/assets/guide-lazyloading/monolithic-bundle.png)A monolithic app bundle

To use modular lazy loading, the application should be split up into logical chunks (modules). The simplest way to split an application into modules is by application feature. If your application has a Messages feature, a Contacts feature, and a Preferences feature, each one should be bundled as a separate module. You can then use UI-Router’s lazy loading capabilities to load the feature module when the user activates it.

![img](https://ui-router.github.io/assets/guide-lazyloading/modular-bundles.png)Modular app bundles

A feature module contains the code which implements the feature. For the Contacts Feature Module, this may include:

- States
  - Top level state, e.g., `contacts`
  - Nested states, e.g., `contacts.list`, `contacts.edit`, `contacts.view`
- Routed components
  - The view(s) for the states, e.g., `ListContacts`, `ViewContact`, `EditContact`
- Non-routed components/directives
  - Any additional components which implement the feature, e.g., `ContactProfileImage`
- Any services or other code necessary to implement the feature

### Future States

A UI-Router application is structured as a tree of states.

[img of state tree]

An entire branch of the state tree (i.e., a module) can be lazy loaded. In this example, the `contacts` state and all its child states will be lazy loaded.

Instead of defining the `contacts` states tree when the application bootstraps, a placeholder for `contacts` and all its children is created. This placeholder is called a “Future State”.

A future state is any state which ends in a glob double-wildcard (`.**`). The future state for the contacts feature module is named `contacts.**`.

```
var contactsFutureState = {
  name: 'contacts.**',
  url: '/contacts',
  lazyLoad: function() {
    // lazy load the contacts module here
  }
}

```

The URL for a future state has an implicit wildcard on the end. The example `url: '/contacts'` will match on both `/contacts` and `/contacts/123/abc/`.

When defining the future state placeholder, only include the minimum details necessary to load the module. You should include the `name:`, the `url:`, and the instructions to fetch and load the module. Do not include other properties such as `resolve` or `onEnter`. Those properties should be placed on the full state, inside the lazy loaded module.

[img of future state tree]

When a user activates a future state (by clicking a ui-sref, etc), UI-Router invokes the `lazyLoad` function. The `lazyLoad` function should load the module’s javascript bundle, including the feature’s components, states, services, etc. UI-Router then *replaces the future state placeholder* with the *fully loaded state tree*.

Now that the module and its states are fully loaded, the original transition is re-run and the correct state is activated.

## Framework specifics

### Angular

Angular has its own module system and requirements around lazy loading them. It also has some limitations related to Ahead of Time compilation.

To use future states with Angular, your root module should provide the `SystemJsNgModuleLoader` using the `NgModuleFactoryLoader` token.

```
import { NgModule, NgModuleFactoryLoader, SystemJsNgModuleLoader } from '@angular/core';
@NgModule({
  ...
  providers: [
    { provide: NgModuleFactoryLoader, useClass: SystemJsNgModuleLoader }
  ]
  ...
})

```

Providing SystemJsNgModuleLoader

Your feature module should be an `NgModule` which imports `UIRouterModule.forChild()`. The `NgModule should be exported from the feature module’s entry point source file. See [contacts.module.ts](https://github.com/ui-router/sample-app-angular/blob/b13c58de5952acb38da2200748b8076876df6456/src/app/contacts/contacts.module.ts) in the sample app.

```
@NgModule({
  imports: [ UIRouterModule.forChild({ states: CONTACTS_STATES }), /* ... and any other modules */ ],
  declarations: [ ContactComponent, /* ... and any other components */ ],
  providers: [ ContactsDataService ],
})
export class ContactsModule { }

```

Exporting an NgModule

An `NgModule` requires some special processing to wire it into your app. This processing is handled automatically by UI-Router for Angular when you use the `loadChildren` property instead of `lazyLoad` in your future state.

When a `loadChildren` property is found, UI-Router builds the `lazyLoad` function with special processing and assigns it to the state. For more details on `loadChildren`, see the [`loadChildren` docs](https://ui-router.github.io/ng2/docs/latest/interfaces/state.ng2statedeclaration.html#loadchildren).

```
export const contactsFutureState = {
  name: 'contacts.**',
  url: '/contacts',
  loadChildren: () => System.import('./contacts/contacts.module').then(mod => mod.ContactsModule)
};

```

loadChildren example (JIT)

```
export const contactsFutureState = {
  name: 'contacts.**',
  url: '/contacts',
  loadChildren: () => System.import('./contacts/contacts.module').then(mod => mod.ContactsModuleFactory)
};

```

loadChildren example (AoT)

The Angular-CLI automatically switches between JIT and AoT modes using the `@ngtools/webpack` project. To leverage this functionality with UI-Router, set `loadChildren` to a string which represents the path to the source file, and the name of the `NgModule`. See [app.states.ts](https://github.com/ui-router/sample-app-angular/blob/b13c58de5952acb38da2200748b8076876df6456/src/app/app.states.ts#L87-L93) in the sample app.

```
export const contactsFutureState = {
  name: 'contacts.**',
  url: '/contacts',
  loadChildren: './contacts/contacts.module#ContactsModule'
};

```

loadChildren example (Angular-CLI or `@ngtools/webpack`)

### AngularJS

AngularJS doesn’t officially support lazy loading of components, services, etc. However, third party libraries such as [ocLazyLoad](https://oclazyload.readme.io/) provide this functionality.

**Lazy loading components**

Lazy loading components is very simple with UI-Router and ocLazyLoad. The component is loaded and registered with ocLazyLoad, then the state is activated.

```
var state = {
  name: 'contacts',
  url: '/contacts',
  component: 'contacts'
  lazyLoad: function ($transition$) {
    return $transition$.injector().get('$ocLazyLoad').load('./contacts.component.js');
  }
}

```

Lazy loading a component

**Lazy loading Future States**

Your future state’s `lazyLoad` function tells ocLazyLoad to load the module bundle.

```
var futureState = {
  name: 'contacts.**',
  url: '/contacts',
  lazyLoad: function ($transition$) {
    return $transition$.injector().get('$ocLazyLoad').load('./contacts.module.js');
  }
}

```

Lazy loading future states

Your AngularJS feature module should register all states and components as you normally would.

```
$stateProvider.state({
  parent: 'app',
  name: 'contacts',
  url: '/contacts',
  component: 'contactsComponent',
  resolve: [
    // Resolve all the contacts.  The resolved contacts are injected into the controller.
    { token: 'contacts', deps: [ContactsDataService], resolveFn: getAllContacts },
  ],
  data: { requiresAuth: true },
});

$stateProvider.state({...

```

Registering lazy loaded states

**Webpack**

Webpack users should use `System.import()` (or just `import()`) in combination with ocLazyLoad. When the module is loaded, pass the AngularJS module object to `$ocLayLoad.load()`; See [the sample app](https://github.com/ui-router/sample-app-angularjs/blob/ac107905c6eba60aca4229f0648102c33b3ee128/app/main/app.states.js#L88-L97) for an example.

```
var futureState = {
  name: 'contacts.**',
  url: '/contacts',
  lazyLoad: function ($transition$) {
    var $ocLazyLoad = $transition$.injector().get('$ocLazyLoad');
    return System.import('./contacts.module.js').then(mod => $ocLazyLoad.load(mod.CONTACTS_MODULE));
  }
}

```

Chaining `System.import` and `$ocLazyLoad.load()`

### React

The future state’s `lazyLoad` function should load the module code. See [main/states.js](https://github.com/ui-router/sample-app-react/blob/545770dad74b4956f394994d42808a5111b48e07/src/main/states.js#L84-L90) in the sample app.

```
var futureState = {
  name: 'contacts.**',
  url: '/contacts',
  lazyLoad: () => System.import('./contacts.module.js'),
  }
}

```

Lazy loading the module

The module should export a variable `states` which contains the module’s states (as an array). UI-Router will register those states before retrying the transition. See [contats/states.js](https://github.com/ui-router/sample-app-react/blob/545770dad74b4956f394994d42808a5111b48e07/src/contacts/states.js#L73) in the sample app.

```
const contactsState = {
  ...
}
const viewContactState = {
  ...
}
const editContactState = {
  ...
}
const newContactState = {
  ...
}
export const states = [contactsState, viewContactState, editContactState, newContactState];

```

Exporting the states as an array







https://ui-router.github.io/ng1/docs/latest/classes/transition.transitionservice.html

https://ui-router.github.io/ng2/docs/latest/interfaces/state.ng2statedeclaration.html#loadchildren

https://ui-router.github.io/ng1/docs/latest/interfaces/ng1.ng1viewdeclaration.html

https://ui-router.github.io/guide/ng1/route-to-component