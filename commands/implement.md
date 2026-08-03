---
description: Implementação direta com develop (sem orquestração)
agent: develop
subtask: true
---

Implemente:
$ARGUMENTS

Regras develop:
- SIMPLE se possível: zero nested agents.
- Diff mínimo; convenções do repo.
- Chame explore/researcher só se faltar mapa.
- tester só se comportamento mudou e valer a pena.
- NÃO chamar architect/review/security.
- Se precisar redesign: BLOCKED para architect.

Saída: assunções, files, comportamento, verificação, agents usados.
Sem commit sem pedido.
