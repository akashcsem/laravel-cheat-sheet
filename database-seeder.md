<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


## Laravel Seeder

- Go to project directory.
- Open terminal, </br>
- Go ahead and create one for Post as well:

```php 
php artisan make:seeder PostTableSeeder
```

Now open the ```database/seeds/PostTableSeeder``` file and pasted the code

```php
<?php

use App\Post;
use Illuminate\Database\Seeder;

class PostTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        Post::create([
            'title'       => 'Learn about database seeder.',
            'description' => 'Databse seeder is easy to understand, it is more flexible for static data',
            'category_id' => 1,
            'tag_id'      => 2,
            'posted_by'   => 'Mr. Admin',
        ]);
    }
}

```
Now open the ```database/seeds/DatabaseSeeder.php``` file and adjust the run method to the following:


```php
  <?php

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


