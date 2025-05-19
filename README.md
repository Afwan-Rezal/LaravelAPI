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
## 
