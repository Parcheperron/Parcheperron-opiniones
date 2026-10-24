# Formulario de opiniones — Parche Perrón

## Archivos
- `index.html`: formulario público. Conserva colores y diseño de la web de enlaces.
- `config.js`: sedes, URL del receptor, logo, política de privacidad.
- `Code.gs`: receptor para Google Apps Script; registra en Sheets, sube fotos a Drive y avisa por correo.
- `assets/`: coloca aquí tu logo `logo-parcheperron.png` y, si tienes licencia web/comercial, la fuente `tipografia-parche.woff2`.

## 1. Preparar Google
1. Crea una hoja de cálculo **privada** en Google Sheets llamada `Opiniones Parche Perrón`.
2. En la primera pestaña, escribe estos encabezados en A1:J1: `Recibido`, `ID`, `Sede`, `Fecha incidente`, `Hora incidente`, `Descripción`, `WhatsApp`, `Foto Drive`, `Estado`, `Origen`.
3. Copia el ID de la hoja desde `https://docs.google.com/spreadsheets/d/ESTE_ES_EL_ID/edit`.
4. Crea una carpeta **privada** de Google Drive llamada `Fotos opiniones Parche Perrón`. Copia su ID desde `https://drive.google.com/drive/folders/ESTE_ES_EL_ID`.
5. Ve a https://script.google.com/ y crea un proyecto. Sustituye su `Code.gs` con el archivo incluido.
6. Cambia `SHEET_ID`, `FOLDER_ID` y `EMAIL_AVISOS` por tus valores.
7. Haz clic en **Implementar → Nueva implementación → Aplicación web**. Configura **Ejecutar como: Yo** y **Quién tiene acceso: Cualquier persona**. Autoriza los permisos. Copia la URL que termina en `/exec`.
8. **No** compartas públicamente ni la hoja ni la carpeta. El acceso público aplica solo a la aplicación web receptora.

## 2. Publicar web en GitHub
1. Crea un repositorio `parcheperron-opiniones` y sube `index.html`, `config.js` y la carpeta `assets` con logo. No necesitas publicar `Code.gs` en el sitio web; se ejecuta en Apps Script.
2. En `config.js`, pega la URL `/exec` en `appsScriptUrl`.
3. Publica una **política de tratamiento de datos personales** válida y pon su enlace en `politicaPrivacidadUrl` antes de activar envíos reales.
4. Configura GitHub Pages: `Settings → Pages`, rama `main`, carpeta `/ (root)`.
5. En `Custom domain` pon `opiniones.parcheperron.com`.
6. En tu proveedor DNS agrega `CNAME` con nombre `opiniones` y destino `parcheperron.github.io` (si ese es tu usuario de GitHub).
7. Espera a que se emita el certificado y activa `Enforce HTTPS`.
8. En el `config.js` de la web de enlaces cambia `opiniones: null` por `opiniones: "https://opiniones.parcheperron.com"`.

## 3. Prueba antes de compartir
- Envía una opinión de prueba sin foto y comprueba que aparezca una fila en Sheets, y que llegue el correo.
- Envía otra con foto JPG pequeña y confirma que el archivo aparezca en la carpeta de Drive y que el enlace de la hoja funcione para el administrador.
- Comprueba que, al marcar «quiero que me contacten», se exige WhatsApp, y que sin marcarlo no se solicita.
- Verifica el mensaje de confirmación en la nueva pestaña. **No** asumas que un envío fue exitoso solo porque se abrió la pestaña.
- Prueba desde celular y con la página de Instagram.

## Límites y seguridad
- Es una solución sencilla y de bajo volumen. Apps Script, MailApp y Drive tienen cuotas y límites de ejecución/tamaño. El archivo se envía codificado en base64, lo que aumenta el tamaño de la solicitud; si hay fallos con imágenes, reduce el máximo de 4 MB o utiliza un backend de formularios especializado.
- La URL de Apps Script es pública: un atacante podría generar envíos falsos o saturar cuotas. Para uso más intensivo se recomienda un servicio con CAPTCHA, límites por IP y controles de abuso.
- El enlace de la imagen queda en la hoja privada. No hagas públicas las fotografías.
- No se valida la propiedad del número de WhatsApp. Evita pedir información sensible y define quién tendrá acceso, plazos de conservación y atención de solicitudes de datos.
- La página abre una pestaña de confirmación del servidor para evitar depender de solicitudes AJAX que suelen presentar restricciones CORS con Apps Script.
- Si cambias `config.js` y el navegador sigue mostrando datos antiguos, aumenta `?v=1` a `?v=2` en `index.html`.
