# Instalación de Jetstream

## Paso 7. Instala JetStream

Instala el paquete JetStream en el proyecto:  

```bash
composer require laravel/jetstream
```

<img width="1096" height="723" alt="imagen" src="https://github.com/user-attachments/assets/d84eb5ab-ff85-4697-9684-b10a66989781" />
<br /><br />

Instala y configura JetStream en la aplicación:  

```bash
php artisan jetstream:install
```

Escribe **inertia** para responder a la pregunta **Which Jetstream stack would you like to install?** y luego presiona **ENTER**  

<img width="1108" height="205" alt="imagen" src="https://github.com/user-attachments/assets/6de2aa29-daeb-4e0d-84af-a52bd1f99fc3" />
<br /><br />

La pregunta **Would you like any optional features?** tiene el valor predeterminado **None**. Solo presiona **ENTER**  

<img width="1095" height="173" alt="imagen" src="https://github.com/user-attachments/assets/4a9479e7-4bb3-4b2b-9b64-3326b6ce36a7" />
<br /><br />

La pregunta **Which testing framework do you prefer?** tiene el valor predeterminaod **Pest**. Presiona **ENTER**  

<img width="1096" height="88" alt="imagen" src="https://github.com/user-attachments/assets/cd7815ad-cb20-456e-b16b-d3646863e91c" />
<br /><br />

## ❗ERROR 

Responde **no** a la pregunta **Would you like to re-run your migrations?** y luego presiona **ENTER**  

<img width="1107" height="662" alt="imagen" src="https://github.com/user-attachments/assets/ea8bfb9a-9db8-4943-adb4-21b885fa76cd" />
<br /><br />

🔖**NOTA** El proceso finaliza pero el proyecto queda con errores de configuración.

A fin de hacer pruebas, yo ejecuté el comando **npm install** y mostró la siguiente información:  

<img width="1303" height="481" alt="imagen" src="https://github.com/user-attachments/assets/ac57d1a1-a8ec-428f-b3fa-30300dfb4edd" />
<br /><br />

***🔎 Investigando la causa del error***  

<img width="760" height="138" alt="506810820-5cbaab70-41d0-4573-b967-b27f17bbdd7c" src="https://github.com/user-attachments/assets/f569fe99-1aa5-4817-9b91-39ad920f6535" />
<br /><br />


**🔑 SOLUCIÓN**  

La solución es instalar un paquete de **plugin-vue** compatible con **Vite 7.1.2**  

```bash
npm install @vitejs/plugin-vue@^6.0.0 --save-dev
```

<img width="778" height="279" alt="imagen" src="https://github.com/user-attachments/assets/76e00649-c717-4359-a380-8e9930923992" />
<br /><br />

Comando para verificar que la solución es correcta:  
```
npm install && npm run build
```


<img width="863" height="1017" alt="imagen" src="https://github.com/user-attachments/assets/a0101ba1-dd29-4260-b769-ca6c48993f8a" />
<br /><br />

## Paso 8. Prueba la aplicación

```bash
npm run dev
php artisan serve
```

Pantalla principal  
<img width="1872" height="981" alt="imagen" src="https://github.com/user-attachments/assets/6adb41f8-e67c-4aaf-aa2f-777a861eeb52" />
<br /><br />

Registro de usuarios  

<img width="731" height="720" alt="imagen" src="https://github.com/user-attachments/assets/36de346e-14ec-4873-8bba-b8414759047a" />
<br /><br />

Autenticación de usuarios  

<img width="734" height="651" alt="imagen" src="https://github.com/user-attachments/assets/33b40a18-0fdb-4928-b1e6-c9c0645622b1" />
<br /><br />

<img width="1470" height="885" alt="imagen" src="https://github.com/user-attachments/assets/d22e61f4-f2ec-45aa-ab05-01b392dae88a" />
<br /><br />
