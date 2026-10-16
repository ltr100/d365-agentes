---
description: Revisa una clase, un fichero o un patrón de objetos D365 F&O con el subagente revisor y muestra primero los errores críticos.
argument-hint: <clase, fichero o patrón, por ejemplo "clases WCO*">
---

Revisa con el subagente `revisor` los objetos indicados: $ARGUMENTS

1. **Localiza los ficheros.** Busca con Glob y Grep en el directorio de metadata accesible (el indicado con `--add-dir`) y, si el argumento se refiere a código generado, también en `output/`. Si no encuentras nada, indícalo al usuario y termina aquí.

2. **Prepara la tarea.** El nombre de la tarea es `revision-` seguido de un slug del argumento (por ejemplo `revision-wco`). Crea la carpeta `output/<tarea>/`.

3. **Revisión.** Invoca el subagente `revisor` pasándole la lista de rutas encontradas y el nombre de la tarea. Debe escribir `output/<tarea>/revision.md` sin modificar el código.

4. **Respuesta al usuario.** Lee `output/<tarea>/revision.md` y responde en español:
   - Primero, los errores críticos, con fichero, línea y corrección resumida.
   - Después, los requisitos no cubiertos, si los hay.
   - Por último, la ruta del informe completo.
