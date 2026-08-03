---
description: Gate final mínimo — tester? + review + security?
agent: orchestrator
---

Ship gate:
$ARGUMENTS

Escopo = diff atual ou $ARGUMENTS.

Fluxo mínimo:
1. Resumir o que entra no ship (paths).
2. tester — só se testes relevantes ainda não rodaram nesta sessão.
3. review — obrigatório se houver diff de código.
4. security — só se diff tocar auth, input, secrets, rede, upload, crypto.
5. Veredito SHIP | NO-SHIP.

Regras:
- Não re-rodar suite full sem motivo.
- Não chamar security em diff de docs/typo.
- Não re-chamar review/security para “segunda opinião” sem achado novo.
- Caps: ≤3 nested (tester? + review + security?).
- Não commit/push/tag sem pedido explícito.

```
Diff summary:
Tests:
Review:
Security:
Veredito: SHIP | NO-SHIP
Bloqueadores:
Agents usados:
```
