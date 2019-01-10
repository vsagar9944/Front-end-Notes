https://www.youtube.com/watch?v=VS6vDlsqW7o



# Difference Between Factory,Service and Provider

So the main difference between factory and service is that factory returns some object, while the service creates new instance and returns "this"



In fact $provide.provider, $provide.factory and $provide.service are more or less the same thing in the sense that all of them are  blueprints / instructions for creating object instances (those  instances are then ready to be injected into collaborators). 

$provide.provider is the most spohisticated method of registering blueprints, it allows you to have a complex creation function and  configuration options. 

$provide.factory is a simplified version of $provide.provider when you don't need to support configuration options but still want to have a more sophisticated creation logic. 

$provide.service is for cases where the whole creation logic boils down to invoking a constructor function. 



**Services**

Syntax: module.service( 'serviceName', function );

Result: When declaring serviceName as an injectable argument **you will be provided the actual function reference** passed to module.service.

Usage: Could be useful for sharing utility functions that are useful to invoke by simply appending () to the injected function reference. Could also be run with injectedArg.call( this ) or similar.

**Factories**

Syntax: module.factory( 'factoryName', function );

Result: When declaring factoryName as an injectable argument **you will be provided the value that is returned by invoking the function reference** passed to module.factory.

Usage: Could be useful for returning a 'class' function that can then be new'ed to create instances.

**Providers**

Syntax: module.provider( 'providerName', function );

Result: When declaring providerName as an injectable argument **you will be provided the value that is returned by invoking the $get method of the function reference** passed to module.provider.

Usage: Could be useful for returning a 'class' function that can then be new'ed to create instances but that requires some sort of configuration before being injected. Perhaps useful for classes that are reusable across projects? Still kind of hazy on this one.





But it turns out that angular only understands provide.provider, all other ones are derived.

provider.service = function(name, Class) {

  provider.provide(name, function() {

​    this.$get = function($injector) {

​      return $injector.instantiate(Class);

​    };

  });

}

provider.factory = function(name, factory) {

  provider.provide(name, function() {

​    this.$get = function($injector) {

​      return $injector.invoke(factory);

​    };

  });

}

provider.value = function(name, value) {

  provider.factory(name, function() {

​    return value;

  });

};





















# Services

Syntax: `module.service( 'serviceName', function );` 
Result: When declaring serviceName as an injectable argument **you will be provided with an instance of the function. In other words** `new FunctionYouPassedToService()`.

# Factories

Syntax: `module.factory( 'factoryName', function );` 
Result: When declaring factoryName as an injectable argument you will be provided with **the value that is returned by invoking the function reference passed to module.factory**.

# Providers

Syntax: `module.provider( 'providerName', function );` 
Result: When declaring providerName as an injectable argument **you will be provided with** `(new ProviderFunction()).$get()`. The constructor function is instantiated before the $get method is called - `ProviderFunction` is the function reference passed to module.provider.

Providers have the advantage that they can be configured during the module configuration phase.

See [here](http://jsbin.com/ohamub/1/edit) for the provided code.

Here's a great further explanation by Misko:

```
provide.value('a', 123);

function Controller(a) {
  expect(a).toEqual(123);
}
```

In this case the injector simply returns the value as is. But what if you want to compute the value? Then use a factory

```
provide.factory('b', function(a) {
  return a*2;
});

function Controller(b) {
  expect(b).toEqual(246);
}
```

So `factory` is a function which is responsible for creating the value. Notice that the factory function can ask for other dependencies.

But what if you want to be more OO and have a class called Greeter?

```
function Greeter(a) {
  this.greet = function() {
    return 'Hello ' + a;
  }
}
```

Then to instantiate you would have to write

```
provide.factory('greeter', function(a) {
  return new Greeter(a);
});
```

Then we could ask for 'greeter' in controller like this

```
function Controller(greeter) {
  expect(greeter instanceof Greeter).toBe(true);
  expect(greeter.greet()).toEqual('Hello 123');
}
```

But that is way too wordy. A shorter way to write this would be `provider.service('greeter', Greeter);`

But what if we wanted to configure the `Greeter` class before the injection? Then we could write

```
provide.provider('greeter2', function() {
  var salutation = 'Hello';
  this.setSalutation = function(s) {
    salutation = s;
  }

  function Greeter(a) {
    this.greet = function() {
      return salutation + ' ' + a;
    }
  }

  this.$get = function(a) {
    return new Greeter(a);
  };
});
```

Then we can do this:

```
angular.module('abc', []).config(function(greeter2Provider) {
  greeter2Provider.setSalutation('Halo');
});

function Controller(greeter2) {
  expect(greeter2.greet()).toEqual('Halo 123');
}
```

As a side note, `service`, `factory`, and `value` are all derived from provider.

```
provider.service = function(name, Class) {
  provider.provide(name, function() {
    this.$get = function($injector) {
      return $injector.instantiate(Class);
    };
  });
}

provider.factory = function(name, factory) {
  provider.provide(name, function() {
    this.$get = function($injector) {
      return $injector.invoke(factory);
    };
  });
}

provider.value = function(name, value) {
  provider.factory(name, function() {
    return value;
  });
};
```