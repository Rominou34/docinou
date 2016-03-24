# Laravel Documentation

## Table of contents

* [Installation](#installation)
* Basics
  * [Routes](#routes)
  * [Controllers](#controllers)
  * [Requests](#requests)
  * [Responses](#responses)
  * [Cookies](#cookies)
  * [Middlewares](#middlewares)
  * [Views](#views)
* Databases
  * Soon...
* Use cases
  * [Combining routes, controllers and views to display and register users](#combining-routes-controllers-and-views-to-display-and-register-users)

# Installation

1. Install Composer
2. From any folder: `composer global require "laravel/installer"` ( also works with Lumen by using `"laravel/lumen-installer"` )
3. Create your project by exectuing this command from the folder where you want it: `composer create-project --prefer-dist laravel/lumen projet`. It also works with this one: `laravel new projet`
4. Go to the `.env` file and change these settings ( localhost on WAMP ):

  ```
  DB_HOST=127.0.0.1:3306
  DB_PORT=3306
  DB_DATABASE=DB_NAME
  DB_USERNAME=root
  DB_PASSWORD=''
  ```
5. Screw that shit, I'll make my own framework

# Basics

## Routes

**Everything regarding routes happens in `app/Http/routes.php`**

*To create a new basic route:*

```php
Route::get('/', function () {
    return 'Hello world!';
});
```

This will return 'Hello World!' if you make a GET request on `/`

*To redirect depending on "folders" in your url*

```php
Route::get('admin', function () {
    return 'Hi admin!';
});
```

This will return 'Hi admin' if you make a GET request on `admin/`.

*To create a route using a controller:*

```php
Route::get('/', 'Controller@function');
```

This will call the function `function()` from the controller `Controller` if you make a GET request `/`

*To redirect if there's a parameter in your url*

```php
Route::get('msg/{id}', 'Controller@msg');
```

This will call the function `msg()` in the controller `Controller` if you make a GET request on `msg/` with a parameter

**Note:** The parameters automatically get passed to the controller so **don't call the controller like this:**

```php
Route::get('msg/{id}', 'Controller@msg(id)');
```

## Controllers

The default controller can be found at `app/Http/Controllers/Controller.php`

If you want to create your own controller, you'll have to make it a child class of the base controller:

1. Import the base controller by putting `use Illuminate\Routing\Controller as BaseController;` at the beginning of your file
2. Declare your controller like this:
```php
class myController extends BaseController {
  // Your controller's stuff
}
```

## Requests

You can retrieve the parameters of a request from your controller by setting a `Request` parameter in your function, which will allow you to get all the parameters:
```php
public function post(Request $req) {
  $id = $req->input('id'); // We get the 'id' parameter form the request
  $name = $req->input('name'); // We get the 'name' parameter
}
```

You can also set a fallback value in case the parameter wasn't set:

```php
$name = $req->input('name', 'Bob'); // If the 'name' parameter is not set, it will default to 'Bob'
```

## Responses

First, you'll need to import the Response class `use Illuminate\Http\Response`

With this, you can customize your response's header and status, here is the basic syntax:
```php
Route::get('/', function () {
    return (new Response($content, $status))
                  ->header($var, $value);
});
```

And here is an example:
```php
Route::get('/', function () {
    return (new Response('{value: 1}', 200))
                  ->header('Content-Type', 'application/json');
});
```

In this example, I returned JSON, as I set `Content-Type: application/json` in the header, and the page was returned with a status 200.

You can of course change the status of the response, and you can also return custom header values ( I still hasn't find any use for that tho ):

```php
Route::get('/', function () {
    return (new Response('Hi there', 404))
                  ->header('Content-Type','text/html')
                  ->header('And-his-name-is','JOHN CENA');
});
```

Here we set the status of the response to 404 and we set a custom header element to JOHN CENA

**NOTE: The convention for custom HTTP headers is to start their name with 'X-' so this header should actually be rename 'X-And-his-name-is'**

*Alternative syntax*

You can pass an array of headers as a parameter instead of passing all headers one by one:
```php
return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
```

#### Return options

If you wish to return a view as your response's content, you can use the `view` method:
```php
return response()
            ->view('view', $data)
```

And you can also easily return JSON with the json() method:
```php
return response()->json(['name' => 'Rominou', 'age' => 19, 'sex' => 'big']);
```

*More to come once I'll get better at Laravel*

## Cookies

You can set cookies in your responses the same way you set headers:

```php
return response($content)
                ->cookie('name', 'value');
```

You can pass more parameters in your cookie constructor for more customization:

```php
->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
```

## Middlewares

HTTP Middlewares are used to filter and redirect HTTP requests before processing them with a controller

#### Creating a Middleware

* Navigate to the root folder of the project and execute this command:
  ```
  php artisan make:middleware MiddlewareName
  ```
* A file `MiddlewareName.php` wille be created in `app/Http/Middleware`
* Inside the file `app/Http/Kernel.php`, add this in the array `protected $middlewareGroups`:
```php
'MiddlewareName' => [
      \App\Http\Middleware\MiddlewareName::class,
],
```
* Finally, to add your middleware to a set of routes, go into `routes.php` and assign your routes to the middleware:
  * Either add the routes inside the middleware declaration:
  ```php
  Route::group(['middleware' => ['MiddlewareName']], function () {
    Route::get('/', function () { /* Do something */ });
      Route::post('/','Controller@function');
  });
  ```
  * Or assign the middleware to each route:
  ```php
  Route::get('/', ['middleware' => 'MiddlewareName', function() {}]);
  // Assign multiple middlewares to a route
  Route::post('/', ['middleware' => ['Mid1', 'Mid2'], function() {}]);
  ```

## Views

Laravel views contain the HTML code we will display on the website, they are located in the folder `resources/views`.

To create a new view, just create a new files in the folder: `resources/views/welcome.php`

Then, put some HTML code in it:

```html
<div class="msg">
  <h1>Welcome <?php echo $name; ?></h1>
</div>
```

Then, call your view ( from your route or controller ) like this:

```php
Route::get('/', function () {
    return view('welcome', ['name' => 'James']);
});
```

You can also pass an object as a parameter:

```html
<div class="userInfo">
  <p>ID: <?php echo $user->id; ?></p>
  <p>Name: <?php echo $user->name; ?></p>
</div>
```

# Databases

## Configuration

# Use cases

#### Combining routes controllers and views to display and register users

Here are example files `routes.php`, `ControllerUsers.php` and `viewUser.php` that act like this:
* GET request on `user/` shows all users
* GET request on `user/{id}` shows the user with the corresponding id
* POST request on `user/` inserts a new user in the BDD with the required parameters
* PUT request on `user/{id}` inserts or replace the user with the corresponding id ( used to update infos )
* DELETE request on `user/{id}` deletes the user with the corresponding id

*routes.php*
```php
Route::get('user/', 'ControllerUsers@getAllUsers');
Route::get('user/{id}', 'ControllerUsers@getUserById');
Route::post('user/', 'ControllerUsers@saveUser');
Route::put('user/{id}', 'ControllerUsers@updateUser');
Route::delete('user/{id}', 'ControllerUsers@deleteUser');
```

*ControllerUsers.php*
```php
namespace App\Http\Controllers;
use DB; // You can't use databases without this

// Blabla import everything
use Illuminate\Foundation\Bus\DispatchesJobs;
use Illuminate\Routing\Controller as BaseController;
use Illuminate\Foundation\Validation\ValidatesRequests;
use Illuminate\Foundation\Auth\Access\AuthorizesRequests;
use Illuminate\Http\Request;

class ControllerUsers extends BaseController
{
    use AuthorizesRequests, DispatchesJobs, ValidatesRequests;

    public function getAllUsers()
    {
        $users = DB::select('select * from users');
        foreach($users as $user) {
          echo(view('viewUser', ['user' => $user]));
        }
    }

    public function getUserById(Request $request) {
      $id = $request->input('id');
      $user = DB::select('select * from users where id =  ?', [$id]);
      echo(view('viewUser', ['user' => $user]));
    }

    public function saveUser(Request $request)
    {
      $i = null;
      $n = $request->input('name');
      $s = $request->input('surname');
      DB::insert('insert into users (id, name, surname) values (?, ?, ?)', [$i, $n, $s]);
    }

    public function updateUser(Request $request) {
      // Don't have time to test it so I'm gonna put a placeholder kappa
      $id = $request->input('id');
      updateTheBitch($id);
    }

    public function deleteUser(Request $request) {
      // Same as updateUser
      $id = $request->input('id');
      updateThisNigga($id);
    }
}
```

*viewUser.php*
```html
<div class="userInfo">
  <p>User id: <?php echo $user->id; ?></p>
  <p>User name: <?php echo $user->name; ?></p>
  <p>User surname: <?php echo $user->surname; ?></p>
</div>
```
