# Core (anti-custo)

## Regra de ouro
Mínimo de agents/tool calls. `1 agent → 1 resultado` > pipeline de 7.
NÃO: delegar SIMPLE; multi-agent quando 1 resolve; repetir trabalho; re-pedir info já no contexto; expandir escopo; commit sem pedido.

## Anti-loop → pare e responda STOP_LOOP
1. Mesma tool+args 2× sem dado novo
2. Mesmo arquivo 3× sem hipótese nova
3. Mesmo erro 2× após o mesmo fix
4. Mesmo agent 2× com brief equivalente
5. Nested Task acima do teto
6. 2 iterações sem progresso (fato/path/hipótese)

```
STOP_LOOP
Tentei: | Sei: | Bloqueio: | Próximo passo (1):
```
NÃO tentar de novo com outras palavras. NÃO re-delegar para “confirmar”.

## Tetos nested Task / tarefa
| Papel | Máx | Re-call mesmo agent |
|-------|-----|---------------------|
| orchestrator | SIMPLE 0–1 / MEDIUM ≤3 / COMPLEX ≤6 | só com fato novo |
| develop/backend/debugger/tester | ≤2 | 0 |
| architect/researcher/review/security | ≤1 | 0 |

## Pare no DoD
DoD cumprido → stop. Sem polish extra, suite full opcional, review/security por protocolo.

## Contexto
Brief: objetivo, paths, símbolos, erro, constraints, output, fora de escopo.
NÃO colar arquivos/logs/conversas inteiras. Subagent lê o repo.
Grep/glob antes de arquivo cheio. Sem `node_modules`/dist/locks.
Resposta curta; sem repetir o enunciado.

## Pós-subagent
Evidência, não dogma. Serve? → siga/feche. Falta 1 input? → pergunte ao user, não lance 3 agents.

## Shell
Non-interactive (`-y`/`--force`). Target/teste único antes de suite full.
