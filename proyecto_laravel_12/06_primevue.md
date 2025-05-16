# PRIMEVUE

## Paso 1. Instalar PrimeVue
```
npm install primevue @primeuix/themes
npm install primevue primeicons
```

## Paso 2. Modificar appVue.js

```
import { createApp } from 'vue';
import App from './components/App.vue';
import router from './router';
import PrimeVue from 'primevue/config';
import Aura from '@primeuix/themes/aura';
import Button from "primevue/button"
import Toast from 'primevue/toast';
import ToastService from 'primevue/toastservice';
import 'primeicons/primeicons.css';

const app = createApp(App);
app.use(router);
app.use(PrimeVue, {
    theme: {
        preset: Aura
    }
});
app.use(ToastService);

app.component('Button', Button);
app.component('Toast', Toast);

app.mount('#app');
```

# Referencias

https://primevue.org/
