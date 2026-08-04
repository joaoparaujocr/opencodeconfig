# Core (protocolo operacional)

## Contrato de tarefa
Toda delegação deve ser tratada como uma tarefa com estes campos, mesmo quando o brief for curto:

```yaml
task_id: identificador curto
objective: resultado desejado
classification: SIMPLE|MEDIUM|COMPLEX
owner: agent responsável
inputs: paths, símbolos, erros e contexto necessário
constraints: limites técnicos e de escopo
deliverables: artefatos esperados
acceptance: critérios verificáveis de aceite
out_of_scope: o que não fazer
failure_policy: retry limitado|blocked|stop
```

Não cole arquivos ou conversas inteiras no brief. Passe paths, símbolos e evidências; o agent lê o repo.

## Estados da tarefa
Use mentalmente e reporte a transição mais recente:

```text
queued -> assigned -> running -> verifying -> done
                               -> blocked
                               -> failed
```

- `done`: todos os critérios de aceite têm evidência.
- `blocked`: falta input, permissão ou decisão externa; não invente uma solução.
- `failed`: a abordagem foi executada e não produziu o resultado esperado.
- `partial`: parte entregável pronta, mas algum critério permanece pendente.

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

## Validação e handoff
Antes de encadear outro agent, valide a saída:
- status informado e compatível com a evidência;
- entregáveis e paths dentro do escopo;
- comandos de verificação e resultados explícitos;
- bloqueios, riscos e lacunas declarados.

Saída vaga, incompleta ou malformada: faça no máximo 1 retry com feedback específico e fato novo. Não propague uma saída inválida para o próximo agent.

Formato mínimo de saída:

```text
Status: done|partial|blocked|failed
Resumo:
Entregáveis/files:
Verificação (comandos + resultado):
Lacunas/riscos:
Próxima ação:
Agents usados:
```

`Próxima ação` deve ser `stop` quando o DoD estiver completo. `blocked` e `failed` devem indicar uma única ação recuperável ou o motivo para escalar.

## Contexto
Brief: objetivo, paths, símbolos, erro, constraints, output, fora de escopo.
NÃO colar arquivos/logs/conversas inteiras. Subagent lê o repo.
Grep/glob antes de arquivo cheio. Sem `node_modules`/dist/locks.
Resposta curta; sem repetir o enunciado.

## Pós-subagent
Evidência, não dogma. Serve? → siga/feche. Falta 1 input? → pergunte ao user, não lance 3 agents.

## Checkpoints humanos
Não peça confirmação após cada Task. Continue automaticamente quando o DoD e as dependências estiverem claros. Pergunte ao usuário somente para ambiguidade de requisito, decisão irreversível, risco alto, mudança de escopo ou bloqueio sem fallback.

## Shell
Non-interactive (`-y`/`--force`). Target/teste único antes de suite full.
