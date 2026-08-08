# Endpoint Notebook

Herramienta web de una sola página para documentar endpoints de API — método, path, body de solicitud, ejemplos de respuesta, errores comunes y notas — sin necesidad de hacer peticiones reales. Todo se guarda localmente en el navegador y se puede exportar o compartir.

## Características

### Organización
- **Proyectos**: cada proyecto agrupa su propia documentación (crear, renombrar, eliminar desde la barra lateral).
- **Carpetas y subcarpetas**: organiza los endpoints en una estructura de árbol dentro de cada proyecto.
- **Arrastrar y soltar**: reordena endpoints y carpetas, o muévelos dentro/fuera de otras carpetas, arrastrando con el mouse.
- **Búsqueda**: filtra el árbol por path o título.
- **Mover entre proyectos**:
  - Individual: cada endpoint tiene un selector "Proyecto" para moverlo a otro proyecto existente.
  - Masivo: el botón "☑ Seleccionar varios" activa checkboxes en el árbol; selecciona varios endpoints/carpetas y muévelos todos juntos a otro proyecto (se conservan las subcarpetas).

### Documentar un endpoint
- Método (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) con color propio.
- Path de la ruta.
- Body de la solicitud (JSON, con validación en vivo).
- Múltiples ejemplos de respuesta (código de estado + etiqueta + JSON).
- Notas con **texto enriquecido**: negrita, cursiva, subrayado, tachado, listas, tipo y tamaño de letra.
- Los campos de texto se ajustan automáticamente al contenido mientras escribes.

### Errores generales
- Cada **proyecto** y cada **carpeta/subproyecto** tiene su propio catálogo de errores comunes (ej. `401` sin token, `500` error interno) — respuestas que no son propias de un único endpoint.
- **Vincular errores a endpoints**: desde el catálogo de errores o desde el propio endpoint puedes vincular uno o varios endpoints a un error general. Una vez vinculado:
  - El error aparece automáticamente como un ejemplo de respuesta más dentro de ese endpoint (marcado como "error general").
  - Editar el error en el catálogo actualiza automáticamente su aparición en todos los endpoints vinculados (es el mismo dato, no una copia).
  - "Quitar vínculo" desvincula el endpoint sin borrar el error del catálogo.

### Exportar y compartir
- **Exportar JSON**: respaldo completo del proyecto activo, importable de vuelta.
- **Exportar Markdown**: documentación legible en `.md`, incluye errores generales y notas con formato convertido a Markdown.
- **Importar**: carga un `.json` exportado previamente (crea un nuevo proyecto).
- **Compartir vista previa**: genera un archivo `.html` autocontenido, de solo lectura, para enviar a un desarrollador (correo, Slack, etc.) sin que necesite esta herramienta. Se puede generar a 3 niveles:
  - Todo el proyecto.
  - Solo una carpeta.
  - Solo un endpoint.

  Los errores generales vinculados a los endpoints compartidos se incluyen automáticamente, aunque compartas solo una carpeta o un único endpoint.

### Persistencia
- Todo se guarda automáticamente en el `localStorage` del navegador. No hay backend ni se envían datos a ningún servidor.
- Los datos son locales a cada navegador/dispositivo — usa "Exportar JSON" para respaldar o mover tu documentación a otra máquina, o "Importar" para cargarla ahí.

## Uso

Abre `index.html` directamente en el navegador, o publícalo con GitHub Pages:

1. Sube este repositorio a GitHub (debe ser público en cuentas gratuitas para usar Pages).
2. En el repo: **Settings → Pages** → Source: `Deploy from a branch` → Branch: `main`, carpeta `/ (root)`.
3. El sitio queda disponible en `https://<usuario>.github.io/endpoint-notebook/`.

### Flujo típico
1. Crea un proyecto con el botón `+` junto al selector de proyecto.
2. Agrega carpetas (`+ Carpeta`) para agrupar endpoints por módulo o feature, y endpoints (`+ Endpoint`) dentro de ellas.
3. Completa método, path, body y ejemplos de respuesta de cada endpoint.
4. Documenta los errores comunes una sola vez en "Errores generales" (a nivel de proyecto o de carpeta) y vincúlalos a los endpoints que correspondan.
5. Cuando necesites compartir el contrato de la API con otro desarrollador, usa "Compartir vista previa" y envía el archivo `.html` generado.

## Formato de datos

El JSON exportado tiene esta forma general:

```json
{
  "id": "...",
  "name": "Nombre del proyecto",
  "errors": [
    { "id": "...", "status": "401", "label": "Sin token", "body": "{...}", "linkedEndpoints": ["id-endpoint-1"] }
  ],
  "nodes": [
    { "id": "...", "type": "folder", "parentId": null, "name": "Autenticación", "errors": [] },
    {
      "id": "id-endpoint-1", "type": "endpoint", "parentId": "id-carpeta",
      "method": "POST", "path": "/auth/login", "body": "{...}", "notes": "<b>HTML</b> con formato",
      "responses": [ { "status": "200", "label": "Éxito", "body": "{...}" } ]
    }
  ]
}
```

## Compatibilidad

Requiere un navegador moderno (Chrome, Safari, Firefox, Edge). El editor de notas usa `document.execCommand`, soportado en todos los navegadores principales de escritorio y móvil actuales.
