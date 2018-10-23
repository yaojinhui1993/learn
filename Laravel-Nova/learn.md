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

### Fields

- [X] How display field in index page?
  - Add the field in the resource model.
  - When client request the resource api, then server resolved the correct format, and return.
  - And client request the resource count.
  - Then client render it.

- [X] Server side how to get the property field for a specified resource?

  - In `ResourceIndexController@handle` method, call resources `serializeForIndex` method
  - Then call trait `ResolveFields@indexFields` method to get current resource fields

- [X] What is the means of `this.$watch` in Vue?

    Watch a expression or function if changed, then act some functions.

## Search

- [X] The global search how to work?

  1. Every page load the `<global-search></global-search>` component
  2. When user search something, request `nova-api/search` method to search.
  3. Get the data, and search result and render.

- [x] How does server do global search?

  1. Filter the can global search resources
  2. Loop the resources to search the resource searchable columns.
  3. Format the response data.

- [X] How does it integrates with Scout?

  If the resource's model uses scout, them it does.

- [X] What is elasticsearch?

  Search Engine

- [X] How to integrate elasticsearch with Laravel?

  Install Laravel scout, and install elasticsearch extends package.

## Filters

- [X] What is the `Filters`?

  Filter data, scope index queries with custom conditions.

- [X] How to make filters work?

  1. Create a filter file: `php artisan nova:filter <FilterType>`
  2. Implement  `apply` and `options` method in the create file.
  3. Register the filter instance to the specified resource.

- [X] How does client trigger send filter request?

  - User select the filter items
  - `<filter-selector></filter-selector>` component trigger change event
  - The context listen the event, update the `filterParameter`
  - The watcher see the `filterParameter` change, send request

- [x] How does server filter the result?

  - When build request quires, apply the filters.

- [X] In `slot-scope`, child component can be use the parent component's methods?

  Yes, this is why `slot-scope` comes in. When define slot, it can pass the slot anything dat to relevant its context, even function.

## Lenses

- [x] What is lenses?

  It can **fully** customize the underlying resource Eloquent query builder.
  So, can select everything you want.

- [X] How does lenses work?

  1. Generate lenses file: `php artisan nova:lens <FileName>`
  2. Finished `query` and `fields` methods in the created file.
  3. Register the file in target resource.

- [X] How does client work with lenses?

  1. When page resource page, client will request the resource's lenses.
  2. Client display the lenses in the dropdown component that contain a router-link to `lens` component
  3. When user visit the router, Vue load the component.
  4. The lens component request `/{resource}/lens/{lens}` to get lens
  5. Show the data.

- [X] How does server response the lens request?

  1. Get current lens
  2. Call the lens `query` method, and get the eloquent builder.
  3. Get the data from database
  4. Format data and response.

## Actions

- [X] What is actions?

  Nova actions allow you to perform custom tasks.

- [X] How does client deal with actions?

  When resource index page created, it will fetch the resource actions. If has actions, will display the actions.

- [x] How does server work with actions request?

  Server receive the request, get the resource actions, and return to client.

- [x] How to make custom actions?

  1. Create actions file: `php artisan nova:action <FileName>`
  2. Implement the created file's `handle` method.
  3. Register the file to resource in `actions` properties.
  4. Done.

- [x] What is action's `field` properties use?

  Gather extra information from the user before dispatching an action.

- [X] How to implement queue actions?

  Just implement the `ShouldQueue` contracts.

- [X] How to active `action log`?

  Let the user eloquent model use `actionable` trait.

- [x] What is `pivot actions` ?

  In `belongsToMany` relationships, so that can operate on pivot/intermediate table records.

## Metrics

## Customization
