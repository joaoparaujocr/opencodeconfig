---
description: Bugfix mínimo (debugger? → fix → tester)
agent: orchestrator
---

Bug:
$ARGUMENTS

Classifique:

**Causa óbvia:** develop|backend → tester (regressão se fizer sentido).

**Causa unclear:** debugger → (fix no debugger ou develop) → tester.

**Complexa:** debugger → develop → tester → review → security?

Regras:
- Não chamar debugger se a causa já está no report.
- Sem stack trace/repro no pedido → envie a `debugger` com o sintoma; ele decide repro.
- `wrong_owner` não queima retry: re-roteie para o owner indicado.
- Não refactor fora do fix.
- 1 brief objetivo por agent.
- Não repetir a mesma investigação.
- Mesmo erro 2× após o mesmo fix → STOP_LOOP (falta repro/env/dado).
- Caps: óbvio ≤2 agents; unclear ≤3; complexa ≤5.

Formato final:
Sintoma / Root cause / Fix (files) / Regressão / Verificação / Agents usados
