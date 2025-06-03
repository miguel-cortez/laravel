# SPATIE/LARAVEL-PERMISSION 

PARTE III. PROTECCIÓN DE RUTAS DE VUE-ROUTER

## 1. CONTEXTO

Ya tengo un proyecto de Laravel configurado, con una aplicación de Vue3, utilizando rutas de vue-router. La autenticación se ha realizado con Laravel/Breeze.

Lo que aquí se explicará es cómo proteger las rutas de vue-router en función de los roles que se le hayan asignado al usuario que ha iniciado sesión.

## 2. DESARROLLO DE LA PRÁCTICA

### 2.1 Crear una función para obtener la información del usuario autenticado.
Archivo donde se creará la función: `app\Http\Controllers\Auth\AuthenticatedSessionController.php`   

```php
public function getUser(Request $request){
    $user = Auth::user();
    $roles = $user->getRoleNames();
    $directPermissions = $user->getDirectPermissions();
    $rolesPermissions = $user->getPermissionsViaRoles();
    return response()->json(["user" => $user, "roles" => $roles, "directPermissions" => $directPermissions, "rolesPermissions" => $rolesPermissions]);
}
```
La función retorna los datos del usuario, sus roles y sus permisos.  

### 2.2 Crear una ruta para hacer la petición con axios.

La ruta será definida en el archivo `routes\auth.php` 

```php
Route::middleware('auth')->group(function () {
    // código omitido
    Route::post('getuser', [AuthenticatedSessionController::class, 'getUser'])
        ->name('getuser');
});
```
:books:  DOCUMENTACIÓN EN PROCESO DE DASARROLLO