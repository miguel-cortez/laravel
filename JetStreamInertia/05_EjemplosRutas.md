# Ejemplos de rutas

## 1. Ruta básica

```php
use Inertia\Inertia;
use Illuminate\Support\Facades\Route;

Route::get('/dashboard', function () {
    return Inertia::render('Dashboard');
});
```

En tu frontend (por ejemplo **resources/js/Pages/Dashboard.vue**):  

```vue
<template>
  <h1>Bienvenido al panel</h1>
</template>
```
