# Routing

- how to import the necessary Angular built-in APIs to implement component routing and navigation,
- how to create the routing module and import it in the main application module,
- how to create routes to components,
- how to use the router outlet to allow Angular to insert components matching a path,
- how and when to use wild card routes,
- how to use `routerLink`
- how to use nested routes
- how to get route parameters



## Creating the Angular 6 Routing Module

You can either define the routes inside the main application module or preferably on its own module

```
import { RouterModule, Routes } from '@angular/router';
```

You also need to import `ModuleWithProviders` which allows you to create a module (`NgModule`) with its providers.



# [Angular 2 Routing - ModuleWithProviders v NgModule](https://stackoverflow.com/questions/45825360/angular-2-routing-modulewithproviders-v-ngmodule)

ModuleWithProviders example:

```
import { ModuleWithProviders } from '@angular/core';
import {Routes, RouterModule} from '@angular/router';

import { AppComponent } from './app.component';
import { AboutComponent } from './about/about.component';
import { ServicesComponent } from './services/services.component';

export const router: Routes = [
    { path: '', redirectTo: 'about', pathMatch: 'full'},
    { path: 'about', component: AboutComponent },
    { path: 'services', component: ServicesComponent }
];

export const routes: ModuleWithProviders = RouterModule.forRoot(router);
```

NgModule Example:

```
import { NgModule } from '@angular/core';
import {Routes, RouterModule} from '@angular/router';

import { AppComponent } from './app.component';
import { AboutComponent } from './about/about.component';
import { ServicesComponent } from './services/services.component';

export const router: Routes = [
    { path: '', redirectTo: 'about', pathMatch: 'full'},
    { path: 'about', component: AboutComponent },
    { path: 'services', component: ServicesComponent }
];

@NgModule({
  imports: [
    RouterModule.forRoot( router )
  ],
  exports: [
    RouterModule
  ]
})

export class AppRoutingModule {}
```

NgModule and ModuleWithProviders are different things.

`NgModule` is a decorator for module classes.

