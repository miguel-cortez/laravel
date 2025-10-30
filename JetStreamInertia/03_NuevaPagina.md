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

📚Cuando se ha creado un proyecto de Laravel **Laravel + Jetstream + Inertia + TailwindCSS**, los estilos de **TailwindCSS** se aplican correctamente cuando se crea un componente y luego una página que use dicho componente como se muestra en los 9 y 10. El componente debe está encerrado entre el componente principal **AppLayout**. Estas son configuraciones por defecto.  

## Información adicional.

<img width="544" height="466" alt="imagen" src="https://github.com/user-attachments/assets/88c32d00-3424-4fd6-bf9f-5bfc4cda17ed" />

<img width="786" height="502" alt="imagen" src="https://github.com/user-attachments/assets/edae30e7-b552-4c86-b084-48b1da5df216" />

<img width="890" height="464" alt="imagen" src="https://github.com/user-attachments/assets/b80abfc7-d3ab-437d-afd4-585023d4df54" />





