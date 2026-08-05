---
description: Mostra mapa visual do time, matriz de delegação e como inspecionar child sessions
agent: build
---

Ignore o pedido do usuário além deste comando. **Não** edite arquivos. **Não** chame subagents. Responda só com o mapa abaixo (pode colar literal).

# Mapa do time (delegação)

```
                    USER
                      |
              Tab: build (default)
              Tab: orchestrator
                      |
              /team /feature /bugfix /ship
                      v
               ORCHESTRATOR
        +--------+----+----+--------+
        |        |    |    |        |
        v        v    v    v        v
   architect  develop debugger researcher
        |        |       |         |
        |        +---+---+         |
        |            |             |
        |         tester           |
        |            |             |
        v            v             |
   review <------ diff -------> security
        (backend = develop p/ Nest/API)
```

## Matriz Task (quem pode chamar quem)

| De | Pode Task → |
|----|-------------|
| orchestrator | architect, develop, backend, debugger, researcher, tester, review, security, explore, scout |
| architect | researcher, explore, scout, security |
| develop / backend | researcher, explore, debugger, tester |
| debugger | researcher, explore, tester |
| researcher | explore, scout |
| tester | researcher, explore, debugger, develop |
| review | researcher, explore, security |
| security | researcher, explore, scout |
| **ninguém** | general |

## Caps

- SIMPLE 1 · MEDIUM ≤3 · COMPLEX ≤6 nested
- workers ≤2 nested · leaves ≤1
- steps: orch 32 · dev/back 28 · dbg/test 24 · arch 20 · research 16 · review/sec 16

## Como VER delegações na UI (visual)

### A) Child sessions (built-in TUI)
1. Rode `/team` ou peça algo que dispare Task.
2. No **parent** (orchestrator/build): `session_child_first` (default **Ctrl+Down** ou keybind configurado).
3. Entre filhos: **Right** / **Left** (`session_child_cycle` / reverse).
4. Voltar ao parent: **Up** (`session_parent`).
5. Cada child = 1 delegação (nome do agent na sessão).

### B) tmux panes (plugin já no config: `opencode-agent-tmux`)
1. Rode OpenCode **dentro de tmux** (`tmux` → `opencode`).
2. Quando um agent/subagent sobe, o plugin abre **pane** com a execução em tempo real.
3. Um pane por agent ≈ árvore visual lado a lado.

### C) Lista de sessões
- `/sessions` (ou `ctrl+x l`) — lista sessões; filhos aparecem ligados ao parent.

## Checklist visual rápido

```
[ ] Trivial → 0 child
[ ] Feature pequena → develop (+ tester?) só
[ ] Bug unclear → debugger antes de develop
[ ] Ship docs → sem security child
[ ] Mesmo agent 2x mesmo brief → FAIL
```

## Commands úteis
/team /feature /bugfix /ship /implement /debug /research /architect /review-pr /secure /test-fix /team-map
