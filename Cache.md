<p align="center">
  <img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400" height="70">
</p>


## Laravel Caching

- Create a folder named it Cache.
- Into the filder create a new file like "Users.php", </br>

See the snapshot bellow.
<p align="left">
  <img src="images/users.PNG" width="600" height="220">
</p>

## Write the code into `User.php` file

  <code>
  <?php
  namespace App\Cache;

  use App\User;
  use Carbon\Carbon;

  class Users
  {
        const CACHE_KEY = 'USERS';

        public function all($orderBy)
        {
              $key = "all.{$orderBy}";
              $cacheKey = $this->getCacheKey($key);
              return cache()->remember($cacheKey, Carbon::now()->addMinutes(5), function () use ($orderBy) {
                    return User::orderBy($orderBy)->get();
              });
        }

        public function getCacheKey($key)
        {
              $key = strtoupper($key);
              return self::CACHE_KEY . "$key";
        }
  }
</code>


