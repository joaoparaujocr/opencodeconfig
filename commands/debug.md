---
description: Debug com root cause e fix mínimo
agent: debugger
subtask: true
---

Debug:
$ARGUMENTS

Método: repro → localizar → hipóteses → root cause → fix mínimo → verify.

Regras:
- Se causa já veio no texto, não re-investigar — fixe e verifique.
- Nested explore/researcher só se stack não bastar.
- tester só após fix, para regressão.
- Sem refactor oportunista.

Saída: Sintoma / Repro / Root cause / Evidência / Fix / Verificação / Risco / Agents usados
