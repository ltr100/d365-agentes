# D365 Finance & Operations: agentes de desarrollo

Este repositorio orquesta subagentes de Claude Code para especificar, desarrollar, revisar y documentar objetos de Microsoft Dynamics 365 Finance & Operations.

## Contexto

- **Plataforma:** Dynamics 365 Finance & Operations. Módulo principal: Finance (diario general, cuentas, dimensiones financieras, cierres).
- **Lenguajes:** X++ para la lógica de negocio y el AOT (metadata en `PackagesLocalDirectory`). C# solo para integraciones.
- **Herramientas del proyecto:** Visual Studio con el SDK de Dynamics 365, Data Management Framework (DMF) para migración y datos, SSRS y Electronic Reporting (ER) para informes.
- **Metadata:** Claude accede al metadata con `claude --add-dir "K:\AosService\PackagesLocalDirectory\<Modelo>"`.
- **Entorno:** este repositorio no compila código. Los entregables se revisan aquí y después se importan en Visual Studio.

## Convenciones de código

- Prefijo de objetos personalizados: `<PREFIJO>`. Defínelo por proyecto.
- Labels con formato `@<Modelo>:<Id>`. No uses textos fijos.
- Extensiones y event handlers antes que overlayering.
- Nunca `select` dentro de bucles. Usa operaciones set-based o carga previa.
- `ttsBegin` y `ttsCommit` acotados, con `ttsAbort` en los errores.
- Clases `final` y tipado fuerte. Los procesos batch usan SysOperation.
- Cada método lleva un comentario `// Cubre: REQ-XX` con los requisitos que implementa.

## Agentes

| Agente | Función | Herramientas | Escribe en |
|---|---|---|---|
| `requerimientos` | Convierte una especificación en requisitos numerados | Read, Glob, Grep, Write | `requerimientos.md` |
| `desarrollador` | Implementa X++ (o C#) a partir de los requisitos | Read, Write, Edit, Glob, Grep | `codigo/` |
| `revisor` | Revisa en solo lectura y propone correcciones | Read, Glob, Grep, Write | `revision.md` |
| `documentador` | Documenta funcionalidad, objetos y pruebas | Read, Glob, Grep, Write | `documentacion.md` |

## Comandos

- `/desarrollo <spec o texto>`: flujo completo. Requerimientos, desarrollo, revisión (máximo 2 correcciones) y documentación.
- `/revisar <clase, fichero o patrón>`: solo revisión, con los errores críticos primero.

## Flujo de ficheros

```
specs/<tarea>.md
output/<tarea>/requerimientos.md
output/<tarea>/codigo/*.xpp
output/<tarea>/revision.md
output/<tarea>/documentacion.md
```

## Reglas para Claude

- Responde en español.
- No inventes nombres de tablas, campos, métodos ni labels. Verifícalos en el metadata o marca `TODO-VERIFICAR`.
- El revisor nunca modifica código; solo escribe `revision.md`.
- `output/` no se versiona (ver `.gitignore`).
