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
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }
    public function getJWTCustomClaims()
    {
        return [];
    }
    // 💡 FINALIZA BLOQUE AGREGADO
}

```

# Crear el controlador AuthController

```
php artisan make:controller AuthController
```

# Agregar la lógica de AuthController

Al controlador agregará dos funciones, register() para crear nuevos usuarios y login() para autenticar usuarios registrados.  
```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator; // 💡LÍNEA AGREGADA
class AuthController extends Controller
{
    // AQUÍ AGREGUE LAS FUNCIONES
}
```

**Código fuente de la función register()**

```
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
```

**Código fuente de la función login()**  

```
    public function login(Request $request){
        $validator = Validator::make($request->all(),
            [
            'email' => 'required|email',
            'password' => 'required|min:6'
            ]
        );
        if ($validator->stopOnFirstFailure()->fails()) {
            return $validator->errors();
        }else{
            $credentials = $request->only('email','password');
            try{
                if(! $token = JWTAuth::attempt($credentials)){
                    return response()->json(['error' => 'invalid credential'],401);
                }
            }catch(JWTException $e){
                return response()->json(['error' => $e],500);
            }
            return response()->json(compact('token'));
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
<?php

use Illuminate\Foundation\Application;
// CODIGO OMITIDO
use App\Http\Middleware\JwtMiddleware; // 🎈 LÍNEA AGREGADA
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    // CODIGO OMITIDO

    // 🎈AGREGAR DESDE AQUÍ
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias(['jwt.verified' => JwtMiddleware::class]);
    })
    // 🎈 HASTA AQUÍ
    ->create();
```

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



