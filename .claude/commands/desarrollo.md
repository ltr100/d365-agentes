---
description: Orquesta un desarrollo completo de D365 F&O: requerimientos, desarrollador, revisor (con un máximo de 2 correcciones) y documentador.
argument-hint: <ruta a una especificación en specs/ o descripción breve de la tarea>
---

Orquesta un desarrollo completo de Dynamics 365 Finance & Operations para: $ARGUMENTS

Ejecuta estos pasos en orden, invocando el subagente indicado en cada uno.

1. **Preparación.** Si el argumento es una ruta a un fichero (por ejemplo `specs/ejemplo.md`), el nombre de la tarea es el nombre del fichero sin extensión. Si es texto libre, crea un nombre corto en minúsculas con guiones (por ejemplo `validacion-diario`). Crea la carpeta `output/<tarea>/codigo/` e indica al usuario el nombre de la tarea.

2. **Requerimientos.** Usa el subagente `requerimientos`, pasándole la especificación (ruta o texto) y la ruta de salida `output/<tarea>/requerimientos.md`. Si el documento contiene dudas marcadas `[BLOQUEANTE]`, detente y pregúntale al usuario antes de seguir.

3. **Desarrollo.** Usa el subagente `desarrollador` con `output/<tarea>/requerimientos.md`. El código debe quedar en `output/<tarea>/codigo/`.

4. **Revisión.** Usa el subagente `revisor` indicándole la tarea `<tarea>`. Lee `output/<tarea>/revision.md` y extrae el valor de `ERRORES_CRITICOS`.

5. **Correcciones (máximo 2 vueltas).** Si `ERRORES_CRITICOS` es mayor que 0 y llevas menos de 2 vueltas, vuelve al paso 3 pasándole al desarrollador `output/<tarea>/revision.md` para que corrija solo los errores críticos y los requisitos no cubiertos. Después repite el paso 4. Si tras 2 vueltas siguen habiendo errores críticos, no sigas corrigiendo: continúa con el paso 6 y repórtalos como pendientes.

6. **Documentación.** Usa el subagente `documentador` con la tarea `<tarea>`.

7. **Resumen final**, conciso y en español:
   - Nombre de la tarea y ruta de `output/<tarea>/`.
   - Requisitos cubiertos sobre el total, y cuáles no lo están.
   - Ficheros de código creados o modificados.
   - Problemas pendientes: errores críticos que sigan abiertos, dudas y `TODO-VERIFICAR`.
   - Recordatorio: revisa el código y crea los labels antes de importarlo en Visual Studio. Aquí no se compila.
