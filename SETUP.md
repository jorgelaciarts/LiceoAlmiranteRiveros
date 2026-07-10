# Configuración de Firebase — Liceo Almirante Riveros

Esta guía te lleva paso a paso desde cero hasta tener el sitio funcionando con base de datos real. No necesitas saber programar.

## 1. Crear el proyecto Firebase

1. Ve a **https://console.firebase.google.com** e inicia sesión con una cuenta Google (idealmente una institucional del liceo).
2. Clic en **"Agregar proyecto"**.
3. Nómbralo, por ejemplo: `liceo-almirante-riveros`.
4. Puedes desactivar Google Analytics (no es necesario). Clic en **"Crear proyecto"**.

## 2. Activar la base de datos (Firestore)

1. En el menú lateral, ve a **Compilación → Firestore Database**.
2. Clic en **"Crear base de datos"**.
3. Elige **"Iniciar en modo producción"**.
4. Selecciona la ubicación **southamerica-east1 (São Paulo)** (la más cercana a Chile) → Habilitar.

## 3. Configurar las reglas de seguridad

1. Dentro de Firestore, ve a la pestaña **"Reglas"**.
2. Reemplaza todo el contenido por esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

3. Clic en **"Publicar"**.

Esto significa: **cualquiera puede leer** el contenido (para que el sitio público funcione), pero **solo alguien con sesión iniciada puede escribir** (es decir, solo tú desde el panel de administración).

## 4. Activar el login del administrador

1. Menú lateral → **Compilación → Authentication** → "Comenzar".
2. En "Métodos de acceso", habilita **"Correo electrónico/contraseña"** → Guardar.
3. Ve a la pestaña **"Users"** → **"Agregar usuario"**.
4. Escribe el correo y contraseña con los que administrarás el sitio (por ejemplo, el correo del liceo). Guarda esta contraseña en un lugar seguro — es la que usarás para entrar a `admin.html`.

> Puedes repetir este paso para crear más de un usuario administrador (por ejemplo, para UTP o Dirección).

## 5. Registrar la app web y copiar la configuración

1. Ve a **⚙️ (ícono de engranaje) → Configuración del proyecto**.
2. En la sección "Tus apps", clic en el ícono **`</>`** (Web).
3. Ponle un apodo, por ejemplo `sitio-liceo` (no marques Firebase Hosting, no lo necesitas).
4. Firebase te mostrará un bloque de código como este:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "liceo-almirante-riveros.firebaseapp.com",
  projectId: "liceo-almirante-riveros",
  storageBucket: "liceo-almirante-riveros.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

5. **Copia esos valores** y pégalos en el archivo `firebase-config.js` que te entregué, reemplazando los valores de ejemplo.

## 6. Subir los archivos a GitHub

Sube estos 3 archivos a la raíz de tu repositorio (reemplazando los anteriores):
- `index.html`
- `admin.html`
- `firebase-config.js` (con tus datos reales del paso 5)

Activa GitHub Pages si aún no lo has hecho: **Settings → Pages → Branch `main`**.

## 7. Cargar el contenido

1. Entra a `tusitio.github.io/admin.html`.
2. Inicia sesión con el correo/contraseña que creaste en el paso 4.
3. Verás un botón **"📥 Cargar datos de ejemplo"** porque la base de datos está vacía. Úsalo para partir con contenido editable, o agrega todo desde cero con los botones **"+ Agregar"** de cada sección.
4. Cada campo se guarda solo, apenas sales de él (verás "Guardado ✓" arriba a la derecha).
5. Entra a `tusitio.github.io/index.html` y confirma que los cambios aparecen.

## Notas importantes

- **Costo:** el plan gratuito de Firebase (Spark) es más que suficiente para el tráfico normal de un liceo.
- **admin.html no necesita estar oculto técnicamente** (las reglas de Firestore ya bloquean escritura sin sesión), pero evita enlazarlo desde el menú público.
- **Recuperar contraseña:** si un administrador la olvida, se restablece desde Firebase Console → Authentication → Users.
- **Más administradores:** repite el paso 4 con otro correo cuando lo necesites.
- **Backups:** Firebase Console → Firestore → puedes exportar los datos manualmente si quieres un respaldo periódico.
