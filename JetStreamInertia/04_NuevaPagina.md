# Creación de una nueva página

📚**NOTAS**  

💠Cuando se instala **Jetstream** se puede elegir el **stack** a utilizar. Tiene dos opciones, ***1. Vue with Inertia*** y ***2. Livewire***.  

💠Debido a que durante la instalación de **Jetstream** se seleccionó ***1. Vue with Inertia*** automáticamente se instaló **Vue 3** y se configuró una aplicación de **Vue**.  

- Crea una carpeta llamada **📁 resources/js/Components** para guardar componentes.
- Crea una carpeta llamada **📁 resources/js/Pages** para guardar páginas.
- Ajusta los archivos **vite.config.js** y **app.js** para trabajar con **Vue 3**.
- Instala dependencias necesarias que luego se instalan con **npm install**, etc.  

💠Las configuraciones de la aplicación **vue** han sido realizadas de forma automática y son totalmente funcionales; pero debemos agregar nuestros archivos en los lugares predefinidos si no queremos alterar la estructura original del proyecto y además, para que se apliquen los estilos de **tailwindcss** sin necesidad de realizar configuraciones adicionales.  

## Paso 1. Crea un componente Contador.vue

Crear un nuevo archivo llamado **Contador.vue** en el directorio **📁 resources/js/Components**  

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}

defineProps({
    titulo: String,
});

</script>

<template>
  <div>
    <div class="p-6 lg:p-8 bg-white border-b border-gray-200">
      <h1 class="text-blue-700">{{ titulo }}</h1>
      <button @click="increment" class="border border-blue-500 bg-cyan-500 hover:bg-cyan-600 hover:cursor-pointer font-semibold text-white py-2 px-4 rounded">
        {{ count }}
      </button>
      <br /><br />
      <button class="border border-yellow-500 text-yellow-600 hover:bg-yellow-500 hover:text-white font-semibold py-2 px-4 rounded transition">
        Advertencia
      </button>

      <br /><br />

<button class="flex items-center gap-2 bg-green-500 hover:bg-green-600 text-white font-medium py-2 px-4 rounded shadow-md hover:shadow-lg transition">
  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
  </svg>
  Confirmar
</button>

    </div>
  </div>
</template>
```
## Paso 2. Crea una página Contador.vue

Crear un nuevo archivo llamado **Contador.vue** en el directorio **📁 resources/js/Pages**  

```vue
<script setup>
import AppLayout from '@/Layouts/AppLayout.vue';
import Contador from '@/Components/Contador.vue'
defineProps({
    titulo: String,
});
</script>

<template>

  <AppLayout title="Contador">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Contador
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <Contador :titulo="titulo" />
                    <Contador titulo="Componente con título estático" />
                </div>
            </div>
        </div>


  </AppLayout>
</template>
```

## Paso 3. Agrega una nueva ruta web

```php
<?php

use Illuminate\Foundation\Application;
use Illuminate\Support\Facades\Route;
use Inertia\Inertia;

// ✂️ código omitido
Route::get('/contador', function () {
    return Inertia::render('Contador', [ 'titulo' => "Componente con título dinámico" ]);
});
```
## Resultado

<img width="1351" height="767" alt="imagen" src="https://github.com/user-attachments/assets/d08ab224-70b0-479a-a01c-74bc87190701" />



