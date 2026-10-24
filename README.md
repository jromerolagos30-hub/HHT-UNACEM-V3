# HHT UNACEM · Nueva versión GitHub Pages

Proyecto **nuevo e independiente**. No sobrescribe la aplicación anterior ni migra automáticamente registros.

## Archivos
- `index.html`: interfaz pública con Dashboard, Reporte semanal por empresa, Empresas, Seguimiento, Consolidado, Actividades críticas y Gestión DPA.
- `config.js`: URL del nuevo despliegue Apps Script.
- `Code.gs`: API de Apps Script que usa una nueva hoja de cálculo.
- `calendario_2026.csv`: calendario oficial extraído de la pestaña **Semanas** del consolidado proporcionado; contiene **63 períodos** de reporte, incluidos A/B.

## Instalación
1. Crea **una nueva** hoja de cálculo Google Sheets y copia su ID.
2. Crea un proyecto independiente en [script.google.com](https://script.google.com/), pega el contenido de `Code.gs` en `Código.gs` y reemplaza `DB_ID` por el ID de la nueva hoja.
3. Ejecuta una sola vez `configurarBase()` y autoriza el acceso. Creará las pestañas necesarias.
4. En la pestaña `Calendario_2026`, **importa los datos** de `calendario_2026.csv` debajo de la fila de encabezados existente, sin duplicar encabezados. Completa la columna `anio` con `2026`.
5. Completa `Empresas` (empresa, area, estado=Activo, demás campos opcionales), `Proyectos` (proyecto, area, empresa, estado=Activo) y `Supervisores_UNACEM` (nombre, correo, area, cargo). Los nombres deben coincidir exactamente. La pestaña `Feriados` requiere fechas `AAAA-MM-DD` para el cálculo de días hábiles.
6. Apps Script → Implementar → Nueva implementación → Aplicación web → Ejecutar como **Yo** → Acceso **Cualquier persona** → Implementar. Copia la URL que termina en `/exec`.
7. Pega la URL en `config.js` (variable `window.HHT_API_URL`).
8. Crea un **nuevo** repositorio GitHub y sube `index.html` y `config.js` a la raíz. Settings → Pages → Deploy from branch → main / root. Abre la URL publicada.
9. Verifica lectura de empresas y semanas, luego prueba un reporte con datos ficticios y comprueba que se guarda con nombre de empresa, área y proyecto. El botón de guardar **actualiza** el registro cuando coinciden empresa, área, proyecto y semana.

## Alcance y advertencias
- Se permiten registros y modificaciones sin inicio de sesión, tal como solicitó el usuario. **No es adecuado para datos personales, información confidencial ni producción corporativa**: cualquiera con el enlace puede modificar registros y no hay auditoría de identidad. Para producción se recomienda autenticación, validación de roles y un endpoint de escritura POST protegido. La API usa JSONP/GET por compatibilidad con GitHub Pages y Apps Script, lo cual tampoco es recomendable para escrituras sensibles.
- No se copian registros históricos del sistema anterior ni se eliminan sus datos.
- `Actividades críticas` tiene la interfaz y la distinción DPA/DIA, pero **la integración real del plano y los puntos está pendiente**: se requiere el esquema de lectura y acceso del aplicativo de alto riesgo existente.
- El cumplimiento considera todas las empresas actualmente activas para los períodos filtrados; si se requiere precisión histórica, añadir fecha de inicio/fin de obligación por empresa/proyecto.
- El consolidado usa el mes asignado por el calendario oficial, no un mes inferido de la fecha de guardado.
- El plazo de 10 días hábiles se calcula excluyendo sábados, domingos y los feriados ingresados en `Feriados`. Las acciones correctivas se asocian al evento mediante una clave técnica interna; los reportes HHT guardan nombres legibles.
- **La versión entregada es una base funcional de desarrollo; no ha sido validada contra una implementación real del usuario.**
