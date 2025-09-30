# Pruebas unitarias con PHPUnit

**Clase prueba inlcuida en el proyecto**

```
<?php

namespace Tests\Unit;

use PHPUnit\Framework\TestCase;

class ExampleTest extends TestCase
{
    /**
     * A basic test example.
     */
    public function test_that_true_is_true(): void
    {
        $this->assertTrue(true);
    }
}
```

**Ejecutar las pruebas**  

Como solo se tiene la clase **ExampleTest** solo se prueaba la función definida en la clase.  

```
php artisan test
```
<img width="509" height="325" alt="imagen" src="https://github.com/user-attachments/assets/802aafb9-d3fa-4155-82e3-25c069cd953c" />
