# Routes, View & Controller

From previous episode we learned how to configure a laravel project from installing the require tools until running our application. In this episode we are gonna learn about how we manage routes and respond incoming request to the specified routes.

## Routes

Routes is a 'route' that make users can send a request to a certain url that already defined in our application. We can define accessable urls as much as we want. But before that, let's take a look into our `web.php` file in `routes` folder. If we open our `web.php` file we are gonna see something like this,

```php
Route::get('/', function () {
    return view('welcome');
});
```

As I mentioned before, the syntax above means, if there any request to path `/` we will return a `welcome` view from our views folder. Then, if we open `welcome.blade.php` which located in `resources/views` we are gonna see a well written HTML file. But, if we look closer we'll find syntax that aren't HTML. Something like this,

```html
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}"></html>

<!-- or -->

@if (Route::has('login'))
<div class="hidden fixed top-0 right-0 px-6 py-4 sm:block">
  @auth
  <a
    href="{{ url('/home') }}"
    class="text-sm text-gray-700 dark:text-gray-500 underline"
    >Home</a
  >
  @else
  <a
    href="{{ route('login') }}"
    class="text-sm text-gray-700 dark:text-gray-500 underline"
    >Log in</a
  >

  @if (Route::has('register'))
  <a
    href="{{ route('register') }}"
    class="ml-4 text-sm text-gray-700 dark:text-gray-500 underline"
    >Register</a
  >
  @endif @endauth
</div>
@endif
```

## View

It's very obvious that syntax above is not HTML. It is a Blade file, Blade is a templating language that allows us to call logical syntax in our 'HTML' like file. We are gonna talk about Blade later.

OK. Let's go ahead and delete everything inside the `welcome.blade.php` file. After deleting all lines inside it, we are gonna insert our name into it. After inserting our name, try to start the server and open http://localhost:8000 . If we doing it right, it will show our name once we open it.

Now that we know if we change something in `welcome.blade.php` it will appear in our website, why don't we make it dynamic?

"But isn't it hard?"

Something like that maybe came into your mind. But, with Laravel, it is very easy to do something like that. We can send data into our `views` by adding a parameter after the template name. Something like this,

```php
Route::get('/', function () {
    return view('welcome', [ 'name' => 'Aziz' ]);
});
```

By adding an array as the second parameter, we can send data to our views. "But, how do we handle it?". It is very simple, we just need to add something like this in our views,

```html
{{ $name }}
```

Syntax above will be translated by blade into an HTML and the variable inside the curly bracket is one of the key from our associative array.

## Controller

Imagined that we have to do some mathematical things to our data before sending it, or we have to query it first from our database, and of course we are not handle a route, but we handle routes. If we keep handling those thing inside our `web.php` file, the file gonna be so~~~~ **BIG**.

To prevent that from happening, we need to make another file to handle the calculation, database query, and etc. We call this file a `Controller`. Something that control all the necessary stuff before showing the view.

In Laravel App, we can create a new controller by running the command below,

```sh
php artisan make:controller WelcomeController
```

After we run the command, we will see a message that tells us the controller successfully created,

```sh
Controller created successfully.
```

To check our controller, we can open it in `app/Http/Controllers`. There'll be a new file with a name `WelcomeController.php`. Then we can add a new function to return a `view` with it's data.

```php
<?php

namespace App\Http\Controllers;

class WelcomeController extends Controller
{
    public function index()
    {
        return view('welcome', ['name' => 'Aziz']);
    }
}
```

Before you open the browser, don't forget to register the controller on the route.

```php
Route::get('/', [WelcomeController::class, 'index']);
```

You're all set to have fun with your controller!
