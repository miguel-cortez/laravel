# Crear un nuevo proyecto en Laravel 12

## Requisitos
1. WAMPServer o paquete equivalente.
2. Composer instalado. A la hora de instalar composer se pide elegir la versión de PHP compatible con Laravel 12.  
  
## Paso 1. Instalar laravel/installer

```
composer global require laravel/installer
```

## Paso 2. Crear una nueva aplicación
```
laravel new example-app
o 
composer create-project laravel/laravel example-app

composer create-project --prefer-dist laravel/laravel example-app "12.*"
```

Responder a las preguntas. Normalmente dejamos las opciones predeterminadas, por lo tanto, solo presionamos ENTER; pero es bueno que lea.
```
 Which starter kit would you like to install? [None]:
  [none    ] None
  [react   ] React
  [vue     ] Vue
  [livewire] Livewire
 >

Which authentication provider do you prefer? [Laravel's built-in authentication]:
  [laravel] Laravel's built-in authentication
  [workos ] WorkOS (Requires WorkOS account)

Which testing framework do you prefer? [Pest]:
  [0] Pest
  [1] PHPUnit

 Would you like to run npm install and npm run build? (yes/no) [yes]:
 >no
```

## Paso 3. Ingresar a la carpeta del proyecto y ejecutarlo.

La documentación oficial de Laravel indica que debemos ejecutar los siguientes comandos
```
cd example-app
npm install && npm run build
composer run dev
```

Nota. Debido a que yo no tengo instalado Nodejs no me han funcionado (hasta este punto) los comandos `npm install && npm run build` ni `composer run dev`

Después de instalar Nodejs sí funcionarán los comandos anteriores.

## Paso 4. Probar el proyecto
```
php artisan serve

```

Nota. Se puede acceder al sitio web desde el navegador web. `http://localhost:8000`

## Paso 5. Descargar e instalar Visual Studio Code.

## Paso 6. Configure las opciones de bases de datos.
Esto en `.env`

```
DB_CONNECTION = mysql
...
```

## Paso 7. Ejecutar las migraciones.

`php artisan migrate`

Nota. Yo tuve problemas a la hora de ejecutar las migraciones. En la descripción del problema se mostraba algo como **users already exists*** 

Al parecer, el error se debe a la longitud predeterminada de las cadenas en el proyecto de Laravel.

La solución:  

* Abrir el archivo `app/Providers/AppServiceProvider.php`
* Importar **use Illuminate\Support\Facades\Schema
* Agregar la siguiente línea a la función boot( )

## Paso 8. Descargar e instalar NodeJS
Ejecute nuevamente las instrucciones del **Paso 3** y ahora sí funcionan. Los comandos npm install && npm run build
composer run dev

# INSTALACIÓN DE ADMINLTE

## Paso 9.

`composer require jeroennoten/laravel-adminlte`

## Paso 10.
`php artisan adminlte:install`

## Paso 11. Crear una vista

`resources\views\admin\dashboard.blade.php`

El contenido de la vista se puede obtener de la siguiente URL:
`https://jeroennoten.github.io/Laravel-AdminLTE/sections/overview/usage.html`

## Paso 12. Agregar una ruta en el menú.

En `config/adminlte.php`

Configuraciones:  

```
'menu' => [
    [
        'text' => 'Dashboard',
        'url'  => 'admin/dashboard',
        'icon' => 'fas fa-tachometer-alt',
    ],
    // Add more menu items here
],
```

## Paso 13. Crear un controlador para la plantilla dashboard.

`php artisan make:controller Admin/DashboardController`

## Paso 14. Programar la función index del controlador para llamar la vista

```
namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;

class DashboardController extends Controller
{
    public function index()
    {
        return view('admin.dashboard');
    }
}
```

## Paso 15. Crear una ruta para la administración del sitio.

```
Route::get('admin/dashboard', [App\Http\Controllers\Admin\DashboardController::class, 'index'])->name('admin.dashboard');
```

Si se quiere proteger la ruta.

```
Route::get('admin/dashboard', [App\Http\Controllers\Admin\DashboardController::class, 'index'])
    ->middleware('auth')
    ->name('admin.dashboard');
```

## Paso 16. Ejecutar el proyecto y probar AdminLTE.

```
npm run dev
php artisan serve
```

vistiar `http://localhost:8000/admin/dashboard`  


# LARAVEL/BREEZE

## Paso 17.
`composer require laravel/breeze` 

## Paso 18.
```
php artisan breeze:install
```

Nota. Para pruebas unitarias yo elegí PHPUnit.

## Paso 19.
```
npm install (yo ya lo había ejecutado)
npm run dev
```

## Paso 20. Ejeuctar migraciones
```
php artisan migrate
```

Nota. En mi caso, no encontró nada nuevo que migrar.

## Paso 21. Probar la aplicación.

```
php artisan serve
```

Nota. Ya se integró Laravel/Breeze; pero con este proceso se borró la ruta que añadí manualmente para el dashboard y tuve necesidad de agregar nuevamente para poder acceder al dashboard administrativo. Me refiero a la ruta del paso 15.


REFERENCIAS


## Instalar laravel/breeze
Referencia inicial: https://codecourse.com/articles/how-to-install-laravel-breeze-on-laravel-12

Referencia más detalles: https://kritimyantra.com/blogs/install-laravel-breeze-package-in-laravel-12
```
composer require laravel/breeze --dev
php artisan breeze:install
```
A la pregunta **Which Breeze stack would you like to install?**, responder con la opción **Blade with Alpine**  



## Vue3 en Laravel 12
https://dev.to/robin-ivi/laravel-12-and-vue-3-ultimate-starter-guide-3bmd


## AdminLTE
https://kritimyantra.com/blogs/laravel-12-adminlte-integration-setup-your-stunning-admin-dashboard

## Original
https://jeroennoten.github.io/Laravel-AdminLTE/


## Habilitar las rutas API
```
php artisan install:api
```