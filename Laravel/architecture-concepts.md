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