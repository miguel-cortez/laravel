# Ejemplos de rutas

## Ejemplo 1. Ruta básica

```php
use Inertia\Inertia;
use Illuminate\Support\Facades\Route;

Route::get('/dashboard', function () {
    return Inertia::render('Dashboard');
});
```

En frontend (por ejemplo **resources/js/Pages/Dashboard.vue**):  

```vue
<template>
  <h1>Bienvenido al panel</h1>
</template>
```

📚 **Nota** Esto renderiza el componente `Dashboard.vue` cuando visitas `/dashboard`.

## Ejemplo 2. Pasando datos desde Laravel a Vue

```php
use Inertia\Inertia;
use Illuminate\Support\Facades\Auth;

Route::get('/profile', function () {
    return Inertia::render('Profile', [
        'user' => Auth::user(),
        'notifications' => [
            'unread' => 3,
            'latest' => 'Tienes un nuevo mensaje'
        ],
    ]);
});
```

En frontend **resources/js/Pages/Profile.vue**  

```vue
<template>
  <div>
    <h2>Hola, {{ user.name }}</h2>
    <p>{{ notifications.latest }}</p>
  </div>
</template>

<script setup>
defineProps({ user: Object, notifications: Object })
</script>
```
📚 **Nota** Así envías datos del backend al frontend sin APIs ni JSON manual.

## Ejemplo 3. Usando un controlador de Laravel

***Definiendo la ruta***  

```php
use App\Http\Controllers\UserController;

Route::get('/users', [UserController::class, 'index'])->name('users.index');
```

***En el controlador***  

```php
use Inertia\Inertia;
use App\Models\User;

class UserController
{
    public function index()
    {
        return Inertia::render('Users/Index', [
            'users' => User::select('id', 'name', 'email')->get(),
        ]);
    }
}
```

En frontend **resources/js/Pages/Users/Index.vue**  

```vue
<template>
  <ul>
    <li v-for="user in users" :key="user.id">
      {{ user.name }} ({{ user.email }})
    </li>
  </ul>
</template>

<script setup>
defineProps({ users: Array })
</script>
```
📚**Nota** Cada vez que entres a `/users`, Laravel renderiza ese componente y le pasa los datos de los usuarios  

## Ejemplo 4. Envío de formularios (POST)

```php
use Illuminate\Http\Request;

Route::post('/users', function (Request $request) {
    $request->validate(['name' => 'required']);
    // Crear usuario, por ejemplo
    return redirect()->back();
});
```

Componente **Users/Create.vue**  

```vue
<template>
  <form @submit.prevent="submit">
    <input v-model="form.name" placeholder="Nombre">
    <button type="submit">Guardar</button>
  </form>
</template>

<script setup>
import { useForm } from '@inertiajs/vue3'

const form = useForm({ name: '' })

function submit() {
  form.post('/users')
}
</script>
```
📚 **Nota** El helper `useForm` de Inertia se encarga de enviar la petición al backend, manejar errores y recargar los datos.

## Ejemplo 5. Ruta con parámetros

```php
Route::get('/users/{user}', function (App\Models\User $user) {
    return Inertia::render('Users/Show', [
        'user' => $user,
    ]);
});
```

En frontend **resources/js/Pages/Users/Show.vue**  

```
<template>
  <h1>Detalles de {{ user.name }}</h1>
</template>

<script setup>
defineProps({ user: Object })
</script>
```

📚 **Nota** Así puedes mostrar información dinámica según el usuario de la URL.  

## Resumen

<table>
    <tr>
        <th>Tradicional</th>
        <th>Inertia</th>
    </tr>
    <tr>
        <td>return view('dashboard', [...])</td>
        <td>return Inertia::render('Dashboard', [...])</td>
    </tr>
    <tr>
        <td>Usa Blade</td>
        <td>Usa Vue o React</td>
    </tr>
    <tr>
        <td>Requiere recarga de página</td>
        <td>Navegación fluida tipo SPA</td>
    </tr>
    <tr>
        <td>Pasa variables a vistas Blade</td>
        <td>Pasa "props" a componentes JS</td>
    </tr>
</table>
