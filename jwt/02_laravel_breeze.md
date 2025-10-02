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

## Para que no encripte

```
vendor\laravel\framework\src\Illuminate\Cookie\Middleware\EncryptCookies.php
```
Modificar en:
```
protected $except = ['token', ];
```
