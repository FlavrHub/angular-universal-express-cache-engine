# Angular Universal Express Cache Engine

Express enging that catches the request by the url so that the request doesn't have to go through all the Angular
Universal each time. Really handy to cache static pages.

Uses the [cache-manager](https://github.com/BryanDonovan/node-cache-manager) package for catching, which can be used with a bunch of different engines, checkout it documentation for futher information.

## Instalation

Just place the [express-engine.ts](./express-engine.ts) file in your project and in your server.ts replace:

```typescript
server.engine('html', ngExpressEngine({
    bootstrap: AppServerModule,
 }));
```

with

```typescript
server.engine('html', ngFlavrHubExpressEngine({
    bootstrap: AppServerModule,
    routeCaches
}));
```

And at the beginning of it specify the routes you want to catch:

```typescript
const routeCaches = [
    {path: '/', ttl: 86400},
    {path: '/explore-recipes', ttl: 86400, useQueryParams: true}
];
```

## Routes configuration

Each route configuration implements the following interface:

```typescript
export interface RouteCache {
  path: string;
  useQueryParams?: boolean;
  ttl?: number;

  isCacheableValue?(path: any, req: Request): boolean;
}
```

* path: The path you want to cache starting with `/`, for example for the home page will be `/`
* useQueryParams: Whether the query params of the url should be taken in account on catching. Default: false.
* ttl: Time to live in cache in seconds. Default: 60 seconds.
* isCacheableValue: A function that decide if a value is cacheable on the fly

## Results

### Before catching

<img src="./before.jpg">

### After catching

<img src="./after.jpg">

