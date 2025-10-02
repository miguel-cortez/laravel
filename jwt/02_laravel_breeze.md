# JWT y Laravel-Breeze

*️⃣ Se pretende utilizar el paquete laravel-breeze para la creación y administración de cuentas de usuario y el paquete tymon/jwt-auth para la protección de rutas API.

<img width="1510" height="592" alt="imagen" src="https://github.com/user-attachments/assets/6971a6a7-344d-4f3e-ba94-fafe792051db" />

## 1. Instale el paquete laravel-breeze

Ejecute del Paso 1 al Paso 5 de la siguiente [Guía](https://github.com/miguel-cortez/laravel/blob/master/proyecto_laravel_12/03_laravel_breeze.md)  

## 2. Configure Vue3
Ejecute de ***Paso 1. Instalar vue y las dependencias adicionales*** al ***Paso 2. Configurar vite para que funcione con vue*** de la siguiente [Guía](https://github.com/miguel-cortez/laravel/blob/master/proyecto_laravel_12/04_vue3.md)  

## 3. Modifique el archivo resources/js/app.js

```Javascript
import './bootstrap';

import Alpine from 'alpinejs';

window.Alpine = Alpine;

Alpine.start();

// 💡INICIA BLOQUE AGREGADO
import { createApp } from 'vue'

import axios from 'axios'
window.axios = axios;


import router from './router'
import App from './components/App.vue'

const app  = createApp(App).use(router)
app.config.globalProperties.$axios = axios;
app.mount('#app')

// 💡FINALIZA BLOQUE AGREGADO
```

## 4. Agregue el archivo resources/router.js
```Javascript
import { createMemoryHistory, createRouter } from 'vue-router'

import HomeView from './components/HomeView.vue'
import AboutView from './components/AboutView.vue'

const routes = [
  { path: '/', component: HomeView },
  { path: '/about', component: AboutView },
]

const router = createRouter({
  history: createMemoryHistory(),
  routes,
})

export default router
```
## 5. Agregue el archivo resources/components/App.vue
```Javascript
<template>
  <h1>Hello App!</h1>
  <p><strong>Current route path:</strong>{{ $route.fullPath }} </p>
  token: {{ tk }}

<button @click="getUsers">
  Leer usuarios
</button>

  <nav>
    <RouterLink to="/" class="color:purple">Go to Home</RouterLink>
    <RouterLink to="/about">Go to About</RouterLink>
  </nav>
  <main>
    <RouterView />
  </main>
</template>
<script setup>
import { ref, onMounted } from 'vue'
const tk = ref('')
onMounted(() => {
  getToken()
})
const getToken = async () => {
  try {
    const response = await axios.get("/api/gettoken");
    tk.value = response.data.trim()
    console.log("tk=" + tk.value)
  } catch (err) {
    console.error(err);
  }
};
const getUsers = async () => {
  try {
    const response = await axios.get("/api/users",{
            headers: {
                Authorization: `Bearer ${tk.value}`
            }
    });
    console.log(response.data)
  } catch (err) {
    console.error(err);
  }
};
</script>
```

## 6. Agregue el archivo resources/components/HomeView.vue
```Javascript
<script>
export default {
  methods: {
    goToAbout() {
      this.$router.push('/about')
    },
  },
}
</script>

<template>
  <h2>HomeView</h2>
  <button @click="goToAbout">Go to About</button>
</template>
```

## 6. Agregue el archivo resources/components/AboutView.vue
```
<script setup>
import { computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const router = useRouter()
const route = useRoute()

const search = computed({
  get() {
    return route.query.search ?? ''
  },
  set(search) {
    router.replace({ query: { search } })
  }
})
</script>

<template>
  <h2>AboutView</h2>
  <label>
    Search: <input v-model.trim="search" maxlength="20">
  </label>
</template>
```

## 7. Modifique el archivo resources/views/layouts/app.blade.php
```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
    // CÓDIGO OMITIDO
    </head>
    <body class="font-sans antialiased">

          // CÓDIGO OMITIDO

            <!-- Page Content -->
            <main>
                {{ $slot }}
                <div id="app"></div> // 💡LÍNEA AGREGADA
            </main>
        </div>
    </body>
</html>
```

## 8. Modifique el archivo app/Http/Controllers/Auth/AuthenticatedSessionController.php

```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Http\Requests\Auth\LoginRequest;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\View\View;
use Illuminate\Support\Facades\Cookie; // 💡 LÍNEA AGREGADA PARA TRABAJAR CON COOKIES
use Tymon\JWTAuth\Facades\JWTAuth; // 💡LINEA AGREGADA PARA TRABAJAR CON JWT.
class AuthenticatedSessionController extends Controller
{
    /**
     * Display the login view.
     */
    public function create(): View
    {
        return view('auth.login');
    }

    /**
     * Handle an incoming authentication request.
     */
    public function store(LoginRequest $request): RedirectResponse
    {
        $request->authenticate();

        $request->session()->regenerate();

        // 💡 INICIA LINEAS AGREGADAS POR MACV
        $user = Auth::user(); // Obtener la información del usuario autenticado
        $token = JWTAuth::fromUser($user); // para generar el token

        // Crear cookie con HttpOnly activado
        Cookie::queue(cookie(
            'token',    // Nombre de variable en cookie
            $token,     // Valor de la variable
            2,          // duración en minutos
            '/',        // ruta
            null,       // dominio específico
            true,       // secure (HTTPS)
            true        // httpOnly (🔒) para que no se pueda leer con JavaScript
        ));
        // 💡 FINALIZA LINEAS AGREGADAS POR MACV
        return redirect()->intended(route('dashboard', absolute: false));
    }

    /**
     * Destroy an authenticated session.
     */
    public function destroy(Request $request): RedirectResponse
    {
        Auth::guard('web')->logout();

        $request->session()->invalidate();

        $request->session()->regenerateToken();

        return redirect('/');
    }
}
```

## 9. Modifique el archivo app/Http/Controllers/AuthController.php

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator; // LINEA AGREGADA
use Illuminate\Support\Facades\Hash; // LINEA AGREGADA
use App\Models\User; // LINEA AGREGADA
use Tymon\JWTAuth\Facades\JWTAuth; // LINEA AGREGADA
use Illuminate\Support\Facades\Auth;
class AuthController extends Controller
{
    // CÓDIGO OMITIDO

    // 💡 FUNCION AGREGADA
    public function getToken(){
        return request()->cookie('token');
    }
}
```
## 10. Agregue una ruta a routes/api

```php
Route::get('gettoken',[AuthController::class,'getToken']);
```
## 11. Configure para que la cookie token no se encripte

Modique el archivo ***vendor\laravel\framework\src\Illuminate\Cookie\Middleware\EncryptCookies.php***  

Cambie `protected $except = [];` por `protected $except = ['token', ];` 
