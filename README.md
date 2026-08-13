# POCITO - fachada Vercel

Repositorio estatico para publicar POCITO con una URL corta de Vercel mientras el juego y la administracion siguen ejecutandose en Google Apps Script.

## Rutas

- `/` -> juego POCITO
- `/admin` -> administrador POCITO

## Importante

No subir `Code.gs` a este repositorio. El backend permanece en Google Apps Script.

Los HTML de Vercel no muestran pantalla de carga ni contenido intermedio: cargan directamente las Web Apps de Apps Script dentro de iframes a pantalla completa.
