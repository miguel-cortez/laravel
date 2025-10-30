# Creación de una nueva página

📚**NOTAS**  
💠 Cuando se crea un proyecto de **Laravel + Jetstream + Inertia + TailwindCSS**, para que los estilos de **TailwindCSS** se apliquen es necesario crear un componente en **📁 resources/js/Components** y una página en **📁 resources/js/Pages** que utilice al componente como se mostrará más adelante.

💠 Las configuraciones de la aplicación **vue** han sido realizadas de forma automática y son totalmente funcionales; pero debemos agregar nuestros archivos en los lugares predefinidos si no queremos alterar la estructura original del proyecto.  

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
