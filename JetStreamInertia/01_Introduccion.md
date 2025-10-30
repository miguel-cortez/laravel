# Introducción

## ¿Qué es Jetstream?  

**Laravel Jetstream** es un **"starter kit"** oficial de **Laravel**.  Un paquete que da una base completa y lista para construir aplicaciones modernas con autenticación, manejo de usuarios, perfiles y más.

Jetstream te ahorra tiempo al crear la estructura inicial de una app web, incluyendo:  

* El backend (rutas, controladores, modelos, migraciones, etc.).

* El frontend (vistas o componentes con Livewire o Inertia).

* Y una integración lista para manejar usuarios, sesiones, contraseñas, etc.

### ¿Qué incluye Jetstream?

Cuando instalas Jetstream, obtienes un sistema de autenticación profesional y extensible, con: 

✅ Registro e inicio de sesión  
✅ Verificación de correo electrónico  
✅ Recuperación de contraseñas  
✅ Gestión de perfiles de usuario  
✅ Autenticación en dos pasos (2FA)  
✅ Gestión de sesiones de usuario  
✅ Soporte para equipos (opcional: usuarios trabajando en grupos)  

Todo esto con un diseño limpio basado en **Tailwind CSS**.

### Stack disonibles

<table>
  <tr>
    <th>Stack</th>
    <th>Tecnologías</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>Livewire</td>
    <td>Blade + Livewire</td>
    <td>Ideal si prefieres quedarte dentro del ecosistema PHP y no usar un framework JS completo.</td>
  </tr>
  <tr>
    <td>Inertia</td>
    <td>Vue 3 (o React) + Inertia.js</td>
    <td>Ideal si quieres una experiencia tipo SPA (Single Page Application) sin una API separada.</td>
  </tr>
</table>

## ¿Qué es Inertia?

**Inertia.js** (o simplemente **Inertia**) es una pieza clave en el ecosistema moderno de Laravel, especialmente cuando se usa con **Jetstream** o **Breeze**.

***Inertia.js no es un framework*** ni de frontend ni de backend.  

Es un **puente** que conecta **Laravel (backend)** con **frameworks modernos de frontend** como **Vue 3** o **React**, sin necesidad de una **API REST o GraphQL**.

### ¿Qué problemas resuelve?

Tradicionalmente, si querías una app moderna con Vue o React, debías crear:

* Un **backend API** (Laravel → JSON responses).

* Un **frontend separado** (Vue/React → consume la API).

Esto implica manejar autenticación por **tokens, CORS, estados, etc**.

Con **Inertia** eso desaparece. Puedes seguir usando **rutas y controladores Laravel normales**, y tus vistas serán **componentes Vue o React**.

### ¿Cómo funciona?

1. Laravel responde a una ruta con un “componente de página” de Vue o React.

2. En lugar de devolver HTML, Laravel envía datos (props) a ese componente.

3. Inertia, en el navegador, intercepta los clics y formularios para hacer las peticiones vía AJAX, sin recargar la página.

4. El frontend (Vue/React) solo reemplaza el contenido dinámico — creando una experiencia **SPA**.

### Ejemplo

```php
// routes/web.php
use Inertia\Inertia;

Route::get('/dashboard', function () {
    return Inertia::render('Dashboard', [
        'user' => Auth::user(),
    ]);
});
```

Lo anterior le indica a **Inertia** "Renderiza el componente Dashboard.vue y pásale el usuario autenticado como prop".  

En el lado de Vue (por ejemplo, en resources/js/Pages/Dashboard.vue):

```vue
<template>
  <div>
    <h1>Bienvenido, {{ user.name }}</h1>
  </div>
</template>

<script setup>
defineProps({ user: Object })
</script>
```

## SPA clásicas vs SPA con Inertia

### SPA tradicional

Cuando construyes una **SPA tradicional**, normalmente tienes **dos aplicaciones separadas**:

1. **Backend (API)** — por ejemplo, Laravel que devuelve JSON.  

2. **Frontend (SPA)** — por ejemplo, una app Vue o React que consume esa API.

Esto implica que:  

* Están en **dominios distintos** (por ejemplo, **api.midominio.com** y **app.midominio.com**).

* El frontend se comunica por **HTTP con la API** (usando fetch o axios).

* Y como son dominios distintos, necesitas configurar **CORS** (Cross-Origin Resource Sharing).

*  Además, la autenticación se hace por **tokens** o **JWT**, no por sesiones PHP, porque el frontend no comparte las cookies de sesión del backend.

* Tienes que manejar **estado de usuario**, tokens, expiraciones, refrescos, etc.

### SPA con Inertia

Con **Inertia, no existen dos aplicaciones** separadas. Solo hay una **app Laravel**, que incluye también tu código **Vue** o **React** dentro del mismo proyecto.  
Esto significa que:  

* Laravel sigue siendo el **controlador de rutas** (no una API externa).  

* Las rutas se resuelven con controladores y middlewares normales.  

* El frontend (Vue o React) **vive dentro del mismo dominio y sesión** que Laravel.  

* Las peticiones son **navegaciones internas** (Inertia las envía con `XMLHttpRequest` o `fetch`, pero al mismo dominio).

Por tal motivo:  

ℹ️ No necesitas CORS, porque no hay comunicación entre dominios distintos.  
ℹ️ No necesitas tokens JWT ni API keys, porque Laravel usa su mismo sistema de sesiones PHP y cookies.  
ℹ️ El estado del usuario (autenticado, roles, etc.) se mantiene automáticamente gracias a la sesión.  

## Páginas oficiales
https://jetstream.laravel.com/introduction.html  
https://inertiajs.com/  
https://vuejs.org/  
https://tailwindcss.com/  
