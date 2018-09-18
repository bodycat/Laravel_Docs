#Контроллеры

##Введение

Находятся в `app/Html/Controllers`

##Основы

###Определение контроллеров

Пример базового определения контроллера

	<?php
	
	namespace App\Http\Controllers;
	
	use App\User;
	use App\Http\Controllers\Controller;
	
	class UserController extends Controller
	{
	    /**
	     * Show the profile for the given user.
	     *
	     * @param  int  $id
	     * @return Response
	     */
	    public function show($id)
	    {
	        return view('user.profile', ['user' => User::findOrFail($id)]);
	    }
	}

Определение маршрута для этого контроллера

	Route::get('user/{id}', 'UserController@show');

###Контроллеры и пространства имен

Базовое пространство имен контроллеров- `app/Html/Controllers` определено в `app/Providers/RouteServiceProvider` в переменной `$namespace = 'App\Http\Controllers';`.

Для вложенных контроллеров в директории `App\Http\Controllers\Photos\AdminController` маршрут определяется как `Route::get('foo', 'Photos\AdminController@method');`

###Контроллеры с одним действием

Если контроллер имеет только одно действие, можно использовать метод `__invoke`.

	<?php
	
	namespace App\Http\Controllers;
	
	use App\User;
	use App\Http\Controllers\Controller;
	
	class ShowProfile extends Controller
	{
	    /**
	     * Show the profile for the given user.
	     *
	     * @param  int  $id
	     * @return Response
	     */
	    public function __invoke($id)
	    {
	        return view('user.profile', ['user' => User::findOrFail($id)]);
	    }
	}

Регистрация маршрута для этого контроллера

	Route::get('user/{id}', 'ShowProfile');

Создание контроллера с одним действием

	php artisan make:controller ShowProfile --invokable

##Контроллер Middleware

Назначение middleware для контроллера при определении маршрута

	Route::get('profile', 'UserController@show')->middleware('auth');

Назначение middleware для контроллера в конструкторе

	class UserController extends Controller
	{
	    /**
	     * Instantiate a new controller instance.
	     *
	     * @return void
	     */
	    public function __construct()
	    {
	        $this->middleware('auth');
	
	        $this->middleware('log')->only('index'); // только для метода index
	
	        $this->middleware('subscribed')->except('store');  // для всех методов, кроме store
	    }
	}

Назначение middleware для контроллера через функцию

	$this->middleware(function ($request, $next) {
	    // ...
	
	    return $next($request);
	});


##Контроллеры ресурсов

Создание контроллера ресурса

	php artisan make:controller PhotoController --resource

Регистрация маршрутов

	Route::resource('photos', 'PhotoController');

Регистрация маршрутов сразу для нескольких контроллеров ресурсов

	Route::resources([
	    'photos' => 'PhotoController',
	    'posts' => 'PostController'
	]);



###Частичные маршруты ресурсов
###Именование ресурсных маршрутов
###Именование параметров маршрута ресурса
###Локализация URI ресурсов
###Добавление контроллеров ресурсов
##Инъекции и контроллеры зависимостей
##Маршрутное кэширование