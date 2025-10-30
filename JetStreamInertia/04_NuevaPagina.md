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
## Paso 2. Crea una página Contador.vue

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

## Paso 3. Agrega una nueva ruta web

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
