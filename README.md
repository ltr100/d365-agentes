# d365-agentes

Orquestación de subagentes de Claude Code para desarrollar y revisar código de Microsoft Dynamics 365 Finance & Operations (X++ y C#).

## Qué hace

El flujo completo es: especificación, requerimientos, desarrollo, revisión y documentación. Cada paso lo ejecuta un subagente especializado con solo las herramientas que necesita.

| Paso | Subagente | Resultado |
|---|---|---|
| 1 | `requerimientos` | `output/<tarea>/requerimientos.md` |
| 2 | `desarrollador` | `output/<tarea>/codigo/` |
| 3 | `revisor` | `output/<tarea>/revision.md` |
| 4 | `documentador` | `output/<tarea>/documentacion.md` |

## Requisitos

- [Claude Code](https://docs.claude.com/en/docs/claude-code) instalado.
- Acceso de lectura al metadata de D365 F&O (`PackagesLocalDirectory`) en la máquina donde se ejecuta.
- Git y GitHub CLI (`gh`), solo si vas a publicar cambios en el repositorio.
- No hace falta compilador. El código se revisa aquí y después se importa en Visual Studio.

## Uso

1. Clona el repositorio y entra en la carpeta.

2. Arranca Claude Code dando acceso al metadata:

   ```
   claude --add-dir "K:\AosService\PackagesLocalDirectory\<Modelo>"
   ```

3. Desarrollo completo desde una especificación:

   ```
   /desarrollo specs/ejemplo.md
   ```

   También puedes pasar el texto de la tarea directamente:

   ```
   /desarrollo "validación de límite de importe en diario general"
   ```

4. Solo revisión de objetos existentes:

   ```
   /revisar MiClase
   /revisar "clases WCO*"
   ```

Los resultados se guardan en `output/<tarea>/`. Al terminar, `/desarrollo` muestra un resumen con los requisitos cubiertos, los ficheros creados y los problemas pendientes.

## Estructura

```
d365-agentes/
├── .claude/
│   ├── agents/        subagentes (requerimientos, desarrollador, revisor, documentador)
│   └── commands/      comandos (/desarrollo, /revisar)
├── specs/             especificaciones de entrada
├── output/            resultados de cada ejecución (no versionado)
├── CLAUDE.md          contexto del proyecto para Claude
└── README.md
```

## Añadir un agente nuevo

1. Crea `.claude/agents/<nombre>.md`.
2. Añade un front matter YAML con `name`, `description` y `tools`. La `description` debe decir cuándo usarlo, porque Claude la utiliza para decidir.
3. Escribe el system prompt debajo del front matter.
4. Limita `tools` a lo mínimo necesario. Un agente que solo analiza no debería tener `Edit`.
5. Si el agente forma parte del flujo, añade su paso en `.claude/commands/desarrollo.md`.

## Añadir un comando nuevo

Crea `.claude/commands/<nombre>.md`. Usa `$ARGUMENTS` para recibir lo que el usuario escriba después del comando.

## Notas

- Revisa siempre el código y crea los labels antes de importarlo en Visual Studio.
- Los `TODO-VERIFICAR` del código indican nombres que el agente no pudo confirmar en el metadata.