[`ModuleWithProviders` is the interface](https://angular.io/api/core/ModuleWithProviders) that is supposed to be returned by `forRoot` method. `ModuleWithProviders` object is plain object that has `ngModule` property that contains actual module class augmented with additional providers in `providers` property.

Since `ModuleWithProviders` is an interface, its usage is optional.



```
{ path: '',  redirectTo: '/products', pathMatch: 'full' },
```

**pathMatch** is used to specify the matching strategy **full** or **prefix**. **full** means that the whole URL's path needs to match by the matching algorithm. **prefix** means the first route where path matches the start of the URL will be chosen. In the case of empty path if we don't set the **full** matching strategy then we won't get the desired behavior as any path starts with an empty path.

```
<router-outlet></router-outlet> 
```

Angular provides `routerLink` and `routerLinkActive` directives that need to be added to `<a>`anchors. 

`routerLink`: this directive is used instead of *href* in the `<a>` tags, `routerLinkActive`: this directive is used to add a CSS class to an element when the link's route becomes active.





---------------

## How to Get Route Parameters

The Angular Router provides two different methods to get route parameters:

- Using the route snapshot,
- Using Router Observables





## Navigation Using The RouterLink Directive with Parameters

```
<a [routerLink]="['/product',product.id]"></a>
```



```
import { ActivatedRoute } from '@angular/router';


 constructor(private route: ActivatedRoute) {}
 
 
 ngOnInit() {
    this.route.paramMap.subscribe(params => {
      this.products.forEach((p: Product) => {
        if (p.id == params.id) {
          this.product = p;
        }
      });
    });
  }
  
  this.route.params.subscribe((params: {id: string}) => {
    this.currentSpeaker = this.service.getSpeakerByID(params.id);
  });
```

You can also use the *snapshot* object of the `ActivatedRoute` instance: 

```
 ngOnInit() {
      this.products.forEach((p: Product) => {
        if (p.id == this.route.snapshot.params.id) {
          this.product = p;
        }
      });

  }
```

### Navigating Programatically Using Router.navigate() and Router.navigateByUrl() 





## How to Create a Named Router Outlet?

```
<router-outlet  name="outlet1"></router-outlet>
```

## How to Create Multiple Router Outlets?

```
<router-outlet></router-outlet>  
<router-outlet  name="sidebar"></router-outlet>  
```

- The unnamed outlet is the primary outlet.
- Except for the primary outlet, all other outlets must have a name.



### What is An Auxiliary Route?

A component has one primary **route** and zero or more **auxiliary** routes.. Auxiliary routes allow you to use and navigate multiple routes. To define an auxiliary route you need a named router outlet where the component of the auxiliary route will be rendered.

```
{
   path: "",
   component: SidebarComponent,
   outlet: "sidebar"
}
```

### Navigating Inside Auxiliary Outlets

You can navigate inside an auxiliary outlet by using the *outlets* property:

```
router.navigate([{outlets: {primary: 'path' ,sidebar: 'path'}}]);
```

Or also using the `routerLink` directive

```
<a [routerLink]="[{ outlets: { primary: ['path'],sidebar: ['path'] } }]">
    Products List
</a>
```



### Route Configuration Properties

- **path**: string, path to match the URL
- **patchMatch**: string, how to match the URL
  - **prefix**: default, matches when the URL **starts with** the value of `path`
  - **full**: matches when the URL **equals** the value of `path`.
- **component**: class reference, component to activate when this route is activated
- **redirectTo**: string, URL to redirect to when this route is activated
- **data**: static data to assign to route
- **resolve**: dynamic data to resolve and merge with **data** when resolved
- **children**: child routes.



### There are two ways to create a routing module:

1. `RouterModule.forRoot(routes)`: creates a routing module that includes the router directives, the route configuration **and** the router service
2. `RouterModule.forChild(routes)`: creates a routing module that includes the router directives, the route configuration **but not** the router service.

When we import a routing module that is created using `RouterModule.forRoot()`, Angular will instantiate the router service. When we import a routing module that’s created using `RouterModule.forChild()`, Angular will **not** instantiate the router service.

Therefore we can only use `RouterModule.forRoot()` once and use `RouterModule.forChild()` multiple times for additional routing modules.





To tell Angular Router where it can place components, we must add the `<router-outlet></router-outlet>` element to `AppComponent`’s HTML template.



### Adding a wildcard route

To handle unmatched URLs gracefully we need to do two things:

1. Create `PageNotFoundComponent` (you can name it differently if you like) to display a friendly message that the requested page could not be found
2. Tell Angular Router to show the `PageNotFoundComponent` when no route matches the requested URL.

```
{
    path: '**',
    component: PageNotFoundComponent
  }
```

Notice that the wildcard route must be the last route in our routing configuration for it to work as expected.

When Angular Router matches a request URL to the router configuration, it stops processing as soon as it finds the first match.



### Resolve

Angular Router uses Angular dependency injection to access resolvers, so we have to make sure we register `TodosResolver` with Angular’s dependency injection system by adding it to the `providers` property in `AppRoutingModule`’s `@NgModule`metadata:



### A route’s data can be accessed from the `ActivatedRoute` or `ActivatedRouteSnapshot`, 



To access the route data, we must import `ActivatedRoute` from `@angular/router`:

```typescript
import { ActivatedRoute } from '@angular/router';

constructor(
  private todoDataService: TodoDataService,
  private route: ActivatedRoute
) {
}

public ngOnInit() {
// Getting data from route
  this.route.data
    .map((data) => data['todos'])
    .subscribe(
      (todos) => {
        this.todos = todos;
      }
    );
}
```





## The 7-step routing process(https://www.jvandemo.com/the-7-step-process-of-angular-router-navigation/)



Every time a link is clicked or the browser URL changes, Angular router makes sure your application reacts accordingly.

To accomplish that, Angular router performs the following 7 steps in order:

1. **Parse**: it parses the browser URL the user wants to navigate to
2. **Redirect**: it applies a URL redirect (if one is defined)
3. **Identify**: it identifies which router state corresponds to the URL
4. **Guard**: it runs the guards that are defined in the router state
5. **Resolve**: it resolves the required data for the router state
6. **Activate**: it activates the Angular components to display the page
7. **Manage**: it manages navigation and repeats the process when a new URL is requested

 

To remember the 7 steps, you can use the [mnemonic](https://en.wikipedia.org/wiki/Mnemonic) **PRIGRAM**, where each letter represents a step in the routing process:

- **P**arse
- **R**edirect
- **I**dentify
- **G**uard
- **R**esolve
- **A**ctivate
- **M**anage

## Terminology

- **router service**: the global Angular router service in our application
- **router configuration**: definition of all possible router states our application can be in
- **router state**: a state of the router at some point in time, expressed as a tree of activated route snapshots
- **activated route snapshot**: provides access to the URL, parameters and data for a router state node
- **guard**: script that runs when a route is loaded, activated or deactivated
- **resolver**: script that fetches data before the requested page is activated
- **router outlet**: location in the DOM where Angular router can place activated components
- **URL segments**: parts of the URL that are divided by slashes

## Step 1 - Parse the browser URL

To parse the URL, Angular uses the following conventions:

- `/`: slashes divide URL segments
- `()`: parentheses specify [secondary routes](https://angular.io/guide/router#secondary-routes)
- `:`: a colon specifies a [named router outlet](https://angular.io/guide/router#displaying-multiple-routes-in-named-outlets)
- `;`: a semicolon specifies a [matrix parameter](https://angular.io/guide/router#heroes-list-optionally-selecting-a-hero)
- `?`: a question mark separates the [query string parameters](https://angular.io/guide/router#query-parameters-and-fragments)
- `#`: a hashtag specifies the [fragment](https://angular.io/guide/router#query-parameters-and-fragments)
- `//`: a double slash separates multiple secondary routes

 

# Dynamic Routing

The property of our interest is **Router.config** that holds information about known routes. You can populate it from the component constructor or any class method similar to the following:

```
this.router.config.unshift(
  { path: 'page1', component: Page1Component },
  { path: 'page2', component: Page2Component }
);
```

 













