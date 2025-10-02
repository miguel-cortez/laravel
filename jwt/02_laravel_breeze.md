# JWT y Laravel-Breeze

📚He instalado Laravel-Breeze en el proyecto donde se configuró JWT y el proceso de autenticación funciona correctamente aún cuando el usuario fue creado con la opción Register de Laravel-Breeze.

<img width="1510" height="592" alt="imagen" src="https://github.com/user-attachments/assets/6971a6a7-344d-4f3e-ba94-fafe792051db" />

## Pistas para desencriptar NO HA FUNCIONADO

```
use Illuminate\Support\Facades\Crypt;
use Illuminate\Support\Facades\Cookie;

// Supongamos que la cookie se llama "mi_cookie"
$cookieValue = request()->cookie('mi_cookie');

$decrypted = Crypt::decrypt($cookieValue);

dd($decrypted);  // Aquí verás el contenido original
```

## Para que no encripte

```
vendor\laravel\framework\src\Illuminate\Cookie\Middleware\EncryptCookies.php
```
Modificar en:
```
protected $except = ['token', ];
```
