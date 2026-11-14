# ADR-0005 — PostgreSQL como banco relacional

- **Status:** Aceito
- **Data:** 2026-11-14

## Contexto
O curso usa MySQL na seção de JDBC e PostgreSQL mais adiante. O domínio é fortemente relacional (agenda, conflitos, integridade).

## Decisão
Usar **PostgreSQL** em todas as fases (JDBC, JPA e produção), rodando localmente via Docker. Nos testes de integração, PostgreSQL real via Testcontainers. H2 **não** será usado como substituto em testes.

## Alternativas consideradas
- **MySQL:** é viável, mas usar dois bancos ao longo do projeto não traz ganho.
- **H2 em testes:** é rápido, mas tem dialeto diferente, e os testes passam com H2 e quebram em produção.

## Consequências
É um único banco para aprender a fundo, e dá acesso a recursos úteis como restrições de exclusão (`EXCLUDE USING gist`) para impedir sobreposição de horários no próprio banco. O custo é exigir Docker no ambiente.
