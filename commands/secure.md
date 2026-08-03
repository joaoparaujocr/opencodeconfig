---
description: Security review de risco real (subtask)
agent: security
subtask: true
---

Security review:
$ARGUMENTS

Se vazio, diff atual + áreas sensíveis tocadas.

Regras:
- Risco real explorável > checklist genérico.
- Sem exploits/PoCs ofensivos.
- Sem ler/imprimir secrets.
- Nested research só para achar entrypoints/deps.
- Mitigações acionáveis para develop.
- Se 0 issues reais, feche clean — sem segundo pass genérico.

Saída no formato do agent security.
