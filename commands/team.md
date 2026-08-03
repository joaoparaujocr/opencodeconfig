---
description: Orquestra o time com workflow mínimo para a tarefa ($ARGUMENTS)
agent: orchestrator
---

Tarefa:
$ARGUMENTS

Aplique o Core do time (mínimo de agents, anti-loop, economia de contexto).

Hard caps:
- SIMPLE: 0–1 agent. MEDIUM: ≤3 nested. COMPLEX: ≤6 nested.
- Mesmo agent no máx 1× salvo brief com fato novo.
- Mesma tool/args 2× ou sem progresso 2 iterações → STOP_LOOP e feche com estado parcial.
- Não re-delegar para “confirmar”. Não pipeline por protocolo.

Fluxo:
1. Classifique: SIMPLE | MEDIUM | COMPLEX.
2. SIMPLE → execute ou 1 agent; feche.
3. MEDIUM/COMPLEX → workflow mínimo (feature/bug/research).
4. Paralelo só se independente.
5. Security/review só com superfície/diff real.
6. Após cada resultado: pode fechar? Se sim, feche.

Fechamento:
Classe / Agents usados / Mudanças / Verificação / Riscos / Próximos passos

Sem commit/push sem pedido.
