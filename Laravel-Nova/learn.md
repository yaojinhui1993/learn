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

    - Whether display the resource in sidebar

        - `public static $displayInNavigation=<bool>`

#### Questions and Tips

- [x] where are Laravel service providers registered?

  - `app` bootstrap some core providers: `EventServiceProvider`, `LogServiceProvider`, `RoutingServiceProvider`
  - In `kernel` bootstrap with `bootstrappers`. In `bootstrappers` contain `RegisterProviders` that register all configured providers.
  - The configured providers register sequence:

    1. Provider name starts with `Illuminate\\`.
    2. In packageManifest providers (composer).
    3. Other providers in the `app.php` config file.

- [x] `/nova` path how to render page?

  - `Laravel/Nova/NovaCoreServiceProvider`
  - `app/ServiceProvider/NovaServiceProvider`

- [x] Why resource default display in sidebar?

  - The resource default property `displayInNavigation=true`

  - In page `/nova`, why the router-link props `to="/"`, and the vue only push the url to `/nova/` ?

    - Because of the `base` properties in `$routers`, and the router base is set at `router/index.js`

- [X] How to display the `helpCard.vue`?
  - Route to dashboard. Because of the Vue router. When router link is active, then the link corresponds component will be render.
  - Send api and get the needed components resources and then Vue render the `help card`.
  - The main relative components are in the list

    - `Dashboard.vue`
    - `Cards.vue`
    - `CardWrapper.vue`
    - `HelpCard.vue`
  - The main relative js file ar in the list
    - `app.js`
    - `Nova.js`
    - `laravel-nova@HasCards` mixins
    - `laravel-nova@CardSizes` methods
- [X] Vue how to use mixin functions?
  - The merge syntax likes `mixins: [myMixins]`
  - Data object merge conflict: Component's data takes priority
  - Hook functions merge conflict: Mixins hooks call before component hooks

- [X] Where is `Nova.request()` method comes from?

  In `Nova.js` file, it has the `request` method.

- [X] What is the snippet means?

  ```javascript
  ;(function() {

  }.call(window))
  ```

  The syntax likes jQuery

- [X] When register the route `/nova-api/cards`, and what is it return?

  - In `Laravel\Nova\NovaServiceProvider@registerRoutes` method, register the route.
  - The route resolve in `DashCardController@index`.

- [X] What is `<component />` tag in Vue?

    Used for [dynamic component](https://vuejs.org/v2/guide/components.html#Dynamic-Components)

- [X] When the `help` card is set in Nova instance in server?

   When laravel application boot `App\Providers\NovaServiceProvider` provider

- [X] The `user` resource how to load?

  - Server resolves route `nova/resources/users` in `Laravel\Nova\Http\Controllers\RouterController@show`
  - And response the `nova::router` view page.
  - then, Vue take the path and load the component: `ResourceIndex`.

    - Does the VueRouter automatic take the path and show the corresponds component? Yes. This is the VueRouter responsibility
    - Where set the `ResourceIndex` component props?
        Because it passes props to the component by [Boolean mode](https://router.vuejs.org/guide/essentials/passing-props.html#boolean-mode)
    - In `router.blade.php`, the `<loading></loading>` component when and how to work? We page loaded, and `Nova` instance is constructed.
    - What is the `<loading-view></loading-view>` component functions? When page is loading, then show the`<loader></loader>` component. Else show the slot
    - What is the `<loader></loader>` component functions? Just a loading SVG
    - When `<create-resource-button></create-resource-button>` path is set? In `routes.js` defined the named route, and in the button set the route parameters.

  - Component get the resource name
  - Component use the name to get the resource, cards, lenses, actions, count, and filters.

    - The method set the hook method `created` to async method, and await `getResources`, `getAuthorizationToRelate`, `getLenses`, `getActions`, `getFilters` methods to get data.
    - `getResources` handled by `ResourceIndexController@handle` method. More detail at the bottom.

## Search

## Filters

## Lenses

## Actions

## Metrics

## Customization
