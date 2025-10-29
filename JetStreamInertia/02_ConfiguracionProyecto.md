## Paso 1. Crear un nuevo proyecto.

📚 En este paso se creará un proyecto básico de Laravel.  

```bash
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

No ejecuté los comandos **npm install** y **npm run build** porque prefiero entrar a la carpeta del nuevo proyecto y ejecutar desde allí los comandos.  

<img width="637" height="50" alt="imagen" src="https://github.com/user-attachments/assets/8bfe5ad8-238b-447d-988e-8ae92cf2b8e3" />

## Paso 2. Ingresa a la carpeta del proyecto

```bash
cd JetStreamInertia
```

## Paso 2. Modificar el archivo .env

📚Este proceso se realizará en Visual Studio Code. Pero cuando abrir el proyecto, ya estaban las configuraciones, se debe a las opciones elegidas previamente mientras se creaba el proyecto.  

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=jetstreaminertia
DB_USERNAME=root
DB_PASSWORD=
```
## Paso 3. Ejecute las migraciones

```bash
php artisan migrate
```

En la siguiente pregunta, solo presiones ENTER, por defecto creará la base de datos.  
<img width="784" height="205" alt="imagen" src="https://github.com/user-attachments/assets/98a19c18-00a1-4598-bd23-c0743d84b48a" />

Falló la ejecución de las migraciones.  

<img width="1106" height="617" alt="imagen" src="https://github.com/user-attachments/assets/6ec8ada5-e919-462e-85a4-5eb6e6bd1ce7" />

Para solucionar el problema modifica el archivo **app/Providers/AppServiceProvider.php** 

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


## Paso 4. Instala dependencias y compila la aplicación

```
npm install && npm run build
```

<img width="699" height="464" alt="imagen" src="https://github.com/user-attachments/assets/41629a85-6ebd-4685-9236-7a8b338a21ea" />

## Paso 5. Ejecuta la aplicación

```bash
npm run dev
php artisan serve
```

<img width="950" height="431" alt="imagen" src="https://github.com/user-attachments/assets/30e40930-ca1a-45e4-af5f-a60015b10ebd" />

## Paso 6. Instalar JetStream

```bash
composer require laravel/jetstream
```

<img width="1096" height="723" alt="imagen" src="https://github.com/user-attachments/assets/d84eb5ab-ff85-4697-9684-b10a66989781" />

```bash
php artisan jetstream:install
```

Selecciona la opción **inertia** y presiona **ENTER**    

<img width="1108" height="205" alt="imagen" src="https://github.com/user-attachments/assets/6de2aa29-daeb-4e0d-84af-a52bd1f99fc3" />

Presiona ENTER (opcionalmente se puede utilizar otra característica)  

<img width="1095" height="173" alt="imagen" src="https://github.com/user-attachments/assets/4a9479e7-4bb3-4b2b-9b64-3326b6ce36a7" />

Presiona ENTER (Por defecto se usará **Pest** para realizar pruebas unitarias)  

<img width="1096" height="88" alt="imagen" src="https://github.com/user-attachments/assets/cd7815ad-cb20-456e-b16b-d3646863e91c" />

❗Ocurrió un error:  

<img width="1107" height="662" alt="imagen" src="https://github.com/user-attachments/assets/ea8bfb9a-9db8-4943-adb4-21b885fa76cd" />

No realizarán nuevamente las migraciones en este momenento.  

<img width="1303" height="481" alt="imagen" src="https://github.com/user-attachments/assets/ac57d1a1-a8ec-428f-b3fa-30300dfb4edd" />

Comentarios acerca del error: 

<img width="760" height="138" alt="506810820-5cbaab70-41d0-4573-b967-b27f17bbdd7c" src="https://github.com/user-attachments/assets/f569fe99-1aa5-4817-9b91-39ad920f6535" />

<pre>
'Log files:
C:\Users\macv\AppData\Local\npm-cache\_logs\2025-10-28T16_27_24_885Z-debug-0.log

# npm resolution error report

While resolving: undefined@undefined
Found: vite@7.1.12
node_modules/vite
  dev vite@"^7.0.7" from the root project

Could not resolve dependency:
peer vite@"^5.0.0 || ^6.0.0" from @vitejs/plugin-vue@5.2.4
node_modules/@vitejs/plugin-vue
  dev @vitejs/plugin-vue@"^5.0.0" from the root project

Fix the upstream dependency conflict, or retry
this command with --force or --legacy-peer-deps
to accept an incorrect (and potentially broken) dependency resolution.
</pre>

**Solución**  

```bash
npm install @vitejs/plugin-vue@^6.0.0 --save-dev
```

<img width="778" height="279" alt="imagen" src="https://github.com/user-attachments/assets/76e00649-c717-4359-a380-8e9930923992" />

Listo  

<img width="863" height="1017" alt="imagen" src="https://github.com/user-attachments/assets/a0101ba1-dd29-4260-b769-ca6c48993f8a" />


## Paso 8. Probar la aplicación

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

## Paso 9. Crear un componente Contador.vue

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
## Paso 10. Crear una página Contador.vue

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

## Paso 11. Crear una ruta para la página de Contador

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





