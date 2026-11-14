# ADR-0003 — Código em inglês, documentação em português

- **Status:** Aceito
- **Data:** 2026-11-14

## Contexto
O produto é brasileiro e o negócio fala português. A linguagem Java, as bibliotecas e o mercado internacional falam inglês. Misturar idiomas no código (`getPacienteById`) gera inconsistência.

## Decisão
- **Código** (classes, métodos, variáveis, commits, nomes de branch): **inglês**.
- **Documentação, issues, PRs e regras de negócio:** **português**.
- O [glossário](../03-glossario.md) é a tradução oficial entre os dois mundos.

## Consequências
O código fica consistente e pronto para portfólio internacional. O custo é manter o glossário atualizado.
