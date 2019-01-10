# Decorators
Decorators are functions that are invoked with a prefixed @ symbol, and immediately followed by a class, parameter, method or property.The decorator function is supplied information about the class, parameter or method, and the decorator function returns something in its place, or manipulates its target in some way.

Decorators are functions, and there are four things (class, parameter, method and property) that can be decorated; consequently there are four different function signatures for decorators:

- class: `declare type ClassDecorator = <TFunction extends Function>(target: TFunction) => TFunction | void;`
- property: `declare type PropertyDecorator = (target: Object, propertyKey: string | symbol) => void;`
- method: `declare type MethodDecorator = <T>(target: Object, propertyKey: string | symbol, descriptor: TypedPropertyDescriptor<T>) => TypedPropertyDescriptor<T> | void;`
- parameter: `declare type ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number) => void;`

# Property Decorators

Property decorators work with properties of classes.

```
function Override(label: string) {
  return function (target: any, key: string) {
    Object.defineProperty(target, key, { 
      configurable: false,
      get: () => label
    });
  }
}

class Test {
  @Override('test')      // invokes Override, which returns the decorator
  name: string = 'pat';
}

let t = new Test();
console.log(t.name);  // 'test'
```

To create your own component that supports two-way binding, you must define an `@Output` property to match an `@Input`, but suffix it with the `Change`. 



No code is needed within the class to tell Angular that it is a component or a module. All we need to do is decorate it, and Angular will do the rest.



**Meta data for @Component**

```
{
  selector: undefined,
  inputs: undefined,
  outputs: undefined,
  host: undefined,
  exportAs: undefined,
  moduleId: undefined,
  providers: undefined,
  viewProviders: undefined,
  changeDetection: ChangeDetectionStrategy.Default,
  queries: undefined,
  templateUrl: undefined,
  template: undefined,
  styleUrls: undefined,
  styles: undefined,
  animations: undefined,
  encapsulation: undefined,
  interpolation: undefined,
  entryComponents: undefined
}
```

