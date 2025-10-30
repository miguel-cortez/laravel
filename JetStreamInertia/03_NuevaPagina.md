# Creación de una nueva página

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

📚**NOTA** Cuando se ha creado un proyecto de Laravel **Laravel + Jetstream + Inertia + TailwindCSS**, los estilos de **TailwindCSS** se aplican correctamente cuando se crea un componente y luego una página que use dicho componente como se muestra en los 9 y 10. El componente debe está encerrado entre el componente principal **AppLayout**. Estas son configuraciones por defecto.  
