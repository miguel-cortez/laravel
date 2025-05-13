# Crear un nuevo proyecto en Laravel 12

## Requisitos

1. **WAMPServer** o paquete equivalente. Lo que realmente se necesita es tener instalado MySQL y el módulo de PHP. Las versiones pueden variar pero se recomienda que sea `MySQL 8.0` o superior y `PHP 8.2` o superior.
2. **Composer** instalado. A la hora de instalar composer se pide elegir la versión de PHP compatible con Laravel 12. Yo seleccioné `php-8.2.17`. Composer es un gestor de dependencias de PHP.

## Paso 1. Instalar laravel/installer

```
composer global require laravel/installer
```

## Paso 2. Crear una nueva aplicación

Para crear una nueva aplicaciones tiene varias alternativas. A continuación se muestran algunas formas posibles:

```
laravel new example-app
```

```
composer create-project laravel/laravel example-app
```

```
composer create-project --prefer-dist laravel/laravel example-app "12.*"
```

Responder a las preguntas. Normalmente dejamos las opciones predeterminadas, por lo tanto, solo presionamos ENTER; pero es bueno que lea y cambie sus respuestas si es conveniente.

Este proceso lo he realizado en equipos diferentes y no siempre es igual (me refiero a las preguntas que hace el asistente). Sin duda, esto se debe al hecho de que tengo diferentes versiones o herramientas configuradas en los equipos.

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

Ingrese al directorio del proyecto con el comando `cd example-app`

La documentación oficial de Laravel indica que debemos ejecutar los siguientes comandos:

```
npm install && npm run build
composer run dev
```

Nota. Debido a que yo no tengo instalado NodeJS no me han funcionado los comandos `npm install && npm run build` ni `composer run dev`. Después de instalar **NodeJS** sí funcionarán los comandos anteriores; pero para probar que el proyecto funciona no son necesarios por el momento.

## Paso 4. Probar el proyecto

```
php artisan serve

```

Nota. Se puede acceder al sitio web desde el navegador web. `http://localhost:8000`

Debido a que no he configurado la base de datos se muestra el siguiente error:  
![image](./img/internal_server_error_query_exception.png)

Antes de continuar, detenga la aplicación con `Control + C` 

## Paso 5. Descargar e instalar Visual Studio Code.

- Descargue Visual Studio Code.
- Instale Visual Studio Code.
- Abra la carpeta `example-app` en Visual Studio Code.

## Paso 6. Configuraciones de base de datos.

Estas configuracionese se realizan en el archivo  `example-app\.env`

```
DB_CONNECTION=sqlite
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=laravel
# DB_USERNAME=root
# DB_PASSWORD=
```

Las configuraciones quedarán como se muestra en el siguiente ejemplo:  

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=example_app
DB_USERNAME=root
DB_PASSWORD=
```

## Paso 7. Ejecutar las migraciones.
Ahora que ya indicó qué gestor de base de datos utilizará y el nombre de la base de datos que quiere crear, ejecute las migraciones.  
`php artisan migrate`

Como la base de datos aún no existe, le hará la siguiente pregunta (responda yes): 

WARN  The database 'example_app' does not exist on the 'mysql' connection.

Would you like to create it? (yes/no) [yes]

:collision: Yo tuve problemas a la hora de ejecutar las migraciones.

![image](./img/sqlstate_42000.png)  

Al parecer, el error se debe a la longitud predeterminada de las cadenas en el proyecto de Laravel.

**SOLUCIÓN**

- Abrir el archivo `app/Providers/AppServiceProvider.php`
- En la parte superior del archivo debe hacer la siguiente importación:  `use Illuminate\Support\Facades\Schema` 
- En el archivo `AppServiceProvider.php` tiene una función llamada boot() y debe modificarla como sigue:  
```
public function boot(): void
{
   Schema::defaultStringLength(191);
}
```

## Paso 8. Descargar e instalar NodeJS

Ejecute nuevamente las instrucciones del **Paso 3** y ahora sí funcionan. Los comandos npm install && npm run build
composer run dev

# INSTALACIÓN DE ADMINLTE

## Paso 9. Configure el paquete jeroennoten/laravel-adminlte

`composer require jeroennoten/laravel-adminlte`

## Paso 10. Publique los assets de AdminLTE en su proyecto.

`php artisan adminlte:install`

## Paso 11. Crear una vista para el panel administrativo.

- Haga una nueva carpeta llamada `admin` en el directorio `resources\views`
- Dentro de la carpeta recién creada, haga un nuevo archivo llamado `dashboard.blade.php` 

El contenido de la vista se puede obtener de la siguiente URL:
`https://jeroennoten.github.io/Laravel-AdminLTE/sections/overview/usage.html`

Este podría ser el contenido de la vista:
```
@extends('adminlte::page')

@section('title', 'Dashboard')

@section('content_header')
    <h1>Dashboard</h1>
@stop

@section('content')
    <p>Welcome to this beautiful admin panel.</p>
@stop

@section('css')
    {{-- Add here extra stylesheets --}}
    {{-- <link rel="stylesheet" href="/css/admin_custom.css"> --}}
@stop

@section('js')
    <script> console.log("Hi, I'm using the Laravel-AdminLTE package!"); </script>
@stop
```

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

Si se quiere proteger la ruta (esto es lo recomendado porque al panel administrativo no debería entrar personas no autenticadas). 

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

## Paso 17. Congigure el paquete laravel/breeze

`composer require laravel/breeze`

## Paso 18. Instale el paquete.

```
php artisan breeze:install
```

Nota. Durante el asistente de instalación pregunta qué herramienta utilizará para hacer pruebas. Yo seleccioné `PHPUnit` quizá no lo utilicemos.  

## Paso 19. Ejecutar comandos.

```
npm install (yo ya lo había ejecutado)
npm run dev
```

## Paso 20. Ejecutar las migraciones

```
php artisan migrate
```

Nota. En mi caso, cuando ejecuté las migraciones en este punto no encontró nada que migrar.  

## Paso 21. Probar la aplicación.

```
php artisan serve
```

Nota. Ya se integró Laravel/Breeze; pero con este proceso se borró la ruta que añadí manualmente para el dashboard y tuve necesidad de agregar nuevamente para poder acceder al dashboard administrativo.

Me refiero a esta ruta que fue creada en el paso 15.

```
Route::get('admin/dashboard', [App\Http\Controllers\Admin\DashboardController::class, 'index'])->name('admin.dashboard');
```

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
