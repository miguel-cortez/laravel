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
<?php

namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Tymon\JWTAuth\Contracts\JWTSubject; // 💡 LINEA AGREGADA
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

// 💡 EN LA SIGUIENTE LINEA, se agregó implements JWTSubject
class User extends Authenticatable implements JWTSubject
{
    /** @use HasFactory<\Database\Factories\UserFactory> */
    use HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }

    // 💡 COMIENZA BLOQUE AGREGADO
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
    // 💡 FINALIZA BLOQUE AGREGADO
}

```

# Crear el controlador

```
php artisan make:controller AuthController
```

# Agregar la lógica de AuthController

```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;
class AuthController extends Controller
{
    public function register(Request $request){
        $validator = Validator::make($request->all(),
            [
            'name' => 'required|max:255',
            'email' => 'required|email|max:255|unique:users',
            'password' => 'required|max:6'
            ]
        );
        if ($validator->stopOnFirstFailure()->fails()) {
            $errors = $validator->errors();
            if($errors->has("name")){
                return response()->json(["message"=>"El nombre no cumple los requerientos"],401);
            }
            else if($errors->has("email")){
                return response()->json(["message"=>"El email no cumple los requerientos"],401);
            }
            if($errors->has("password")){
                return response()->json(["message"=>"El password no cumple los requerientos"],401);
            }
        }else{
            return response()->json(["message"=>"SATISFACTORIO"],200);
        }
    }
}

```

## Crear un controlador para demostar la protección de rutas

```
php artisan make:controller UserController --model=User --api
```

## Crear un Middleware

```
php artisan make:middleware JwtMiddleware
```

## Registrar el Middleware en bootstrap/app.php
```
->withMiddleware(function (Middleware $middleware) {
    $middleware->alias(['jwt.verified' => JwtMiddleware::class]);
})
```

<details>
<?php

use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Foundation\Configuration\Middleware;
use App\Http\Middleware\JwtMiddleware; // LINEA AGREGADA
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware): void {
        //
    })
    ->withExceptions(function (Exceptions $exceptions): void {
        //
    })
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias(['jwt.verified' => JwtMiddleware::class]); // MIDDLEWARE AGREGADO
    })
    ->create();
</details>

## Rutas no protegidas

```
Route::post('register',[AuthController::class,'register']);
Route::post('login',[AuthController::class,'login']);
Route::get('users',[UserController::class,'index']); // BORRAR ESTA RUTA DESPUÉS DE PROTEGERLA CON EL MIDDLEWARE
```
## Crear una ruta protegida

```
Route::middleware('jwt.verified')->group(function(){
    Route::get('users',[UserController::class,'index']);
});
```


# Referencias  

https://youtu.be/p7tGYsVQV48  
https://youtu.be/jqaAfWQYb0Y  
https://jwt-auth.readthedocs.io/en/develop/quick-start/  



