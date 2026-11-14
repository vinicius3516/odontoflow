# ADR-0006 — Tratamento de data e hora

- **Status:** Aceito
- **Data:** 2026-11-14

## Contexto
Agenda é, essencialmente, data e hora. Erros de fuso horário e horário de verão são os bugs mais comuns e mais caros nesse tipo de sistema.

## Decisão
1. **Instantes** (quando algo acontece, como o início de um agendamento ou a data de criação) são `Instant`, persistidos em **UTC**.
2. **Regras de calendário** (expediente, escala, "hoje", "amanhã") usam `LocalDate`/`LocalTime`, interpretados no **`ZoneId` da clínica** (RN-001).
3. **Durações** são `Duration`.
4. O "agora" **nunca** vem de `Instant.now()` direto no domínio. Ele vem de um `java.time.Clock` injetado, para que os testes sejam determinísticos.
5. Na API, as datas usam o formato ISO 8601.

> Até a Sprint 6 (seção 11 do curso) trabalharemos com minutos como `int`. Isso é **dívida técnica intencional**, quitada na Sprint 6.

## Consequências
O sistema fica correto em qualquer fuso e testável. Em troca, exige disciplina na conversão entre `Instant` e o horário local da clínica.
