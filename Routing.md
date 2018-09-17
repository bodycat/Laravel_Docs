#Routing

##Basic routing

    Route::get('uri', function() {
		return "Hello World";
	});

###Default route files

Расположены в директории `routes`

`routes/web.php`- для `web` интерфейса, связан с группой middleware `web`

`routes/api.php`- для `api` интерфейса, связан с группой middleware `api`

###Доступные методы

	Route::get('uri', $callback)- получение ресурса;
	Route::post('uri', $callback)- создание ресурса;
	Route::put('uri', $callback)- обновление ресурса;
	Route::patch('uri', $callback);
	Route::delete('uri, $callback)- удаление ресурса;
	Route::options('uri', $callback)- определение возможностей сервера или параметров соединения для ресурса;

Несколько методов

	Route::match(['get', 'post'], '/', $callback);

Все методы

	Route::any('uri', $callback);

###CSRF защита

Любая HTML форма, использующая `get`, `post` или `delete` метод, должна иметь поле CSRF токена. Иначе запрос будет отклонен.

	<form method="POST" action="/profile">
	    @csrf
	    ...
	</form> 

###Перенаправление маршрутов

	Route::redirect('/here', '/there', 301);

###Маршруты для представлений

	Route::view('/welcome', 'welcome');
	Route::view('/welcome', 'welcome', ['name' => 'Taylor']);

##Параметры маршрутов

###Обязательные параметры

Один параметр

	Route::get('user/{id}', function($id) {
		return 'User: '.$id;
	});

Можно определить несколько обязательных параметров

	Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
	    //
	});

Параметры всегда заключаются в фигурные скобки `{}` и должны содержать только буквенные символы. Вместо тире `-` можно использовать знак подчеркивания `_`.
Параметры передаются в функцию или контроллер в их порядке, имена параметров не имеют значения.

###Необязательные параметры

	Route::get('user/{name?}', function ($name = null) {
	    return $name;
	});
	
	Route::get('user/{name?}', function ($name = 'John') {
	    return $name;
	});

###Регулярные выражения

///

##Именованные маршруты

	Route::get('user/profile', function () {
	    //
	})->name('profile');

или

	Route::get('user/profile', 'UserProfileController@show')->name('profile');

###Использование именованных маршрутов

Генерация URL

	$url = route('profile');

Генерация редиректа

	return redirect()->route('profile');

Передача параметров в именованные маршруты

	Route::get('user/{id}/profile', function ($id) {
	    //
	})->name('profile');
	
	$url = route('profile', ['id' => 1]);

###Проверка текущего маршрута

///

##Группы маршрутов

###Middleware

	Route::middleware(['first', 'second'])->group(function () {
	    Route::get('/', function () {
	        // Uses first & second Middleware
	    });
	
	    Route::get('user/profile', function () {
	        // Uses first & second Middleware
	    });
	});

###Пространства имен

	Route::namespace('Admin')->group(function () {
	    // Controllers Within The "App\Http\Controllers\Admin" Namespace
	});

###Префиксы маршрутов

	Route::prefix('admin')->group(function () {
	    Route::get('users', function () {
	        // Matches The "/admin/users" URL
	    });
	});

###Префиксы имен маршрутов

	Route::name('admin.')->group(function () {
	    Route::get('users', function () {
	        // Route assigned name "admin.users"...
	    })->name('users');
	});