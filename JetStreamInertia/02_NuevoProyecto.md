# Proyecto de Laravel con JetStream + Inertia

## Paso 1. Crear un nuevo proyecto.

📚 En este paso se creará un proyecto básico de Laravel.  

```bash
laravel new JetStreamInertia
```
El valor de la pregunta **Which starter kit would you like to install?** por defecto será **None**. Solo presiona **ENTER**.  

<img width="547" height="381" alt="imagen" src="https://github.com/user-attachments/assets/69b2e510-5515-4e15-afb4-75a5e0225610" />

<br /><br />

El valor de la pregunta **Which testing framework do you prefer?** por defecto será **Pest**. Presiona **ENTER**. 

<img width="435" height="106" alt="imagen" src="https://github.com/user-attachments/assets/06eb6ff3-cd10-4cb8-85b6-8b1daa4d8bf8" />
<br /><br />

El valor de la pregunta **Do you want to install Laravel Boost to improve AI assisted coding? (yes/no)** por defecto será **yes**. Presiona **ENTER**    

<img width="780" height="61" alt="imagen" src="https://github.com/user-attachments/assets/ea0fbad4-cfaa-4950-92ab-7de6e2395779" />
<br /><br />

El valor predeterminado de la pregunta **Which database will your application use? [SQLite]** será **SQLite**. Cambia el valor a **mysql** y presiona **ENTER**.  

<img width="477" height="140" alt="imagen" src="https://github.com/user-attachments/assets/be8bfe51-2026-4b49-a8c6-fe3f3c51c34b" />
<br /><br />

Si prefieres, puedes ejecutar las migraciones. Yo no las ejecuté en este punto porque supuse que debía modificar el archivo **.env** (*️⃣ Ver Paso 3).   

<img width="897" height="58" alt="imagen" src="https://github.com/user-attachments/assets/f871035b-3cbb-4ab4-923c-f3eabf618430" />
<br /><br />

Yo no ejecuté los comandos **npm install** y **npm run build** porque considero que es necesario primero ingresar a la carpeta del proyecto.  

<img width="637" height="50" alt="imagen" src="https://github.com/user-attachments/assets/8bfe5ad8-238b-447d-988e-8ae92cf2b8e3" />
<br /><br />

## Paso 2. Ingresa a la carpeta del proyecto

```bash
cd JetStreamInertia
```

## Paso 3. Modifica el archivo .env

📚Este proceso se realizará en Visual Studio Code. Pero cuando abrir el proyecto, ya estaban las configuraciones, se debe a las opciones elegidas previamente mientras se creaba el proyecto.  

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=jetstreaminertia
DB_USERNAME=root
DB_PASSWORD=
```
## Paso 4. Ejecuta las migraciones

```bash
php artisan migrate
```

El valor predeterminado para la pregunta **Would you like to create it?** es **yes**, lo cual significa que la base de datos **jetstreaminertia** será creada. Solo presiona **ENTER**   
<img width="784" height="205" alt="imagen" src="https://github.com/user-attachments/assets/98a19c18-00a1-4598-bd23-c0743d84b48a" />
<br /><br />

En la siguiente imagen se indica que las migraciones no se ejecutaron con éxito.   

<img width="1106" height="617" alt="imagen" src="https://github.com/user-attachments/assets/6ec8ada5-e919-462e-85a4-5eb6e6bd1ce7" />
<br /><br />

Para solventar este problema será necesario modificar el archivo **app/Providers/AppServiceProvider.php**, borrar la base de datos y ejecutar nuevamente las migraciones.  

Contenido del archivo ***app/Providers/AppServiceProvider.php*** modificado:  
```php
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
Borra la base de datos    

<img width="255" height="311" alt="imagen" src="https://github.com/user-attachments/assets/744be3d4-fcee-4cc3-b924-1e119876d167" />



```bash
drop database jetstreaminertia
```

Ejecuta nuevamente las migraciones.  

<img width="1097" height="443" alt="imagen" src="https://github.com/user-attachments/assets/64c23b35-d921-4886-9aeb-6a05266fdf91" />


## Paso 5. Instala dependencias y compila la aplicación

```
npm install && npm run build
```

<img width="699" height="464" alt="imagen" src="https://github.com/user-attachments/assets/41629a85-6ebd-4685-9236-7a8b338a21ea" />

## Paso 6. Ejecuta la aplicación

```bash
npm run dev
php artisan serve
```

<img width="950" height="431" alt="imagen" src="https://github.com/user-attachments/assets/30e40930-ca1a-45e4-af5f-a60015b10ebd" />

