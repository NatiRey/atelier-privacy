# Revisión de traducciones (ES)

Fecha: 2026-04-23

## Objetivo
Revisar todos los archivos en español del repositorio para detectar texto pendiente de traducir, inconsistencias de idioma o mezclas innecesarias de inglés en contenido redactado para usuarios.

## Archivos revisados

| Archivo | Resultado | Observaciones |
|---|---|---|
| `index.html` | Ajustado | Se corrigió una redacción pendiente: `llenar el formulario` → `completar el formulario` para mantener consistencia de estilo en español formal. |
| `android_fix_notes.md` | Ajustado | Se tradujo el título principal y la referencia `light/dark` a `modo claro/oscuro`. Se conservaron términos técnicos que deben permanecer en inglés (`drawable-night`, nombres de recursos, `Jetpack Compose`). |
| `README.md` | Sin contenido para traducir | El archivo está vacío, no presenta cadenas a revisar. |

## Criterio aplicado
- Traducir frases de interfaz/documentación general al español.
- Mantener en inglés solo:
  - nombres de APIs, permisos, recursos y rutas técnicas (`drawable-night`, `R.drawable...`);
  - nombres propios de producto/plataforma (`Jetpack Compose`, `Play Store`).

## Pendientes detectados
No se detectaron otras traducciones faltantes en los archivos disponibles dentro de este repositorio.

## Nota
Si existen textos faltantes en otro repositorio, rama, carpeta no versionada o en recursos de la app que no están en este proyecto (por ejemplo `strings.xml`, JSON de i18n, etc.), hay que revisar ese origen también porque aquí no están presentes.
