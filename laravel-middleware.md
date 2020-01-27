<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


# Defining Middleware

- Go to project directory.
- Open terminal, </br>
- Go ahead and create one for Post as well:

```php 
php artisan make:middleware AuthorMiddleware
```

Now open the ```App\Http\Middleware\AuthorMiddleware.php``` file and pasted the code

```php
 
namespace App\Http\Middleware;
 
use Closure;
use Illuminate\Support\Facades\Auth;
 
class AuthorMiddleware
{
    public function handle($request, Closure $next)
    {
	 // set your condition, if fillup condition then continue target request, in my case the user is author whose role_id is 2  
        if (Auth::check() && Auth::user()->role_id == 2)  {
            return $next($request);
        } else {
	    // if false condition then redirect to login or another where you want to send
            return redirect()->route('login');
        }
    }
}
```


### Registering Middleware
- Got to `App\Http\Kernel.php` and the line 

```php
'author' => AuthorMiddleware::class,
```


use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        $this->call(PostTableSeeder::class);
    }
}

```

- Go to terminal and run the commund.

```php
php artisan db:seed
```


### Assigning Middleware To Routes
- Got to `routes\web.php`
```
Route::get('admin/profile', function () {
    //
})->middleware('short_accessible_name');
```
- Or When assigning middleware, you may also pass the fully qualified class name
```
Route::get('admin/profile', function () {
    //
})->middleware(MiddlewareName::class);
```

- You may also assign multiple middleware to the route:
```
Route::get('/', function () {
    //
})->middleware('first_middleware', 'second_middleware');
```
- Middleware Groups
```
Route::middleware(['middleware1', 'middleware2', 'middleware3'])->group(function () { 
    // list of route you can set here
});
```
