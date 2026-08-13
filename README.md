# POCITO - Wrapper para Vercel

Este repositorio NO reemplaza Google Apps Script. Solo publica URLs mas cortas y limpias en Vercel.

## URLs

- `/` muestra la Web App principal de Apps Script.
- `/admin` muestra la Web App de administracion (`?page=admin`).

Vercel usa `cleanUrls: true`, por lo que `admin.html` queda disponible como `/admin`.

## Estructura

```text
pocito-vercel/
├── index.html
├── admin.html
├── vercel.json
├── .gitignore
└── README.md
```

## Importante en Google Apps Script

Para que el contenido pueda mostrarse dentro de un iframe alojado en Vercel, el `HtmlOutput` que devuelve tu `doGet(e)` debe usar:

```javascript
.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
```

En el Code.gs actual de POCITO esta configuracion YA existe en doGet(), por lo que no hace falta modificarla para Vercel.

Ejemplo generico:

```javascript
function doGet(e) {
  const pagina = e && e.parameter && e.parameter.page === 'admin' ? 'admin' : 'index';

  const output = HtmlService
    .createTemplateFromFile(pagina)
    .evaluate();

  return output
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}
```

Si tu proyecto inyecta variables, incluye otros archivos o hace verificaciones de acceso, conserva toda esa logica.

## Flujo de actualizacion

1. Modificar `Code.gs`, `index.html` o `admin.html` en Google Apps Script.
2. Guardar los cambios.
3. Crear una nueva version de la Web App y actualizar LA MISMA implementacion existente.
4. Mantener el mismo Deployment ID / URL `/exec`.
5. Recargar la URL de Vercel.

No hace falta modificar ni redesplegar Vercel por cambios normales dentro de Apps Script.

## Subir a GitHub

1. Crear un repositorio nuevo en GitHub.
2. Subir todos los archivos de esta carpeta a la raiz del repositorio.
3. Confirmar que `index.html` y `vercel.json` queden en la raiz, no dentro de una carpeta adicional.

## Publicar en Vercel

1. En Vercel, crear un proyecto nuevo e importar el repositorio de GitHub.
2. Framework Preset: `Other`.
3. Root Directory: raiz del repositorio.
4. No hace falta Build Command.
5. Desplegar.

Luego tendras URLs como:

```text
https://TU-PROYECTO.vercel.app
https://TU-PROYECTO.vercel.app/admin
```

## Administrador y Google Workspace

La URL del administrador pertenece al dominio `grupoproaco.com`. Segun la sesion del navegador y las politicas de Google Workspace, Google puede requerir autenticacion en una pestaña de nivel superior. Por eso `admin.html` incluye un enlace de apertura directa como respaldo.


## Seguridad del backend

No subas `Code.gs` a este repositorio. El backend debe permanecer dentro de Google Apps Script. La copia segura entregada por separado lee la clave del administrador desde la propiedad de script `POCITO_ADMIN_PASSWORD`.
