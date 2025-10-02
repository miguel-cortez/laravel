# JWT y Laravel-Breeze
## Contexto
🚀 **Objetivo** Se pretende utilizar el paquete **laravel-breeze** para la creación y administración de cuentas de usuario y el paquete **tymon/jwt-auth** para la protección de rutas API.

<img width="1510" height="592" alt="imagen" src="https://github.com/user-attachments/assets/6971a6a7-344d-4f3e-ba94-fafe792051db" />

✴️ **Requisito** Haber desarrollado la práctica [Protección de rutas API con JWT](https://github.com/miguel-cortez/laravel/blob/master/jwt/01_guia.md)  

## 1. Instale Laravel Breeze
- Agregue el paquete de **Laravel Breeze** como dependencia en **composer.json**

```
composer require laravel/breeze
```
- Instale el paquete de **Laravel Breeze**
```
php artisan breeze:install
```
- Instale las dependencias listadas en **package.json**
```
npm install
```
- Compile los archivos **css** y **js** para desarrollo
```
npm run dev
```
- Ejecute las migraciones
```
php artisan migrate
```
- Pruebe la aplicación
```
php artisan serve
```

## 2. Configure Vue3

### Instale los paquetes
```
npm install vue@latest vue-router@latest @vitejs/plugin-vue
```

### Configure vite.config.js

```
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue'; // 💡Línea agregada
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/js/app.js'],
            refresh: true,
        }),
        vue(), // 💡 Línea agregada
    ],
});
```

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

📖 ***Comentarios (🔅Los comentarios no son parte del proceso, no haga nada con ellos. Solo son explicaciones)***.  
- El archivo **app.js** importa el archivo **bootstrap.js** y también importa **alpinejs** que se encuentra en **node_modules/alpinejs/**. Se puede instalar con **npm install alpinejs** o ya viene instalado con algún paquete como **laravel-breeze**, **Laravel Jetstream**, etc.
- Las tres líneas correspondientes a **Alpine** se podrían borrar sin problemas para este ejemplo, sin embargo, no permite cargar información con **{{ $slot }}** presente en el archivo **resources/views/layouts/app.blade.php**
  <img width="1053" height="312" alt="imagen" src="https://github.com/user-attachments/assets/e102750d-8174-4a65-9687-9a23b7b8ba01" />
- El bloque de código agregado, crea la aplicación de **Vue3**, configura **axios**, agrega las **rutas** para componentes de **vue-router** y lanza la aplicación de **Vue3**  

## 4. Agregue el archivo resources/js/router.js

⚡Aquí se definen las rutas a las cuales tendrá acceso (podrá utilizar) la aplicación de **Vue3**.  

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
## 5. Agregue el archivo resources/js/components/App.vue

⚡Este es el componente principal de la aplicación de **Vue3**. La carpeta 📁**components** no existe y debe ser creada.    

```Javascript
<template>
  <h1>Hello App!</h1>
  <p><strong>Current route path:</strong>{{ $route.fullPath }} </p>
  
  <div style="margin:10px;border:1px solid gray;padding: 10px;overflow: scroll;">token: {{ token }}</div>
  <br />
  <nav>
    <RouterLink to="/" style="margin:10px;border:1px solid gray;padding: 10px;">Go to Home</RouterLink>
    <RouterLink to="/about" style="margin:10px;border:1px solid gray;padding: 10px;">Go to About</RouterLink>
  </nav>
  <br />

  <button @click="getUsers" style="margin:10px;border:1px solid gray;padding: 10px;">
  Leer usuarios
</button>
<br />
<div style="margin:10px;border:1px solid gray;padding: 10px;">Usuarios: {{ users }}</div>

  <main>
    <RouterView />
  </main>
</template>
<script setup>
import { ref, onMounted } from 'vue'
const token = ref('')
const users = ref([])
onMounted(() => {
  getToken() // Cuando la aplicación Vue3 carga, automáticamente solicita el token (se obtiene de la cookie token)
})
const getToken = async () => {
  try {
    const response = await axios.get("/api/gettoken"); // Obtiene el token que será utilizado posteriormente en cada petición de una ruta protegida.
    token.value = response.data.trim() // Quita espacios en blanco de los extremos del token porque un espacio en blanco altera su contenido.
  } catch (err) {
    console.error(err);
  }
};
const getUsers = async () => {
  try {
    const response = await axios.get("/api/users",{
            headers: {
                Authorization: `Bearer ${token.value}` // Envía el token a la ruta API.
            }
    });
    users.value = response.data
  } catch (err) {
    console.error(err);
  }
};
</script>
```

## 6. Agregue el archivo resources/js/components/HomeView.vue

⚡Esta vista no es relevante para el ejemplo.  

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

## 6. Agregue el archivo resources/js/components/AboutView.vue

⚡Esta vista no es relevante para el ejemplo.  

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
                {{ $slot }}  // 👈 PARA DESPLEGAR INFORMACIÓN FUERA DEL CONTEXTO DE VUE3 (YA EXPLICADO)
                <div id="app"></div> // 💡LÍNEA AGREGADA PARA CARGAR LA APLICACION DE VUE3
            </main>
        </div>
    </body>
</html>
```

📚 **Nota**. Si borra la línea **{{ $slot }}** no se comportará de forma normal el menú del usuario autenticado

<img width="1070" height="614" alt="imagen" src="https://github.com/user-attachments/assets/68fb43b8-9ab8-4346-acec-a2c31ae3d5f6" />

## 8. Modifique el archivo app/Http/Controllers/Auth/RegisteredUserController.php

```
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
// ✂️CÓDIGO OMITIDO
use Illuminate\Support\Facades\Cookie; // 💡 LÍNEA AGREGADA PARA TRABAJAR CON COOKIES
use Tymon\JWTAuth\Facades\JWTAuth; // 💡LINEA AGREGADA PARA TRABAJAR CON JWT.
class RegisteredUserController extends Controller
{
    // ✂️CÓDIGO OMITIDO
    public function store(Request $request): RedirectResponse
    {
        // ✂️CÓDIGO OMITIDO

        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => Hash::make($request->password),
        ]);

        event(new Registered($user));

        Auth::login($user);

        // 💡 INICIA LINEAS AGREGADAS POR MACV
        $user = Auth::user(); // Obtener la información del usuario autenticado
                              // Alternativa $user = $request->user();
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

        return redirect(route('dashboard', absolute: false));
    }
}
```

📚**Nota**. Genera el token y lo almacena en una cookie con nombre **token** justo después de que un usuario se registre. Luego de registrarse, el usuario es redirigido al **dashboard** y en este caso ya tendrá disponible el **token**.  

<img width="478" height="521" alt="imagen" src="https://github.com/user-attachments/assets/dddcd2d3-d88d-454c-8e05-f1926bdb3b6c" />

## 9. Modifique el archivo app/Http/Controllers/Auth/AuthenticatedSessionController.php

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


📚**Nota**. Genera el token y lo almacena en una cookie con nombre **token** justo después de iniciar sesión. Luego de iniciar sesión, el usuario es redirigido al **dashboard** y en este caso ya tendrá disponible el **token**.  

<img width="486" height="401" alt="imagen" src="https://github.com/user-attachments/assets/f64f6722-9ad1-49fd-bcea-871e2ae13b00" />


## 10. Modifique el archivo app/Http/Controllers/AuthController.php

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
## 11. Agregue una ruta a routes/api.php

```php
Route::get('gettoken',[AuthController::class,'getToken']);
```
## 12. Configure para que la cookie token no se encripte

Modique el archivo ***vendor\laravel\framework\src\Illuminate\Cookie\Middleware\EncryptCookies.php***  

Cambie `protected $except = [];` por `protected $except = ['token', ];` 
