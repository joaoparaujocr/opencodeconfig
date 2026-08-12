---
description: Orquestra o time com workflow mínimo para a tarefa ($ARGUMENTS)
agent: orchestrator
---

Tarefa:
$ARGUMENTS

Aplique o Core do time (mínimo de agents, anti-loop, economia de contexto).

Hard caps:
- SIMPLE: 1 agent (sempre delegue). MEDIUM: ≤3 nested. COMPLEX: ≤6 nested.
- Mesmo agent no máx 1× salvo re-delegação (≤2) com fato novo.
- Mesma tool/args 2× ou sem progresso 2 iterações → STOP_LOOP e feche com estado parcial.
- Não re-delegar para “confirmar”. Não pipeline por protocolo.

Fluxo:
1. Classifique: SIMPLE | MEDIUM | COMPLEX.
2. SIMPLE → 1 agent especialista; feche. Nunca self-edit.
3. MEDIUM/COMPLEX → decomponha slices (`owner`, `paths`, `acceptance`); marque `parallel` | `blocked_by`.
4. Fan-out: wave de N `Task` **na mesma mensagem** se independentes (caps Core: MEDIUM ≤3, COMPLEX ≤4/wave).
5. Serial só com `depends_on` real (ex.: develop → tester). Não serializar o que já é paralelo.
6. Security/review só com superfície/diff real (podem ir no mesmo wave pós-diff).
7. Após cada wave: valide evidência; pode fechar? Se sim, feche. Senão, próximo wave.

Fechamento:
Classe / Agents usados / Mudanças / Verificação / Riscos / Próximos passos

Sem commit/push sem pedido.
