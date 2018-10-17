# Nova Source code learning

    Learn code along with [nova.laravel.com](https://nova.laravel.com/docs)

## Getting Started

    - Install Nova
        - via zip file
        - via composer
        - Install commands

            ```shell
                php artisan nova:install
                php artisan migrate
            ```
    - Authorize Nova

        `NovaServiceProvider@gate` method

    - Update Nova

        ```shell
            php artisan nova:publish --force
            php artisan view:clear
        ```
    - Logging Nova

        - Visit `/nova` path why redirect to `/nova/login` when user not login?

            - When visit `/nova`, the `Nova\Http\Middleware` will check the user authentication, and throw `Nova\Exceptions\AuthenticationException` and then Laravel catch the exception, render it and redirect to `/nova/login`.

        - Visit `/nova/login` path why redirect to `/nova` path when user login.?

            - Because of the alias middleware `RedirectIfAuthenticated`, it will check the user wether login or not, if login, redirect to `/nova` path. And the route has been added the middleware.

        - How to render `/nova/login` page?
            - The route page is a regular HTML page.

        - When I open `route:list` in vscode, and open xdebug, it will automatic stop in breakpoint. Because the command will trigger by schedule.

        - Because of the `/nova` route register in event listener, so the route do not appear in the `route:list` command

    - Logout Nova

        - Like common Laravel logout.

    - Authentication

        - It just like common Laravel authentication.

    - Authorize Nova

        - `NovaServiceProvider@gate` method define the gate who can visit the nova page.

## Resources

### The Basics

    - Defining Resources

        - Command: `php artisan nova:resource <resource|Post>`

    - Binding resource to model

        - `public static $model="App\Post"`

    - Registering Resources
        Overriding `NovaServiceProvider@resouces`
        The two ways:

        1. `Nova::resourcesIn(<directory_path>)`
        2. `Nova::resources(<array>)`

    - Wether display the resource in sidebar

        - `public static $displayInNavigation=<bool>`

#### Question

    - Where are Laravel service providers registered?

        - `app` bootstrap some core providers: `EventServiceProvider`, `LogServiceProvider`, `RoutingServiceProvider`
        - In `kernel` bootstrap with `bootstrappers`. In `bootstrappers` contain `RegisterProviders` that register all configured providers.
        - The configured providers register sequence:

            1. Provider name starts with `Illuminate\\`.
            2. In packageManifest providers (composer).
            3. Other providers in the `app.php` config file.


    - `/nova` path how to render page?

        - `Laravel/Nova/NovaCoreServiceProvider`
        - `app/ServiceProvider/NovaServiceProvider`

    - Why resource default display in sidebar?

        - The resource default property `displayInNavigation=true`

    - In page `/nova`, why the router-link props `to="/"`, and the vue only push the url to `/nova/` ?

        - Because of the `base` properties in `$routers`, and the router base is set at `router/index.js`

    - How to display the `helpCard.vue`?
        - Because of the Vue router. When router link is active, then the link corresponds component will be render.

## Search

## Filters

## Lenses

## Actions

## Metrics

## Customization
