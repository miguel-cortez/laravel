# SPATIE/LARAVEL-PERMISSION 

PARTE III. PROTECCIÓN DE RUTAS DE VUE-ROUTER

## 1. CONTEXTO

Ya tengo un proyecto de Laravel configurado, con una aplicación de Vue3, utilizando rutas con `vue-router`. La autenticación se ha realizado con Laravel Breeze.

Lo que aquí se explicará es cómo proteger las rutas de `vue-router` en función de los roles que se le hayan asignado al usuario que ha iniciado sesión.

## 2. DESARROLLO DE LA PRÁCTICA

### 2.1 Crear una función para obtener la información del usuario autenticado.
Archivo donde se creará la función: `app\Http\Controllers\Auth\AuthenticatedSessionController.php`   

```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Http\Requests\Auth\LoginRequest;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\View\View;

class AuthenticatedSessionController extends Controller
{
    // código omitido
    public function getCredenciales(Request $request){
        $user = Auth::user();
        $user->getPermissionsViaRoles();
        $user->getDirectPermissions();
        return response()->json(["user" => $user]);
    }
}
```
:books: La función `getCredenciales(Request $request)` retorna un objeto JSON con la información del usuario autenticado, incluyendo también los roles, los permisos asociados a los roles y los permisos directos asignados al usuario.    


**Ejemplo del objeto JSON retornado**  

```javascript
{
  "user": {
    "id": 1,
    "name": "Miguel Cortez",
    "email": "mcortez_vasquez@yahoo.com",
    "email_verified_at": null,
    "created_at": "2025-06-04T16:43:15.000000Z",
    "updated_at": "2025-06-04T16:43:15.000000Z",
    "roles": [
      {
        "id": 1,
        "name": "estandar",
        "guard_name": "web",
        "created_at": "2025-06-04T16:45:48.000000Z",
        "updated_at": "2025-06-04T16:45:48.000000Z",
        "pivot": {
          "model_type": "App\\Models\\User",
          "model_id": 1,
          "role_id": 1
        },
        "permissions": [
          {
            "id": 1,
            "name": "agregar productos",
            "guard_name": "web",
            "created_at": "2025-06-04T16:46:36.000000Z",
            "updated_at": "2025-06-04T16:46:36.000000Z",
            "pivot": {
              "role_id": 1,
              "permission_id": 1
            }
          },
          {
            "id": 2,
            "name": "editar productos",
            "guard_name": "web",
            "created_at": "2025-06-04T16:46:50.000000Z",
            "updated_at": "2025-06-04T16:46:50.000000Z",
            "pivot": {
              "role_id": 1,
              "permission_id": 2
            }
          }
        ]
      },
      {
        "id": 3,
        "name": "supervisor",
        "guard_name": "web",
        "created_at": "2025-06-06T16:52:33.000000Z",
        "updated_at": "2025-06-06T16:52:33.000000Z",
        "pivot": {
          "model_type": "App\\Models\\User",
          "model_id": 1,
          "role_id": 3
        },
        "permissions": [
          {
            "id": 4,
            "name": "ver informes",
            "guard_name": "web",
            "created_at": "2025-06-06T16:53:05.000000Z",
            "updated_at": "2025-06-06T16:53:05.000000Z",
            "pivot": {
              "role_id": 3,
              "permission_id": 4
            }
          }
        ]
      }
    ],
    "permissions": [
      {
        "id": 6,
        "name": "cancelar ventas",
        "guard_name": "web",
        "created_at": "2025-06-06T16:59:32.000000Z",
        "updated_at": "2025-06-06T16:59:32.000000Z",
        "pivot": {
          "model_type": "App\\Models\\User",
          "model_id": 1,
          "permission_id": 6
        }
      }
    ]
  }
}
```



### 2.2 Crear una ruta para hacer la petición con axios.

La ruta será definida en el archivo `routes\auth.php` 

```php
<?php
use App\Http\Controllers\Auth\AuthenticatedSessionController;
// código omitido
use Illuminate\Support\Facades\Route;

Route::middleware('guest')->group(function () {
    // código omitido
});

Route::middleware('auth')->group(function () {
    // código omitido
    Route::post('getcredenciales', [AuthenticatedSessionController::class, 'getCredenciales'])
        ->name('getcredenciales'); // ESTA ES LA RUTA AGREGADA.
});
```

### Modificar la función destroy

**Contenido de la función original**
```php
    public function destroy(Request $request): RedirectResponse
    {
        Auth::guard('web')->logout();

        $request->session()->invalidate();

        $request->session()->regenerateToken();

        return redirect('/');
    }
```

**Contenido de la función modificada**

En el archivo `app\Http\Controllers\Auth\AuthenticatedSessionController.php` 

```php
    public function destroy(Request $request): RedirectResponse
    {
        Auth::guard('web')->logout();

        $request->session()->invalidate();

        $request->session()->regenerateToken();

        return redirect('/')->with('javascript_code', "localStorage.removeItem('credenciales');"); // LINEA MODIFICADA
    }
```


### En resources\views\welcome.blade.php

```php
@if(session('javascript_code'))
    <script>{!! session('javascript_code') !!}</script>
@endif
```