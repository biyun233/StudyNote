## LARAVEL



### Valet

[Laravel Valet](https://github.com/laravel/valet) is a development environment for macOS minimalists. Laravel Valet configures your Mac to always run [Nginx](https://www.nginx.com/) in the background when your machine starts. Then, using [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq), Valet proxies all requests on the `*.test` domain to point to sites installed on your local machine.

[Laravel Valet Setup (Mac)](https://gist.github.com/bradtraversy/b58f74cd863a465068eaeaae1544d9be)

- The `park` Command

The `park` command registers a directory on your machine that contains your applications. Once the directory has been "parked" with Valet, all of the directories within that directory will be accessible in your web browser at `http://<directory-name>.test`

```sh
cd ~/Sites
valet park
```



### Setting up Debugbar

- install package

```sh
composer require barryvdh/laravel-debugbar --dev
```

- `/config/app.php`

​		add `Barryvdh\Debugbar\**ServiceProvider**::class,` in `providers`

- examples

```php
Debugbar::info('INFO!');
Debugbar::error('ERR!');
Debugbar::warning('warning!');
Debugbar::addMessage('msg!');
```



### Regenerate App_Key

```sh
php artisan key:generate
```



### Controllers

```sh
php artisan make:controller PostsController
```

laravel allows `resource controller` to generate four crud methods automatically

```sh
php artisan make:controller PostsController --resource
```

- Routes

  - ```php
    Route::resource('blog', PostsController::class);
    ```

    resource方法获取Controller中所有方法，并与各endpoint相关联

    ```sh
    php artisan route:list
    ```

    ![laravel1](img/laravel1.png)



### Single Action Controllers

When there is only one action in the controller, we can use `__invoke` method;

- Routes

  - ```sh
    Route::get('/', HomeController::class);
    ```

    

### Basic Routing

```php
// GET
Route::get('/blog', [PostsController::class, 'index']);
Route::get('/blog/1', [PostsController::class, 'show']);

// POST
Route::get('/blog/create', [PostsController::class, 'create']);
Route::post('/blog/1', [PostsController::class, 'store']);

// PUT OR PATCH
Route::get('/blog/edit/1', [PostsController::class, 'edit']);
Route::patch('/blog/1', [PostsController::class, 'update']);

// DELETE
Route::delete('/blog/1', [PostsController::class, 'destroy']);
```

Equals 

```php
Route::resource('blog', PostsController::class);
```

#### Other methods

```php
// Multiple HTTP verbs
Route::match(['GET', 'POST'], 'blog', [PostsController::class, 'index']);
Route::any('/blog', [PostsController::class, 'index']);

// Return view
Route::view('/blog', 'index', ['name' => 'Code with Biyun']);
```

#### Route Parameters

```php
Route::get('/blog/{id}', [PostsController::class, 'show']);
```

#### Routes with Expressions

```php
Route::get('/blog/{id}/{name}', [PostsController::class, 'show'])->where([
    'id' => '[0-9]+',
    'name' => '[a-z]+',
]);

Route::get('/blog/{id}/{name}', [PostsController::class, 'show'])
    ->whereNumber('id')
    ->whereAlpha('name');
```

#### Named Routes

```php+HTML
Route::get('/blog', [PostsController::class, 'index'])->name('blog.index');
Route::get('/blog/{id}', [PostsController::class, 'show'])->name('blog.show');

<a href={{ route('blog.index') }}>Blog</a>
<a href={{ route('blog.show', ['id' => 1]) }}>Show Blog</a>
```

#### Route Prefixes

```php
Route::prefix('/blog')->group(function () {
    Route::get('/', [PostsController::class, 'index'])->name('blog.index');
    Route::get('/{id}', [PostsController::class, 'show'])->name('blog.show');
    Route::get('/create', [PostsController::class, 'create'])->name('blog.create');
    Route::post('/{id}', [PostsController::class, 'store'])->name('blog.store');
    Route::get('/edit/{id}', [PostsController::class, 'edit'])->name('blog.edit');
    Route::patch('/{id}', [PostsController::class, 'update'])->name('blog.update');
    Route::delete('/{id}', [PostsController::class, 'destroy']);
});
```

#### Fallback Routes

```sh
php artisan make:controller FallbackController
```

```php
Route::fallback(FallbackController::class);
```



### Database

#### Test Database connected Using Tinker

```sh
php artisan tinker
DB::connection()->getPdo();
```



#### Migrations

```sh
php artisan make:migration create_posts_table
```

```php
/**
* Run the migrations.
*
* @return void
*/
public function up()
{
  Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title')->unique();
    $table->text('excerpt')->nullable();
    $table->text('body');
    $table->integer('min_to_read')->default(1);
    $table->string('image_path');
    $table->boolean('is_published');
    $table->timestamps();
  });
}
```

```sh
php artisan migrate
php artisan migrate:status
php artisan migrate:reset (rollback all)
php artisan migrate:refresh (rollback all and run all)
php artisan migrate:refresh --path=database/migrations/2023_05_24_054016_create_posts_table.php
```

#### Seeders

```sh
php artisan make:seeder PostsTableSeeder
php artisan make:model Post
```



/database/seeders/PostsTableSeeder.php

```php
		/**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $posts = [
            [
                'title' => 'Post One',
                'excerpt' => 'Summary of Post One',
                'body' => 'Body of Post One',
                'image_path' => 'Empty',
                'is_published' => false,
                'min_to_read' => 2
            ],
            [
                'title' => 'Post Two',
                'excerpt' => 'Summary of Post Two',
                'body' => 'Body of Post Two',
                'image_path' => 'Empty',
                'is_published' => false,
                'min_to_read' => 2
            ],
        ];

        foreach ($posts as $key => $value) {
            Post::create($value);
        }
    }
```



/database/seeders/DatabaseSeeder.php

```php
		/**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        $this->call(PostsTableSeeder::class);
    }
```



- Method 1

  - ```sh
    php artisan migrate:reset
    php artisan migrate --seed
    ```

- Method 2

  - ```sh
    php artisan migrate:reset
    php artisan migrate
    php artisan db:seed
    ```

    

	#### Factories

```sh
php artisan make:factory PostFactory
```



/database/seeders/DatabaseSeeder.php

```php
class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        // $this->call(PostsTableSeeder::class);
        Post::factory(100)->create();
      	// Post::factory(100)->create([
        //    'body' => 'overriding the body of pur post'
        // ]);
    }
}
```

```sh
php artisan db:seed
```

#### Query Builder

```php
 $posts = DB::table('posts')
            ->select('id', 'title', 'min_to_read', 'is_published')
            ->where('id', '>', 50)
            ->where('is_published', true)
            ->whereBetween('min_to_read', [2, 6])
            ->orderBy('id', 'desc')
            ->get();
```

