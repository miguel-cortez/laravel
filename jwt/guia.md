🪲DOCUMENTACIÓN EN PROCESO

# Protección de rutas API con JWT

## Creación del proyecto.
```
composer create-project laravel/laravel example-jwt-laravel-12
```

## Habilitar rutas API

```
php artisan install:api
```

## Instalación de paquete de JWT para Laravel

[Referencia](https://jwt-auth.readthedocs.io/en/develop/laravel-installation/)

ℹ️**Thunder Client**, extensión de Visual Studio Code para hacer pruebas de rutas simila a **Postman**

```
composer require tymon/jwt-auth
```

## Registrar el proveedor

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

Ahora debe existir el siguiente archivo: `config/jwt.php` 

## Generando una clave secreta

```
php artisan jwt:secret
```

Esto agrega una clave secreta en el archivo `.env` y muesta la clave en la terminal como sigue:  

jwt-auth secret [**Yn2uTgg40nqfQn6HDk4ecbTWcJ...I588Y9ZHUkKkXV9UsqBl**] set successfully.  

## Actualizar Models\User

```
use Tymon\JWTAuth\Contracts\JWTSubject;
```

**Se deben implementar los siguientes dos métodos:**  

```

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
```

El modelo **User** lucirá como el siguiente código:  

```
<?php

namespace App\Models;

// UNA LÍNEA OMITIDA

use Tymon\JWTAuth\Contracts\JWTSubject;

// CÓDIGO OMITIDO

class User extends Authenticatable implements JWTSubject
{

    // CÓDIGO OMITIDO

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }

}
```



# Referencias  

https://youtu.be/jqaAfWQYb0Y
https://jwt-auth.readthedocs.io/en/develop/quick-start/
