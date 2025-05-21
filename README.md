# Laravel API with CRUD
> This README serves only as a documentation for oneself to learn how to create API. The source tutorial is located ([here](https://www.youtube.com/watch?v=LmMJB3STuU4&list=PL38wFHH4qYZUXLba1gx1l5r_qqMoVZmKM)). Note that the tutorial uses Laravel v11.x, and this project uses Laravel v12.x.

## Getting Started
This project uses:
- PostmanAPI to identify the endpoints
- Visual Studio Code as IDE

## Starting the project
Ensure that PHP and Composer is installed.

1. Open a new Workspace to install Laravel Project using: `composer create-project laravel/laravel 'path/to/folder'`
2. Ensure workspace and terminal is inside the project directory.
3. Run `php artisan install:api`.
4. Congratulations, you have an API project!

---
## Creating and testing API route
1. Enter `routes/api.php`. The newly created file will contain the following:
```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');
```

2. Comment out the initialized `Route::get('/user'...);` and enter:
```
Route::get('/', function (){
    return 'Hello World';
});
```

3. In the terminal, you can see the available routes using `php artisan route:list`.

4. Enter `web.php` from the same directory as `api.php` and delete or comment out the code with Route below:
```
Route::get('/', function () {
    return view('welcome');
});
```

5. In the terminal, run `php artisan serve`. Go to PostmanAPI, and enter 127.0.0.1:8000/api to see the *return* value.

---
## Configuration
1. Create a model named Post using `php artisan make:model -a --api Post` in the terminal.

2. Go to the newly created Post model: `app/Models/Post.php`.

3. Enter below `use HasFactory`:
```
protected $fillable = [
    'title',
    'body',
];
```

4. Go to the newly created Post Table migration in `database/migrations` and enter in the `up` function:
```
Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title'); // Enter 'title' column
            $table->text('body'); // Enter 'body' column
            $table->timestamps();
        });
```

5. Migrate the new Migration file by going to the terminal and run `php artisan migrate`.

6. In `route/api.php`, delete/comment out the previous `Route::get('/',...);` and enter: `Route::apiResource('posts', PostController::class);`.
> Ensure you have, at the top of the file, a line that declares to use the file: `use App\Http\Controllers\PostController;`.

### Controller (Storing and Listing values)

1. In `app/Http/Controllers/PostController.php`, configure the following:
   - `index()` function
     - Enter `return Post::all();` within the function.
    ```
    public function index()
        {
            return Post::all();
        }
    ```
   - `store()` function
     - Edit the parameters from `StorePostRequest $request` to `Request $request`. 
     Ensure that `use Illuminate\Http\Request;` is declared.
     - Using `$request`, validate the values with the requirement below:
       - 'title'=> 'required|max:255'
       - 'body'=> 'required' 
     - Enter `return 'ok';` to ensure that the operation goes.
    ```
    public function store(Request $request)
    {
        $request->validate([
            'title'=> 'required|max:255',
            'body'=> 'required',
        ]);
        
        return 'ok';
    }
    ```

2. (IMPORTANT) Using PostmanAPI, head into `Header` and congifure:
- Key -> `Accept`
- Value -> `application/json`

> (Note) When run, the expected value should be a status: `422 Unprocessable Content`. This is because the testing of the value doesn't contain a JSON format to send to PostmanAPI.
> For testing purposes, within PostmanAPI, you can go to `Body` and click on `raw`, and add:
> ```
> {
>   'title' : 'Test',
>   'body' : 'Test body'
> }
> ```
> This will help show the returned 'ok'. 

3. To store the value, within `app/Http/Controllers/PostController.php`, configure the following:
```
    public function store(Request $request)
    {
        $request->validate([
            'title'=> 'required|max:255',
            'body'=> 'required',
        ]);
        
        $post = Post::create($fields);
        
        return ['post' => $post];
    }
```
When run using the same value in PostmanAPI, the body will return the value sent to the database. When a GET request is done, the index will show the stored value.

### Controller (Updating and Deleting)
