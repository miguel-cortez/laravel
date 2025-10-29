## Paso 1. Crear un nuevo proyecto.

📚 En este paso se creará un proyecto básico de Laravel.  

```
laravel new JetStreamInertia
```
El valor de la pregunta **Which starter kit would you like to install?** por defecto será **None**  

<img width="547" height="381" alt="imagen" src="https://github.com/user-attachments/assets/69b2e510-5515-4e15-afb4-75a5e0225610" />

El valor de la pregunta **Which testing framework do you prefer?** por defecto será **Pest**  

<img width="435" height="106" alt="imagen" src="https://github.com/user-attachments/assets/06eb6ff3-cd10-4cb8-85b6-8b1daa4d8bf8" />

El valor de la pregunta **Do you want to install Laravel Boost to improve AI assisted coding? (yes/no)** por defecto será **yes**  
<img width="780" height="61" alt="imagen" src="https://github.com/user-attachments/assets/ea0fbad4-cfaa-4950-92ab-7de6e2395779" />

El valor predeterminado de la pregunta **Which database will your application use? [SQLite]** será **SQLite** pero yo lo cambié a **mysql**  

 <img width="477" height="140" alt="imagen" src="https://github.com/user-attachments/assets/be8bfe51-2026-4b49-a8c6-fe3f3c51c34b" />

No ejecuté las migraciones por defecto para poder cambiar el nombre de la base de datos en el archivo **.env**   
<img width="897" height="58" alt="imagen" src="https://github.com/user-attachments/assets/f871035b-3cbb-4ab4-923c-f3eabf618430" />

No ejecuté los comandos **npm instal** y **npm run build** porque prefiero entrar a la carpeta del nuevo proyecto y ejecutar desde allí los comandos.  

<img width="637" height="50" alt="imagen" src="https://github.com/user-attachments/assets/8bfe5ad8-238b-447d-988e-8ae92cf2b8e3" />

## Paso 2. Ingrese a la carpeta del proyecto

```
cd JetStreamInertia
```

## Paso 2. Modificar el archivo .env

📚Este proceso se realizará en Visual Studio Code. Pero cuando abrir el proyecto, ya estaban las configuraciones, se debe a las opciones elegidas previamente mientras se creaba el proyecto.  

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=jetstreaminertia
DB_USERNAME=root
DB_PASSWORD=
```
## Paso 3. Ejecute las migraciones

```
php artisan migrate
```

En la siguiente pregunta, solo presiones ENTER, por defecto creará la base de datos.  
<img width="784" height="205" alt="imagen" src="https://github.com/user-attachments/assets/98a19c18-00a1-4598-bd23-c0743d84b48a" />

<img width="1106" height="617" alt="imagen" src="https://github.com/user-attachments/assets/6ec8ada5-e919-462e-85a4-5eb6e6bd1ce7" />

Modifique el archivo **app/Providers/AppServiceProvider.php** 

```
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Schema; // 👈LINEA AGREGADA
class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        Schema::defaultStringLength(191); // 👈LÍNEA AGREGADA
    }
}
```
Borre la base de datos y ejecute nuevamente las migraciones con `php artisan migrate`  

<img width="237" height="311" alt="imagen" src="https://github.com/user-attachments/assets/d0695c56-adb8-4f63-9f1d-a56099c7e95a" />

```
drop database jetstreaminertia
```

<img width="1097" height="443" alt="imagen" src="https://github.com/user-attachments/assets/64c23b35-d921-4886-9aeb-6a05266fdf91" />

