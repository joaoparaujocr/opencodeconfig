---
description: Testes — rodar/escrever/isolar falhas (subtask)
agent: tester
subtask: true
---

Testes:
$ARGUMENTS

Se vazio: detectar runner → suíte principal/CI → isolar falhas.

Regras:
- Framework já do repo.
- develop só para seam mínimo de testabilidade (brief estrito).
- debugger só se falha com causa unclear.
- Não re-rodar full suite após pass do subset sem motivo.
- Determinístico; sem asserts silenciados.

Saída: estratégia, comandos, pass/fail, falhas, testes tocados, gaps, agents usados.
