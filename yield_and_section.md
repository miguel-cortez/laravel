# YIELD Y SECTION

## Definir una plantilla
```php
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title', 'Mi sitio web')</title>
</head>
<body>
    <header>
        <!-- encabezado común -->
    </header>

    <main>
        @yield('content')
    </main>

    <footer>
        <!-- pie de página común -->
    </footer>
</body>
</html>
```

## Notas
* `@yield()` define secciones que pueden ser sobrescritas por plantillas hijas. 
* En esta plantilla, `@yield('title')` y `@yield('content')` son puntos de inserción para que otras vistas puedan colocar su contenido.
## Crear una plantilla hija
```php
<!-- resources/views/home.blade.php -->
@extends('layouts.app')

@section('title', 'Página de inicio')

@section('content')
    <h1>Bienvenido a la página de inicio</h1>
    <p>Este es el contenido principal de la página.</p>
@endsection
```

## ¿Qué es @stack()?

@stack('nombre') es un marcador de posición en tu plantilla donde quieres que se inserte contenido dinámico que fue definido en otras partes con @push('nombre') o @prepend('nombre').

### Ejemplo práctica.

Plantilla base: `layouts/app.blade.php`  

```php
<!DOCTYPE html>
<html>
<head>
    <title>Mi sitio</title>
    @stack('styles')
</head>
<body>
    @yield('content')

    @stack('scripts')
</body>
</html>
```

Vista hija: `home.blade.php`  

```php
<!DOCTYPE html>
<html>
<head>
    <title>Mi sitio</title>
    @stack('styles')
</head>
<body>
    @yield('content')

    @stack('scripts')
</body>
</html>
```

**Diferencia entre @push y @prepend**  

* `@push('nombre')`: Agrega al final de la pila.

* `@prepend('nombre')`: Agrega al inicio de la pila.