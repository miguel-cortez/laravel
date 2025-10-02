# JWT y Laravel-Breeze

*️⃣ Se pretende utilizar el paquete laravel-breeze para la creación y administración de cuentas de usuario y el paquete tymon/jwt-auth para la protección de rutas API.

<img width="1510" height="592" alt="imagen" src="https://github.com/user-attachments/assets/6971a6a7-344d-4f3e-ba94-fafe792051db" />

## 1. Instale el paquete laravel-breeze

Ejecute del Paso 1 al Paso 5 de la siguiente [Guía](https://github.com/miguel-cortez/laravel/blob/master/proyecto_laravel_12/03_laravel_breeze.md)  

## 2. Configure Vue3
Ejecute de ***Paso 1. Instalar vue y las dependencias adicionales*** al ***Paso 2. Configurar vite para que funcione con vue*** de la siguiente [Guía](https://github.com/miguel-cortez/laravel/blob/master/proyecto_laravel_12/04_vue3.md)  

## Para que no encripte

```
vendor\laravel\framework\src\Illuminate\Cookie\Middleware\EncryptCookies.php
```
Modificar en:
```
protected $except = ['token', ];
```
