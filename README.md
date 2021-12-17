# Mezzio tips

### mezzio/mezzio-hal
You are working with [Doctrine](https://github.com/doctrine) and [Paginators](https://docs.mezzio.dev/mezzio-hal/v2/doctrine/) and your ID column is [Ben Ramsey's UUID](https://github.com/ramsey/uuid), but a `fetch all` only renders an error similar to this?
```
Mezzio\Router\Exception\RuntimeException: 
Route `user.get` expects at least parameter values for [id], 
but received [userName,emailAddress,userClass] in file 
/var/www/html/vendor/mezzio/mezzio-fastroute/src/FastRouteRouter.php on line 291
```
- You double checked the metadata mapping
- You started adding identifiers
- ...nothing works!

A simple solution:
- Go into the vendor directory -> vendor\mezzio\mezzio-hal\src\ResourceGenerator
- Copy the file `RouteBasedResourceStrategy.php`
- Create a new directory in your project module, e.g. `Hal`
- Paste the file into the new directory
- Find the codeblock with `if (! is_scalar($value)) {`
- Change `if (! is_scalar($value)) {` into `if (! is_scalar($value) && ! $value instanceof UuidInterface) {` and save it
- Open your `ConfigProvider.php` or `dependencies.global.php` and add `RouteBasedResourceStrategy::class => MyRouteBasedResourceStrategyAlias::class` to the `invokables`
