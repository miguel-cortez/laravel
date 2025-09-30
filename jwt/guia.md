🪲DOCUMENTACIÓN EN PROCESO

# Protección de rutas API con JWT

## Creación del proyecto.
```
composer create-project laravel/laravel example-jwt-laravel-12
```

## Habilitar rutas API

```
php artisan install:api
```

## Instalación de paquete de JWT para Laravel

[Referencia](https://jwt-auth.readthedocs.io/en/develop/laravel-installation/)

ℹ️**Thunder Client**, extensión de Visual Studio Code para hacer pruebas de rutas simila a **Postman**

```
composer require tymon/jwt-auth
```

## Registrar el proveedor

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

Ahora debe existir el siguiente archivo: `config/jwt.php` 

## Generando una clave secreta

```
php artisan jwt:secret
```

Esto agrega una clave secreta en el archivo `.env` y muesta la clave en la terminal como sigue:  

jwt-auth secret [**Yn2uTgg40nqfQn6HDk4ecbTWcJ...I588Y9ZHUkKkXV9UsqBl**] set successfully.  

## Actualizar Models\User

```
use Tymon\JWTAuth\Contracts\JWTSubject;
```

**Se deben implementar los siguientes dos métodos:**  

```

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
```

El modelo **User** lucirá como el siguiente código:  

```
<?php

namespace App\Models;

// UNA LÍNEA OMITIDA

use Tymon\JWTAuth\Contracts\JWTSubject;

// CÓDIGO OMITIDO

class User extends Authenticatable implements JWTSubject
{

    // CÓDIGO OMITIDO

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }

}
```

## Configurar Auth guard

Realizar estas configuraciones en **config/auth.php**  

🖊️Cambié `web` por `api` en:  

```
    'defaults' => [
        'guard' => env('AUTH_GUARD', 'api'),
        'passwords' => env('AUTH_PASSWORD_BROKER', 'users'),
    ],
```

y agregué:  

```
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
```

en:  

```
    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'api' => [
            'driver' => 'jwt',
            'provider' => 'users',
        ],
    ],
```

## Añadir algunas rutas de autenticación básicas

Estas rutas se agregarán en `routes/api.php` 

⚠️El archivo api.php ya tenía el siguiente código.

```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');
```

y las rutas que se suguiere agregarar son las siguientes (pendiente de comprobar si entra en conflicto con las líneas ya existentes):  
```
Route::group([

    'middleware' => 'api',
    'prefix' => 'auth'

], function ($router) {

    Route::post('login', 'AuthController@login');
    Route::post('logout', 'AuthController@logout');
    Route::post('refresh', 'AuthController@refresh');
    Route::post('me', 'AuthController@me');

});
```
⏲️POR AHORA, yo he dejado el siguiente código:  

```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');

Route::group([

    'middleware' => 'api',
    'prefix' => 'auth'

], function ($router) {

    Route::post('login', 'AuthController@login');
    Route::post('logout', 'AuthController@logout');
    Route::post('refresh', 'AuthController@refresh');
    Route::post('me', 'AuthController@me');

});
```

## Crear Authcontroller

```
php artisan make:controller AuthController
```

El cotenido original del controlador recien creado es:  

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class AuthController extends Controller
{
    //
}
```

y finalmente, el contenido debe ser el siguiente:  

```
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Auth;
use Illuminate\Http\Request;
use App\Models\User;
class AuthController extends Controller
{
    /**
     * Create a new AuthController instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth:api', ['except' => ['login']]);
    }

    /**
     * Get a JWT via given credentials.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function login()
    {
        $credentials = request(['email', 'password']);

        if (! $token = auth()->attempt($credentials)) {
            return response()->json(['error' => 'Unauthorized'], 401);
        }

        return $this->respondWithToken($token);
    }

    /**
     * Get the authenticated User.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function me()
    {
        return response()->json(auth()->user());
    }

    /**
     * Log the user out (Invalidate the token).
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function logout()
    {
        auth()->logout();

        return response()->json(['message' => 'Successfully logged out']);
    }

    /**
     * Refresh a token.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function refresh()
    {
        return $this->respondWithToken(auth()->refresh());
    }

    /**
     * Get the token array structure.
     *
     * @param  string $token
     *
     * @return \Illuminate\Http\JsonResponse
     */
    protected function respondWithToken($token)
    {
        return response()->json([
            'access_token' => $token,
            'token_type' => 'bearer',
            'expires_in' => auth()->factory()->getTTL() * 60
        ]);
    }
}

```


# Referencias  

https://youtu.be/p7tGYsVQV48  
https://youtu.be/jqaAfWQYb0Y  
https://jwt-auth.readthedocs.io/en/develop/quick-start/  



