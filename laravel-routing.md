
<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


# Laravel Routing

```
Route::get('foo', function () {
    return 'Hello World';
});
````

### Route With Controller
```
  Route::get('/user', 'UserController@index');
  Route::get($uri, $callback);
  Route::post($uri, $callback);
  Route::put($uri, $callback);
  Route::patch($uri, $callback);
  Route::delete($uri, $callback);
```

### View Route 
If your route only needs to return a view, you may use the Route::view method. Like the redirect method, this method provides a simple shortcut so that you do not have to define a full route or controller.
```
  Route::view('/welcome', 'welcome');
  Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
```
### Optional Parameter 
- add ? 'question mark' with the parameter
```
  Route::get('user/{name?}', function ($name = null) {
      return $name;
  });
  Route::get('user/{name?}', function ($name = 'John') {
      return $name;
  });
```

### Name Route 
- You can set name of a route
```
  Route::get('user/profile', function () {
      //
  })->name('profile');
  Route::get('user/profile', 'UserProfileController@show')->name('profile');
```

### Middleware 
- You can protect the route using middleware
```
Route::middleware(['first', 'second'])->group(function () {
    Route::get('user/profile', function () {
        // Uges first & second Middleware
    });
});
```

### Route Prefix 
```
Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});
```

### Resource Route 
```
Route::resource('route_name', 'ControllerName');
```


### Api Routes 
```
Route::get('category-list', 'API\CategoryController@category_list');
Route::apiResources(['user' => 'API\UserController']);
```

### Accessing The Current Route 
```
$route  = Route::current();
$name   = Route::currentRouteName();
$action = Route::currentRouteAction();
```
