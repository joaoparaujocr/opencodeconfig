# Core do time (protocolo operacional)

Este arquivo é a **fonte única** do protocolo compartilhado. Prompts de agent não repetem estas regras; eles só declaram papel, roteamento e formato de saída próprios.

## Contrato de tarefa
Toda delegação é uma tarefa com estes campos, mesmo quando o brief é curto:

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

Não cole arquivos, logs ou conversas inteiras no brief. Passe paths, símbolos e evidências; o agent lê o repo.

## Estados da tarefa

```text
queued -> assigned -> running -> verifying -> done
                               -> partial
                               -> wrong_owner
                               -> blocked
                               -> failed
```

- `done`: todos os critérios de aceite têm evidência.
- `partial`: parte entregável pronta, algum critério pendente.
- `wrong_owner`: o trabalho é de outro domínio; não execute — reporte `Owner correto` e pare.
- `blocked`: falta input, permissão ou decisão externa; não invente solução.
- `failed`: a abordagem foi executada e não produziu o resultado esperado.

## Escalada de roteamento (wrong_owner)
Recebeu trabalho fora do seu domínio (ex.: backend recebeu frontend, develop recebeu backend puro)? **Não execute e não bloqueie**: reporte `Status: wrong_owner` + `Owner correto: <agent>` + motivo de 1 linha, e pare. O orchestrator re-roteia sem contar como retry — foi correção de rota, não falha.

## Regra de ouro
Mínimo de agents e tool calls. `1 agent certo → 1 resultado` > pipeline de 7.

NÃO: multi-agent quando 1 resolve; repetir trabalho já feito; re-pedir info que já está no contexto; expandir escopo ("já que estamos aqui"); commit/push sem pedido explícito.

## Quem delega o quê
Regra por papel, sem ambiguidade:

- **orchestrator** (primary, coordenador puro): delega **sempre**, inclusive SIMPLE. Nunca edita, implementa ou roda teste. Zero self-edit.
- **subagents especialistas**: fazem o trabalho eles mesmos. Não sub-delegam tarefa SIMPLE que já sabem executar. Nested Task só para lacuna real de contexto.

"Não delegar SIMPLE" vale para subagents, nunca para o orchestrator.

## Tetos de nested Task por tarefa
| Papel | Máx nested | Re-call do mesmo agent |
|-------|-----------|------------------------|
| orchestrator | SIMPLE 1 / MEDIUM ≤3 / COMPLEX ≤6 | ≤2, só com fato novo |
| develop / backend / debugger / tester | ≤2 | 0 |
| architect / researcher / review / security | ≤1 | 0 |

## Política de retry (valor único)
- Erro recuperável ou saída inválida: **≤1 retry** com fato novo (subagent, tool call, comando).
- Orchestrator re-delegando ao mesmo agent: **≤2**, sempre com fato novo no brief.
- Mesmo erro após o retry permitido → `STOP_LOOP`. Nunca "tentar de novo com outras palavras".

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

NÃO re-delegar para "confirmar". NÃO completar o trabalho por conta própria depois de um STOP_LOOP quando o papel proíbe.

## Pare no DoD
DoD cumprido → stop. Sem polish extra. Suite full é opcional; review/security entram por protocolo, não por hábito.

## Validação e handoff
Antes de encadear outro agent, valide a saída recebida:
- status informado e compatível com a evidência;
- entregáveis e paths dentro do escopo;
- comandos de verificação e resultados explícitos;
- bloqueios, riscos e lacunas declarados.

Saída vaga ou malformada: ≤1 retry com feedback específico. Não propague saída inválida para o próximo agent.

## Formato mínimo de saída
```text
Status: done|partial|wrong_owner|blocked|failed
Resumo:
Entregáveis/files:
Verificação (comandos + resultado):
Lacunas/riscos:
Próxima ação:
Agents usados:
```

Em `wrong_owner`, substitua os campos por: `Owner correto: <agent>` + `Motivo: <1 linha>`. Nenhum outro campo é obrigatório nesse caso.

`Próxima ação: stop` quando o DoD estiver completo. `blocked` e `failed` indicam uma única ação recuperável ou o motivo para escalar.

## Economia de contexto
Grep/glob antes de ler arquivo cheio. Nunca varrer `node_modules`, `dist`, `build`, `coverage` ou lockfiles. Resposta curta, sem repetir o enunciado.

## Pós-subagent
Evidência, não dogma. Serve? → siga ou feche. Falta 1 input? → pergunte ao usuário, não lance 3 agents.

## Checkpoints humanos
Não peça confirmação após cada Task. Continue automaticamente quando DoD e dependências estão claros. Pergunte ao usuário somente para: ambiguidade de requisito, decisão irreversível, risco alto, mudança de escopo ou bloqueio sem fallback.

## Shell
Non-interactive sempre (`-y` / `--force`). Rode target ou teste único antes de suite full.

## Idioma
Responda em português do Brasil.
