---
description: Review de diff/PR (subtask)
agent: review
subtask: true
---

Revise:
$ARGUMENTS

Se vazio, use diff de trabalho atual (git diff / staged / branch vs base).

Regras:
- Não implementar.
- Severidade: blocker | major | minor | nit.
- explore/researcher só para impacto fora do diff.
- security só se superfície sensível no diff (não por protocolo).
- Priorize blockers; poucos nits.

Saída no formato do agent review + veredito approve|request_changes|comment + agents usados.
