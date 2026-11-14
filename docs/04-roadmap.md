# 04 — Roadmap

O roadmap acompanha a trilha de estudos: **nenhuma task exige um conteúdo que ainda não foi estudado**. Cada issue indica a seção do curso de que depende, com a label `secao:XX`.

Os prazos são **estimativas**. O ritmo é ditado pelo aprendizado, não pelo calendário. Ao fim de cada fase existe uma tag de versão e um **critério de saída** objetivo.

## Visão geral

| Fase | Nome | Curso | Período estimado | Versão |
|---|---|---|---|---|
| 0 | Setup e processo | — | Sprint 0 | — |
| 1 | Núcleo de domínio em Java puro | Fundamentos, seções 4–18 | ~6 meses | `v0.1.0` |
| 2 | Persistência com JDBC | Fundamentos, seção 20 | ~1 mês | `v0.2.0` |
| 3 | API REST com Spring Boot | Fundamentos 21–22 + Java Profissional (DI, ORM, REST, JPQL) | ~2–3 meses | `v0.3.0` |
| 4 | MVP em produção | Java Profissional (segurança, testes, Docker, CI/CD) | ~2 meses | `v1.0.0` |
| 5 | Produto | Java Expert | ~4–6 meses | `v1.x` |

---

## Fase 0 — Setup e processo
**Objetivo:** ambiente pronto, repositório organizado e o primeiro PR passando pelo fluxo completo.
**Critério de saída:** projeto Maven compila com `mvn verify`, o primeiro PR foi revisado e mergeado, e você leu e questionou a documentação.

## Fase 1 — Núcleo de domínio em Java puro (`odontoflow-core`)
**Objetivo:** implementar as regras de negócio centrais **sem nenhum framework**. A aplicação roda por linha de comando (CLI) e persiste em arquivo.

**Por que assim?** Em sistemas bem projetados, o domínio não depende de framework ([ADR-0004](adr/0004-dominio-isolado-de-framework.md)). Tudo o que você construir aqui **sobrevive** às fases seguintes: o Spring só vai "envolver" esse núcleo.

Evolução planejada, sprint a sprint:

| Sprint | Seção do curso | O que o domínio ganha |
|---|---|---|
| 1 | 4 — Estrutura sequencial | Cálculos de horário com minutos inteiros (término de consulta, capacidade do expediente) |
| 2 | 5 — Condicionais | Validação de horário dentro do expediente, cancelamento no prazo ou tardio |
| 3 | 6–7 — Repetição, Strings, funções | Grade de horários do dia, validação de CPF, menu da CLI |
| 4 | 8–9 — OO, construtores, encapsulamento | Primeiras classes (`Procedure`, `Dentist`, `Patient`) e **JUnit 5** |
| 5 | 10 — Memória, arrays, listas | Agenda do dia como lista e detecção de conflitos (RN-051/052/053) |
| 6 | 11 — Data e hora | Refatoração para `java.time` e fuso da clínica (RN-001, RN-055, RN-056) |
| 7 | 12 — Enums e composição | `AppointmentStatus` com transições (RN-060), habilitações (RN-012) |
| 8 | 13 — Herança e polimorfismo | Bloqueios de agenda polimórficos (RN-070), notificações |
| 9 | 14 — Exceções | Hierarquia de exceções de domínio ligadas às RNs |
| 10 | 15 — Arquivos | Persistência em CSV, importação de pacientes |
| 11 | 16 — Interfaces | Repositórios como interfaces, injeção de dependência manual, `Clock` injetável |
| 12 | 17 — Generics, Set, Map | Repositório genérico, `equals`/`hashCode`, índices |
| 13 | 18 — Lambdas e streams | **Cálculo de disponibilidade (RN-080)**, relatórios |

**Critério de saída (`v0.1.0`):** um agendamento completo pela CLI (cadastrar, consultar disponibilidade, agendar, mudar status, cancelar) respeitando as RNs 001–071 aplicáveis, com testes unitários cobrindo cada uma.

## Fase 2 — Persistência com JDBC
**Objetivo:** trocar a persistência em CSV por **PostgreSQL** via JDBC puro, padrão DAO e transações. Isso prova que as interfaces da Sprint 11 funcionam: o domínio não muda.
**Entregas:** PostgreSQL via Docker, scripts SQL de schema, implementações JDBC dos repositórios, remarcação transacional (RN-063).
**Critério de saída (`v0.2.0`):** a CLI funciona igual, agora com PostgreSQL. Testes de integração dos DAOs.

## Fase 3 — API REST com Spring Boot
**Objetivo:** expor o domínio como **API REST**.
**Entregas:** Spring Boot, JPA/Hibernate, Flyway, camadas (controller → service → domínio → repository), DTOs, Bean Validation, tratamento global de erros (padrão RFC 9457 *Problem Details*), paginação, consultas JPQL otimizadas (sem N+1), auditoria (RN-120).
**Critério de saída (`v0.3.0`):** todos os casos de uso da Fase 1 disponíveis por HTTP, com uma collection do Postman versionada no repositório.

## Fase 4 — MVP em produção
**Objetivo:** produto utilizável por uma clínica real.
**Entregas:**
- Spring Security com JWT, perfis (RN-110), multi-tenant (RN-111), cadastro (signup) e recuperação de senha por e-mail.
- Testes de unidade (Mockito), de camada web (MockMvc) e de API (RestAssured), com cobertura medida pelo Jacoco (meta: ≥ 80% no domínio).
- Docker e Docker Compose; perfis `test`/`dev`/`prod`; pipeline de CI/CD no GitHub Actions; deploy (Railway).

**Critério de saída (`v1.0.0`):** API em produção, pipeline verde, README com instruções de execução e demonstração gravada.

## Fase 5 — Produto (Java Expert)
Cada item entra como épico **somente quando houver um caso de uso real**:

| Tema do curso | Caso de uso no OdontoFlow |
|---|---|
| Envio de e-mail (SendGrid, Strategy) | Notificações transacionais (RN-090) com provedor trocável |
| Upload no S3 | Logo da clínica e foto do dentista |
| Swagger / OpenAPI | Documentação pública da API |
| Versionamento de API | `/api/v1`, `/api/v2` quando houver *breaking change* |
| Spring Batch | Lembretes diários (RN-090), importação de pacientes em massa, relatório mensal |
| Cache com Redis | Consulta pública de disponibilidade (leitura intensa) e teste de carga com k6 |
| Mensageria (SQS/SNS) e SSE | Evento `AppointmentBooked` → notificação, lista de espera (RN-100/101), painel da recepção em tempo real |
| OAuth2 / OIDC | Authorization Server próprio: login do paciente e da equipe |
| Deploy na AWS | ECS Fargate, ALB, CloudWatch, autoscaling |
| Spring AI | Assistente de agendamento por chat (tool calling) e FAQ da clínica (RAG) |
| MongoDB / Cassandra / WebFlux | **Somente se surgir caso legítimo** (candidato: trilha de auditoria). Caso contrário, ficam como estudo fora do projeto. |
