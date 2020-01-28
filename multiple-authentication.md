<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


# Multiple Authentication

- First create a new laravel project.
- Then configure .env file with database.
- Then execute these commands

```
composer require laravel/ui --dev
npm install
npm run dev
php artisan ui vue --auth
```

Suppose we will create two types of authentication, admin and author. So first of all we should create or modify user table.
So open got to ```project_folder->database->migrations->_create_users_table.php```

```php 
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('name');
            $table->integer('role_id');
            $table->string('email', 191)->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

```


```txt
Modify your migration like this. Then execute this command. php artisan migrate
After migrating. Create two middleware by these commands

php artisan make:middleware AdminMiddleware 
and 
php artisan make:middleware AuthorMiddleware

Then go to 
app-->Http-->Middleware-->AdminMiddleware
```
### Then write these code


```php
 <?php
 
namespace App\Http\Middleware;
 
use Closure;
use Illuminate\Support\Facades\Auth; // carefully check this use
 
class AdminMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if (Auth::check() && Auth::user()->role_id == 1)  {
            return $next($request);
        } else {
            return redirect()->route('login');
        }
        
    }
}
```
### Then go to 
```txt
app-->Http-->Middleware-->AuthorMiddleware
```
### Then write these code

```php
<?php
 
namespace App\Http\Middleware;
 
use Closure;
use Illuminate\Support\Facades\Auth;
 
class AuthorMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if (Auth::check() && Auth::user()->role_id == 2)  {
            return $next($request);
        } else {
            return redirect()->route('login');
        }
    }
}
```

#### Then go to Middlleware directory and open `RedirectIfAuthenticated.php` file and change by this code
```php
  public function handle($request, Closure $next, $guard = null)
  {
      if (Auth::guard($guard)->check() && Auth::user()->role_id == 1) {
          return redirect()->route('admin.dashboard');
      } if (Auth::guard($guard)->check() && Auth::user()->role_id == 2) {
          return redirect()->route('author.dashboard');
      } else {
          return $next($request);
      }
  }
```

#### Go to `app->Http->Controllers->Auth->LoginController.php`, and pasted this code
```php
protected $redirectTo;   
 
    public function __construct()
    {
        if (Auth::check() && Auth::user()->role_id == 1) {
            $this->redirectTo = route('admin.dashboard');
        } else {
            $this->redirectTo = route('author.dashboard');
        }
        $this->middleware('guest')->except('logout');
    }
```
#### Open RegisterController.php and pasted this code
```php
    protected $redirectTo;    
    
    public function __construct()
    {
        if (Auth::check() && Auth::user()->role_id == 1) {
            $this->redirectTo = route('admin.dashboard');
        } else {
            $this->redirectTo = route('author.dashboard');
        }
 
        $this->middleware('guest');
    }
```

#### ResetPasswordController.php
```php
    protected $redirectTo;
    
    public function __construct()
    {
        if (Auth::check() && Auth::user()->role_id == 1) {
            $this->redirectTo = route('admin.dashboard');
        } else {
            $this->redirectTo = route('author.dashboard');
        }
 
        $this->middleware('guest');
    }
```
#### Go to `app\Http\Kernel.php` and register admin and author middleware, into `$routeMiddleware` array
```txt
  //  custom middlewares
  'admin' => AdminMiddleware::class,
  'author' => AuthorMiddleware::class,
```
#### And then go to `web.php` file and write this code
```php
<?php
// public routes
Route::get('/', function () {
    return view('welcome');
});
Auth::routes();
Route::get('/home', 'HomeController@index')->name('home');
 
 
// routes for admins
Route::group(['prefix'=>'admin', 'as'=>'admin.', 'namespace'=>'Admin', 'middleware'=>['auth', 'admin']], function() {
    Route::get('dashboard', 'DashboardController@index')->name('dashboard');
});
 
 
// routes for authors
Route::group(['prefix'=>'author', 'as'=>'author.', 'namespace'=>'Author', 'middleware'=>['auth', 'author']], function() {
    Route::get('dashboard', 'DashboardController@index')->name('dashboard');
});
```

