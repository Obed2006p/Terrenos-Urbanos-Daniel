
# · Inmuebles V · | Plataforma de Bienes Raíces

Esta es una plataforma moderna para la compra y venta de inmuebles, construida con React, TypeScript y Firebase. Utiliza una arquitectura estática optimizada para un despliegue rápido y seguro en plataformas como Vercel.

---

## 🚀 Puesta en Marcha (Guía Detallada)

Sigue estos pasos **cuidadosamente** para configurar y desplegar tu propia versión de la aplicación. La causa más común de errores es omitir uno de estos pasos.

### ✅ Paso 1: Configurar tu Proyecto en Firebase

1.  **Crea tu proyecto:** Ve a la [Consola de Firebase](https://console.firebase.google.com/). Haz clic en **"Crear un proyecto"** y sigue los pasos.

2.  **Crea una App Web:**
    *   Dentro de tu proyecto, haz clic en el ícono de engranaje ⚙️ junto a "Descripción general del proyecto" y ve a **Configuración del proyecto**.
    *   En la pestaña **General**, baja hasta la sección "Tus apps".
    *   Haz clic en el ícono de **App web** (`</>`) para registrar una nueva.
    *   Dale un apodo (ej. "Mi Portal Inmobiliario") y haz clic en **"Registrar app"**.
    *   Firebase te mostrará un objeto `firebaseConfig`. **¡Esto es muy importante!**

### ✅ Paso 2: Configurar el Archivo `firebase.ts`

1.  **Copia tu `apiKey`:** Del objeto `firebaseConfig` que viste en el paso anterior, copia el valor de la propiedad `apiKey`. Es una cadena larga de texto.

2.  **Pega la `apiKey` en el código:**
    *   Abre el archivo `firebase.ts` en tu editor de código.
    *   Busca la línea: `apiKey: "TU_API_KEY_AQUI",`
    *   **Reemplaza `"TU_API_KEY_AQUI"`** con la `apiKey` que copiaste. Asegúrate de que quede entre comillas.

### ✅ Paso 3: Habilitar los Servicios de Firebase

1.  **Habilitar Autenticación:**
    *   En el menú lateral izquierdo de la consola de Firebase, ve a **Authentication**.
    *   Haz clic en la pestaña **Sign-in method**.
    *   Habilita los proveedores **"Correo electrónico/Contraseña"** y **"Google"**.

2.  **Autorizar tu Dominio:**
    *   Todavía en **Authentication**, ve a la pestaña **Settings**.
    *   En la sección **Dominios autorizados**, haz clic en **"Agregar dominio"**. Debes agregar `localhost` (para pruebas locales) y el dominio donde desplegarás tu app (ej. `mi-portal.vercel.app`).

3.  **Crear la Base de Datos Firestore:**
    *   En el menú lateral izquierdo, ve a **Firestore Database**.
    *   Haz clic en **"Crear base de datos"**.
    *   Selecciona **"Iniciar en modo de producción"** y haz clic en "Siguiente".
    *   Elige una ubicación para tus servidores (ej. `us-central`) y haz clic en **"Habilitar"**.

4.  **Configurar las Reglas de Seguridad:**
    *   Una vez creada la base de datos, ve a la pestaña **Reglas**.
    *   Reemplaza todo el contenido con las siguientes reglas. Esto permite que cualquiera lea las opiniones, pero solo los usuarios registrados pueden escribir las suyas.

    ```
    rules_version = '2';
    service cloud.firestore {
      match /databases/{database}/documents {
        match /reviews/{reviewId} {
          allow read: if true;
          allow create: if request.auth != null;
          allow update, delete: if request.auth.uid == resource.data.userId;
        }
      }
    }
    ```
    *   Haz clic en **"Publicar"**.

---

### 🚨 Solución de Problemas

Si la aplicación muestra un error de conexión, revisa esta lista punto por punto. El 99% de los problemas se resuelven aquí.

*   **[  ] ¿La `apiKey` en `firebase.ts` es correcta?**
    *   Abre `firebase.ts` y la configuración de tu app en la consola de Firebase. Compara las `apiKey` carácter por carácter. ¡Son sensibles a mayúsculas y minúsculas!

*   **[  ] ¿El proveedor de "Correo/Contraseña" está HABILITADO?**
    *   Ve a `Firebase Console > Authentication > Sign-in method`. El interruptor para "Correo electrónico/Contraseña" debe estar en azul (activado).

*   **[  ] ¿El proveedor de "Google" está HABILITADO?**
    *   Ve a `Firebase Console > Authentication > Sign-in method`. El interruptor para "Google" debe estar en azul (activado).

*   **[  ] ¿Has creado la base de datos de Firestore?**
    *   Ve a `Firebase Console > Firestore Database`. Deberías ver tu base de datos y la colección `reviews` (si ya se escribió alguna), no un botón de "Crear base de datos".

*   **[  ] ¿Las reglas de Firestore son las correctas?**
    *   Ve a `Firebase Console > Firestore Database > Reglas`. El código debe ser idéntico al que se proporciona en el Paso 3.4 de esta guía.

*   **[  ] ¿Está tu dominio autorizado?**
    *   Ve a `Firebase Console > Authentication > Settings > Dominios autorizados`. Si estás probando localmente, `localhost` debe estar en la lista. Si ya desplegaste, la URL de tu app (ej. `nombre-app.vercel.app`) debe estar en la lista.

Una vez que completes todos estos pasos, tu aplicación se conectará a Firebase sin problemas.