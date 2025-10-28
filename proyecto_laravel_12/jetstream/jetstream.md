## Crear el proyecto
```
laravel new jetinertia
```

<img width="563" height="375" alt="imagen" src="https://github.com/user-attachments/assets/20ccab95-ff60-4f06-8a5a-d168f1d4d136" />

<img width="763" height="117" alt="imagen" src="https://github.com/user-attachments/assets/8b614d62-aea7-4e19-ac68-b68b0caffd3d" />

<img width="453" height="94" alt="imagen" src="https://github.com/user-attachments/assets/7951dac0-57dd-4f5b-b003-27b7f0e04e6b" />

<img width="653" height="64" alt="imagen" src="https://github.com/user-attachments/assets/5c5dfadc-e8a5-41e2-9c24-f1452271ac25" />

## Instalar inertia server-side
```
cd jetinertia
composer require inertiajs/inertia-laravel
```

<img width="923" height="404" alt="imagen" src="https://github.com/user-attachments/assets/103db55b-fb21-4739-9cec-40736504af52" />

## Middleware

```
php artisan inertia:middleware
```

Contenido del Middleware:  

```
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Inertia\Middleware;

class HandleInertiaRequests extends Middleware
{
    /**
     * The root template that's loaded on the first page visit.
     *
     * @see https://inertiajs.com/server-side-setup#root-template
     *
     * @var string
     */
    protected $rootView = 'app';

    /**
     * Determines the current asset version.
     *
     * @see https://inertiajs.com/asset-versioning
     */
    public function version(Request $request): ?string
    {
        return parent::version($request);
    }

    /**
     * Define the props that are shared by default.
     *
     * @see https://inertiajs.com/shared-data
     *
     * @return array<string, mixed>
     */
    public function share(Request $request): array
    {
        return [
            ...parent::share($request),
            //
        ];
    }
}
```

## Plantailla principal

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{ config('app.name', 'Laravel') }}</title>
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.bunny.net">
    <link href="https://fonts.bunny.net/css?family=instrument-sans:400,500,600" rel="stylesheet" />
    @vite(['resources/css/app.css', 'resources/js/app.js'])
    @inertiaHead
  </head>
  <body>
    @inertia
  </body>
</html>
```

## Vita Welcome

```
<script setup>
import { ref } from 'vue'
</script>

<template>
    Bienvenidos
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

## Vista Macv

```
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<template>
  <button @click="count++" class="text-blue-400">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

## Rutas web

```
<?php

use Illuminate\Support\Facades\Route;
use Inertia\Inertia;

/*
Route::get('/', function () {
    return view('welcome');
});
*/


Route::get('/', function () {
    return Inertia::render('Welcome', [
        'appName' => config('app.name'),
    ]);
});

Route::get('/macv', function () {
    return Inertia::render('Macv', [
        'appName' => config('app.name'),
    ]);
});
Route::get('/algo', function () {
    return Inertia::render('Algo', [
        'appName' => config('app.name'),
    ]);
});
```

## vite.config.js

```
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import tailwindcss from '@tailwindcss/vite';
import vue from '@vitejs/plugin-vue';
export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
        tailwindcss(),
        vue(),
    ],
});
```

## app.js

```
import './bootstrap';

import { createApp, h } from 'vue'
import { createInertiaApp } from '@inertiajs/vue3'

createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.vue', { eager: true })
    return pages[`./Pages/${name}.vue`]
  },
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el)
  },
})
```


## Instalar jetstream

```
composer require laravel/jetstream
```

```
php artisan jetstream:install inertia
```

<img width="1112" height="502" alt="imagen" src="https://github.com/user-attachments/assets/eba6cc16-92ae-4295-ba90-dd5940ed53bf" />

## Solución

<img width="760" height="138" alt="imagen" src="https://github.com/user-attachments/assets/5cbaab70-41d0-4573-b967-b27f17bbdd7c" />

```
npm install @vitejs/plugin-vue@^6.0.0 --save-dev
```

```
npm run build
npm run dev
```

## Referencias
https://jetstream.laravel.com/installation.html
https://inertiajs.com/server-side-setup

