# Protección de rutas API con JWT

## 1. Creación del proyecto.
```
composer create-project laravel/laravel example-jwt-laravel-12
```

## 2. Habilitar rutas API

```
php artisan install:api
```

## 3. Instalación de paquete de JWT para Laravel

```
composer require tymon/jwt-auth
```

## 4. Registrar el proveedor

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

Ahora debe existir el siguiente archivo: `config/jwt.php` 

## 5. Generando una clave secreta

```
php artisan jwt:secret
```

Esto agrega una clave secreta en el archivo `.env` y muesta la clave en la terminal como sigue:  

jwt-auth secret [**Yn2uTgg40nqfQn6HDk4ecbTWcJ...I588Y9ZHUkKkXV9UsqBl**] set successfully.  

## 5. Actualizar Models\User

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

    // CÓDIGO OMITIDO.

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

# 6. Crear el controlador AuthController

```
php artisan make:controller AuthController
```

# 7. Agregar la lógica de AuthController

Al controlador agregará dos funciones, register() para crear nuevos usuarios y login() para autenticar usuarios registrados.  
```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator; // LINEA AGREGADA
use Illuminate\Support\Facades\Hash; // LINEA AGREGADA
use App\Models\User; // LINEA AGREGADA
use Tymon\JWTAuth\Facades\JWTAuth; // LINEA AGREGADA
use Illuminate\Support\Facades\Auth;
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
            'password' => 'required|min:6'
            ]
        );
        if ($validator->stopOnFirstFailure()->fails()) {
            return $validator->errors();
        }else{
            $user = User::create([
                'name' => $request->name,
                'email' => $request->email,
                'password' => Hash::make($request->password)
            ]);

            $token = JWTAuth::fromUser($user);

            return response()->json(
                [
                'user'=>$user,
                "token" => $token
                ],201);
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

## 8. Crear un controlador para demostrar el uso de rutas protegidas

```
php artisan make:controller UserController --model=User --api
```

## En la función index() del controlador UserController obtenga la lista de usuarios

**Código fuente de la función index()**  

```
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Display a listing of the resource.
     */

    public function index()
    {
        $users = User::all();
        return response()->json($users);
    }

    // CÓDIGO OMITIDO
}

```

## 9. Crear un Middleware

```
php artisan make:middleware JwtMiddleware
```

## 10. Programe el Middleware

```
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;
use Tymon\JWTAuth\Facades\JWTAuth; // 💡LINEA AGREGADA
use Tymon\JWTAuth\Exceptions\JWTException; // 💡LÍNEA AGREGADA
use Tymon\JWTAuth\Exceptions\TokenExpiredException; // 💡 LÍNEA AGREGADA
use Tymon\JWTAuth\Exceptions\TokenInvalidException; // 💡 LÍNEA AGREGADA

class JwtMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        // 💡INICIA LA LÓGICA AGREGADA AL MIDDLEWARE
        if (!JWTAuth::getToken()) {
            return response()->json(['status' => 'token not provided'], 401);
        }
        try {
            JWTAuth::parseToken()->authenticate();
        }catch(TokenInvalidException $e) {
            return response()->json(['status' => 'invalid token'], 401);
        }catch (TokenExpiredException $e) {
            return response()->json(['status' => 'expired token'], 401);
        } catch (JWTException $e) {
            return response()->json(['status' => 'token error'], 401);
        }
        // 💡FINALIZA LA LÓGICA AGREGADA AL MIDDLEWARE
        return $next($request);
    }
}
```

## 11. Registrar el Middleware en bootstrap/app.php
```
<?php

use Illuminate\Foundation\Application;
// CODIGO OMITIDO
use App\Http\Middleware\JwtMiddleware; // 💡 LÍNEA AGREGADA
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    // CODIGO OMITIDO

    // 💡AGREGAR DESDE AQUÍ
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias(['jwt.verified' => JwtMiddleware::class]);
    })
    // 💡 HASTA AQUÍ
    ->create();
```

## 12. Agregue las rutas en routes/api.php

**En la parte superior de api.php**  

```
use App\Http\Controllers\AuthController;
use App\Http\Controllers\UserController;
use App\Http\Middleware\JwtMiddleware;
```

**Rutas no protegidas**  

```
Route::post('register',[AuthController::class,'register']);
Route::post('login',[AuthController::class,'login']);

Route::get('users',[UserController::class,'index']); // 🔖 PRUEBE OBTENER LA LISTA DE USUARIOS CON ESTA RUTA DESPROTEGIDA. LUEGO, CUANDO LA PROTEGA CON EL MIDDLEWARE BORRE ESTA LÍNEA.  
```
**Crear un grupo de rutas protegidas**  

📚Nota. Puede incluir varias rutas que estarán protegidas con el mismo Middleware.

```
Route::middleware('jwt.verified')->group(function(){
    Route::get('users',[UserController::class,'index']); // ESTA ES UNA RUTA PROTEGIDA
});
```
## 13. Pruebas en Postman

### A. Cree un nuevo usuario
<img width="1506" height="709" alt="imagen" src="https://github.com/user-attachments/assets/cc15e65a-3780-4bc2-aaab-92e99fb43274" />

### B. Prueba la ruta sin protección.  
<img width="1491" height="582" alt="imagen" src="https://github.com/user-attachments/assets/c7d6d543-1c5e-42d1-9541-7567df5d62c6" />

### C. Autentique el usuario previamente registrado.  

<img width="1526" height="577" alt="imagen" src="https://github.com/user-attachments/assets/fe1bd92f-fd27-4fd4-8e0b-b8fac30c85d9" />

ℹ️Copie el token para usarlo más tarde.

### D. Pruebe la ruta protegida sin enviar el token

<img width="1522" height="565" alt="imagen" src="https://github.com/user-attachments/assets/2541836a-e031-48aa-9266-9879a0873675" />


### E. Pruebe la ruta protegida enviando el token.

***Pegue el token***

<img width="1538" height="385" alt="imagen" src="https://github.com/user-attachments/assets/f4398f13-3985-463a-966e-e47346f7bbf9" />


***Envíe la petición***  
<img width="1501" height="566" alt="imagen" src="https://github.com/user-attachments/assets/0091f64f-8926-493f-915e-c2d3b5c19f5b" />

### F. Probando la ruta protegida con un token expirado

<img width="1510" height="543" alt="imagen" src="https://github.com/user-attachments/assets/5b5aa82b-2388-4f16-bc4f-b8cc12c616b8" />

### G. Probando la ruta protegida con un token alterado

<img width="1542" height="548" alt="imagen" src="https://github.com/user-attachments/assets/de1e6ce2-a2b6-4c6c-9460-545bcc5eda9f" />

## 🧰 Herramienta alternativa a Postman

ℹ️**Thunder Client**, extensión de **Visual Studio Code** para hacer pruebas de rutas similar a **Postman**  

<img width="1669" height="524" alt="imagen" src="https://github.com/user-attachments/assets/7d457a1b-afa4-4ee8-a02d-197f70a540c0" />

# Referencias  

https://youtu.be/p7tGYsVQV48  
https://youtu.be/jqaAfWQYb0Y  
https://jwt-auth.readthedocs.io/en/develop/quick-start/  



