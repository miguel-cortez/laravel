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

## Paso 7. Instala JetStream

Instala el paquete JetStream en el proyecto:  

```bash
composer require laravel/jetstream
```

<img width="1096" height="723" alt="imagen" src="https://github.com/user-attachments/assets/d84eb5ab-ff85-4697-9684-b10a66989781" />
<br /><br />

Instala y configura JetStream en la aplicación:  

```bash
php artisan jetstream:install
```

Escribe **inertia** para responder a la pregunta **Which Jetstream stack would you like to install?** y luego presiona **ENTER**  

<img width="1108" height="205" alt="imagen" src="https://github.com/user-attachments/assets/6de2aa29-daeb-4e0d-84af-a52bd1f99fc3" />
<br /><br />

La pregunta **Would you like any optional features?** tiene el valor predeterminado **None**. Solo presiona **ENTER**  

<img width="1095" height="173" alt="imagen" src="https://github.com/user-attachments/assets/4a9479e7-4bb3-4b2b-9b64-3326b6ce36a7" />
<br /><br />

La pregunta **Which testing framework do you prefer?** tiene el valor predeterminaod **Pest**. Presiona **ENTER**  

<img width="1096" height="88" alt="imagen" src="https://github.com/user-attachments/assets/cd7815ad-cb20-456e-b16b-d3646863e91c" />
<br /><br />

## ❗ERROR 

Responde **no** a la pregunta **Would you like to re-run your migrations?** y luego presiona **ENTER**  

<img width="1107" height="662" alt="imagen" src="https://github.com/user-attachments/assets/ea8bfb9a-9db8-4943-adb4-21b885fa76cd" />
<br /><br />

🔖**NOTA** El proceso finaliza pero el proyecto queda con errores de configuración.

A fin de hacer pruebas, yo ejecuté el comando **npm install** y mostró la siguiente información:  

<img width="1303" height="481" alt="imagen" src="https://github.com/user-attachments/assets/ac57d1a1-a8ec-428f-b3fa-30300dfb4edd" />
<br /><br />

***🔎 Investigando la causa del error***  

<img width="760" height="138" alt="506810820-5cbaab70-41d0-4573-b967-b27f17bbdd7c" src="https://github.com/user-attachments/assets/f569fe99-1aa5-4817-9b91-39ad920f6535" />
<br /><br />


**🔑 SOLUCIÓN**  

La solución es instalar un paquete de **plugin-vue** compatible con **Vite 7.1.2**  

```bash
npm install @vitejs/plugin-vue@^6.0.0 --save-dev
```

<img width="778" height="279" alt="imagen" src="https://github.com/user-attachments/assets/76e00649-c717-4359-a380-8e9930923992" />
<br /><br />

Comando para verificar que la solución es correcta:  
```
npm install && npm run build
```


<img width="863" height="1017" alt="imagen" src="https://github.com/user-attachments/assets/a0101ba1-dd29-4260-b769-ca6c48993f8a" />
<br /><br />

## Paso 8. Prueba la aplicación

```bash
npm run dev
php artisan serve
```

Pantalla principal  
<img width="1872" height="981" alt="imagen" src="https://github.com/user-attachments/assets/6adb41f8-e67c-4aaf-aa2f-777a861eeb52" />

Registro de usuarios  

<img width="731" height="720" alt="imagen" src="https://github.com/user-attachments/assets/36de346e-14ec-4873-8bba-b8414759047a" />

Autenticación de usuarios  

<img width="734" height="651" alt="imagen" src="https://github.com/user-attachments/assets/33b40a18-0fdb-4928-b1e6-c9c0645622b1" />

<img width="1470" height="885" alt="imagen" src="https://github.com/user-attachments/assets/d22e61f4-f2ec-45aa-ab05-01b392dae88a" />

## Paso 9. Crea un componente Contador.vue

Crear un nuevo archivo llamado **Contador.vue** en el directorio **📁 resources/js/Components**  

```
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment" class="text-sky-400">
    {{ count }}
  </button>
</template>
```
## Paso 10. Crea una página Contador.vue

Crear un nuevo archivo llamado **Contador.vue** en el directorio **📁 resources/js/Pages**  

```
<template>
  <AppLayout>
    <Contador />
  </AppLayout>
</template>

<script setup>
import Contador from '@/Components/Contador.vue'
</script>
```

## Paso 11. Crea una ruta web

```
<?php

use Illuminate\Foundation\Application;
use Illuminate\Support\Facades\Route;
use Inertia\Inertia;

// ✂️ código omitido
Route::get('/contador', function () {
    return Inertia::render('Contador', []);
});
```

📚Cuando se ha creado un proyecto de Laravel **Laravel + Jetstream + Inertia + TailwindCSS**, los estilos de **TailwindCSS** se aplican correctamente cuando se crea un componente y luego una página que use dicho componente como se muestra en los 9 y 10. El componente debe está encerrado entre el componente principal **AppLayout**. Estas son configuraciones por defecto.  

## Información adicional.

<img width="544" height="466" alt="imagen" src="https://github.com/user-attachments/assets/88c32d00-3424-4fd6-bf9f-5bfc4cda17ed" />

<img width="786" height="502" alt="imagen" src="https://github.com/user-attachments/assets/edae30e7-b552-4c86-b084-48b1da5df216" />

<img width="890" height="464" alt="imagen" src="https://github.com/user-attachments/assets/b80abfc7-d3ab-437d-afd4-585023d4df54" />





