# ADR-0002 — Java 25 LTS e Maven

- **Status:** Aceito
- **Data:** 2026-11-14

## Contexto
Precisamos de uma versão de Java com suporte longo e alinhada ao mercado, e de uma ferramenta de build padrão.

## Decisão
- **Java 25 (LTS)**, a versão LTS mais recente.
- **Maven**, com **Maven Wrapper** (`mvnw`), para que qualquer pessoa (e o CI) compile com a mesma versão do Maven sem instalá-lo.

## Alternativas consideradas
- **Java 21 (LTS anterior):** ainda é muito usado, mas não há motivo para começar um projeto novo numa LTS mais antiga.
- **Gradle:** é excelente, mas os cursos usam Maven e ele é o mais comum no mercado Java corporativo.

## Consequências
Podemos usar recursos modernos (records, pattern matching, `switch` expressions, virtual threads). Bibliotecas antigas podem não ser compatíveis, mas isso não é problema num projeto novo.
