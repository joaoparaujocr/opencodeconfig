---
description: Feature com workflow mínimo (architect? → develop → tester → review?)
agent: orchestrator
---

Feature:
$ARGUMENTS

Classifique tamanho:

**Pequena:** develop|backend → tester (se comportamento mudou). Sem architect/review obrigatório.

**Média:** architect só se design incerto → develop|backend → tester.

**Complexa:**
researcher? → architect → develop|backend → tester → review → security?

Regras:
- Não chamar architect para feature trivial.
- Não chamar security sem superfície sensível.
- Não chamar review antes de diff útil.
- Brief claro; sem duplicar contexto.
- Paralelo proibido na cadeia develop→tester.
- Caps: pequena ≤2 agents; média ≤3; complexa ≤6. Sem re-call do mesmo agent.
- DoD cumprido → parar (sem polish extra).

Entregar: classe, plano curto, paths, verificação, testes, gates se houver.
