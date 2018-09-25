# Architecture Concepts

## Request Life Cycle

A good, high-level overview of how the Laravel work.

1. First Thing

    The entry point for all requests to the Laravel is the `public/index.php`

    1. Register the Auto Loader
    2. Retrieves an instance of the Laravel application
        1. Create the Application
            * set base path

                > `path`, `path.base`, `path.lang`, `path.config`, `path.public`, `path.storage`, `path.database`, `path.resources`, `path.bootstrap`
            * Register the basic bindings

                * Set the shared instance of the container: `Container::$instance = Application::class`
                * Bind `app` and `Container::class` to `Application::class` instance
                * Bind `PackageManifest::class` to `PackageManifest::class` instance
            * Register all of the base service provider

                * `EventServiceProvider`
                * `LogServiceProvider`
                * `RoutingServiceProvider`
            * Register the core class alias in the container

                > `app`, `auth`, `auth.driver`, `blade.compiler`, `cache`, `cache.store`, `config`, `cookie`, `encrypter`, `db`, `db.connection`, `events`, `files`, `filesystem`, `filesystem.disk`, `filesystem.cloud`, `hash`, `hash.driver`, `translator`, `log`, `mailer`, `auth.password`, `auth.password.broker`, `queue`, `queue.connection`, `queue.failer`, `redirect`, `redis`, `request`, `router`, `session`, `session.store`, `url`, `validator`, `view`
        2. Bind Important Interfaces

            * Http Kernel
            * Console Kernel
            * Exception Handler
    3. Run the application

2. HTTP Kernels

    1. Create Kernel instance

        * Inject `app`, and `Router` in constructor
        * Set `Router->middlewarePriority` properties

            > `StartSession`, `ShareErrorFromSession`, `Authenticate`, `AuthenticateSession`, `SubstituteBindings`, `Authorize`
        * Set `Router->middlewareGroup` properties

            > The Default: `web` and `api`

        * Set `Router->aliasMiddleware` by `routeMiddleware`

            > The Default: `auth`, `auth.basic`, `bindings`, `cache.headers`, `can`, `guest`, `signed`, `throttle`, `verified`
    2. The kernel instance's handle the request instances and return the response

        * Send the given request through the middleware / router

            * Bind request instance to `request` in the Application
            * Boot Application with `bootstrappers`

                > Default bootstrappers contains: `LoadEnvironmentVariables`, `LoadConfiguration`, `HandleExceptions`, `RegisterFacades`, `RegisterProviders`, `BootProviders`
            * Dispatch to Route

                * Bind `request` to Application
                * Dispatch request to route

    3. Send response to user
    4. Kernel terminate with the request and response

## Service Container

### Binding Basics

1. Simple Bindings

    * Closure resolution

        ```php
        $container->bind('name', function() {
            return 'Taylor';
        });
        ```

    * Shared closure resolution

        ```php
        $container->singleton('class', function() {
            return new stdClass;
        });
        ```

    * Auto concrete resolution
    * Shared concrete resolution
    * Abstract to concrete resolution
    * Nested dependence resolution
    * Aliases
    * Aliases with array of parameters
    * Binding can be overridden
    * Extended bindings
    * Multiple extended bindings
    * Binding an instance return the instance
    * Extend instance are preserved
    * Extend is lazy initialized
    * Extend can be called before bind
    * Extend instance call rebinding callback.
    * Extend bind call rebinding callback when bind is resolved
    * Resolution of default parameters.
    * Resolving callbacks are called for specified abstract
    * Global resolving callbacks are called for all abstract
    * Resolving callbacks are called for specified type
    * Rebound listeners
    * Rebound listeners on instances
    * Call with dependencies, and wrap method

        ```php
        $result = $container->call(function ($foo = null) {
            return func_get_args();
        }, ['foo' => 'bar'])
        // return[0] = 'bar';
        ```

    * Call with `at sign` based class references

        ```php
        // 1
        $container->call('CallStub@work', ['default' => 'foo']);
        // 2
        $container->call('CallStub', ['default' => 'foo'], 'work');
        // 3
        $container->call([new CallStub, 'work'], ['default' => 'foo']);
        ```

    * Call with static method

        ```php
        $container->call('CallStaticStub::work');
        ```

    * Call with global method

        ```php
        $container->call('CallGlobalFuncStub', ['default' => 'foo']);
        ```

    * Call with bound method

        ```php

        // 1
        $container->bindMethod('CallStub@unresolvable', function($stub, $container) {});
        // 2
        $container->bindMethod([new CallStub, 'unresolvable'], function($stub, $container) {});

        // 1
        $container->call('CallStub@unresolvable'); // use the bound method.
        // 2
        $container->call([new CallStub, 'unresolvable']);
        ```

    * Container can inject different implementations depending on context
    * Container tags

        ```php
        // tag
        // 1.
        $container->tag('ImplementationStub', 'foo', 'bar');
        // 2.
        $container->tag('ImplementationStub', ['foo', 'bar']);
        // 3.
        $container->tag(['ImplementationStubOne', 'ImplementationStubTwo'], 'foo')

        // Resolve tag
        $container->tagged('foo');
        ```

    * Forget instance and forget instances
    * Flush containers
    * Container can inject simple variable

        ```php
        $container->when('InjectVariableStub')->needs('$something')->give(100);
        ```

    * Container factory
    * `makeWith` is an alias for `make` method

## Service Provider

* `FoundationApplicationTest`

  * Service provider `bind` and `singleton` properties can be automatic resolve when it is registered.
  * Deferred service don't run when instance set.
  * Single provider can provide multiple deferred services

* `SupportServiceProviderTest`

## Facades

* `SupportFacadeTest`
* `SupportFacadeEventTest`