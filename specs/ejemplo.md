# Especificación: aviso por límite de importe en diario general

## Objetivo

Avisar al usuario cuando una línea de un diario general supera un importe máximo configurable. El aviso no debe impedir guardar ni contabilizar el diario.

## Contexto

- Módulo: Finance, diario general (`LedgerJournalTable` y `LedgerJournalTrans`).
- Solo afecta a diarios de tipo General.

## Requisitos

- **R1.** Al validar una línea de diario general, si el valor absoluto de la diferencia entre el débito y el crédito en moneda de la transacción supera el límite, se muestra un aviso con el texto: "La línea {0} supera el límite de importe".
- **R2.** El límite se guarda en una tabla de parámetros de Finance, con una fila por compañía. Si no existe fila para la compañía, no se valida nada.
- **R3.** El aviso es informativo. No bloquea guardar ni contabilizar.
- **R4.** Solo se valida si el diario todavía no está contabilizado.

## Tablas y campos afectados

| Tabla | Campo | Uso | Requisito |
|---|---|---|---|
| `LedgerJournalTrans` | `AmountCurDebit`, `AmountCurCredit` | Lectura | R1 |
| `LedgerJournalTrans` | `JournalNum`, `LineNum` | Lectura (texto del aviso) | R1 |
| `<PREFIJO>LimiteImporteParams` (nueva) | `LimiteImporte` (decimal) | Lectura | R2 |

## Filtros y selecciones

- Solo líneas de diarios de tipo General (R1).
- Solo diarios no contabilizados (R4).

## Validaciones y avisos

- **Aviso (Infolog warning):** `abs(AmountCurDebit - AmountCurCredit) > LimiteImporte`. Mensaje según R1.

## Tipo de ejecución

**Por evento:** `validateWrite` de `LedgerJournalTrans`.

## Dudas detectadas

- ¿El límite se aplica sobre la moneda de la transacción o sobre la moneda de la compañía? [BLOQUEANTE]
- ¿Debe validarse también en diarios importados por DMF?
- ¿Quién mantiene la tabla de parámetros: el funcional o el administrador del sistema?
