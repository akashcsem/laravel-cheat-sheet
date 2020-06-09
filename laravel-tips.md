<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>

1. Single Action Controllers

```php
  <?php
    namespace App\Http\Controllers;
    use App\User;
    use App\Http\Controllers\Controller;
    class ShowProfile extends Controller
    {
      public function __invoke($id)
      {
        return view('user.profile', ['user' => User::findOrFail($id)]);
      }
    }
    Routes:
      Route::get('user/{id}', 'ShowProfile');
```

2. OrderBy on Eloquent relationships
<p>You can specify ​ orderBy()​ directly on your Eloquent relationships.</p>

```php
    public function products()
    {
      return $this->hasMany(Product::class);
    }
    
    public function productsByName()
    {
      return $this->hasMany(Product::class)->orderBy('name');
    }
```

3. Raw DB Queries: havingRaw()
<p>You can use RAW DB queries in various places, including ​ havingRaw()​ function after groupBy()​.</p>

```php
    Product::groupBy('category_id')->havingRaw('COUNT(*) > 1')->get();
```

4. Eloquent where date methods
<p>In Eloquent, check the date with functions ​ whereDay()​ , ​ whereMonth()​ , ​ whereYear()​ ,
whereDate()​ and ​ whereTime()​.</p>

```php
    Product::whereDate('created_at', '2018-01-31')->get();
    Product::whereMonth('created_at', '12')->get();
    Product::whereDay('created_at', '31')->get();
    Product::whereYear('created_at', date('Y'))->get();
    Product::whereTime('created_at', '=', '14:13:58')->get();
```
5. Increments and decrements
<p>if you want to increment some DB column in some table, just use ​ increment()​ function. Oh,
and you can increment not only by 1, but also by some number, like 50.</p>

```php
    Post::find($post_id)->increment('view_count');
    User::find($user_id)->increment('points', 50);
```
6. Does view file exist?
<p>You can check if View file exists before actually loading it.</p>

```php 
    if (view()->exists('custom.page')) {
      // Load the view
    }
```

7. No timestamp columns
<p>If your DB table doesn't contain timestamp fields ​ created_at​ and ​ updated_at​ , you can
specify that Eloquent model wouldn't use them, with ​ $timestamps = false​ property.</p>

```php 
    class Company extends Model
    {
      public $timestamps = false;
    }
```

8. Migration fields with timezones
<p>Did you know that in migrations there's not only ​ timestamps()​ but also ​ timestampsTz()​ , for
the timezone?</p>

```php
    Schema::create('employees', function (Blueprint $table) {
    $table->increments('id');
    $table->timestampsTz();
    });
```
<p>Also, there are columns ​ dateTimeTz()​ , ​ timeTz()​ , ​ timestampTz()​ , ​ softDeletesTz()​.</p>

9. Eloquent has() deeper
  <p>You can use Eloquent ​ has()​ function to query relationships even two layers deep!</p>
  
```php
    // Author -> hasMany(Book::class);
    // Book -> hasMany(Rating::class);
    $authors = Author::has('books.ratings')->get();
    or
    $authors = Author::whereHas('books.ratings')->get(); 
  ``` 
  
  16. Default Timestamp
<p>While creating migrations, you can use ​ ->timestamp()​ column type with option ->useCurrent()​ , it will set ​ CURRENT_TIMESTAMP​ as default value.</p>

```php
    $table->timestamp('created_at')->useCurrent();
    $table->timestamp('updated_at')->useCurrent();
```
