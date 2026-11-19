# 🦷 OdontoFlow

**SaaS de agendamento para clínicas odontológicas.** Agenda única de dentistas e consultórios, autoagendamento pelo paciente, menos faltas e visão gerencial.

> Projeto conduzido como um produto real, com regras de negócio, backlog, sprints, kanban, PRs revisados, ADRs e CI/CD. Ele acompanha uma trilha de estudos de Java de ~18 meses, começando em Java puro e terminando com Spring, nuvem e IA.

## Status
📋 [Kanban do projeto](https://github.com/users/vinicius3516/projects/5)

🚧 **Fase 0 — Setup e processo.** Veja o [roadmap](docs/04-roadmap.md).

## Documentação
| Documento | Conteúdo |
|---|---|
| [Visão do produto](docs/01-visao-do-produto.md) | Problema, personas e escopo |
| [Regras de negócio](docs/02-regras-de-negocio.md) | RN-001 … RN-120 |
| [Glossário](docs/03-glossario.md) | Termos do negócio ↔ nomes no código |
| [Roadmap](docs/04-roadmap.md) | Fases, entregas e critérios de saída |
| [Processo](docs/05-processo-de-desenvolvimento.md) | Sprints, kanban, Git, PRs e revisão |
| [Arquitetura](docs/06-arquitetura.md) | Princípios, pacotes e stack |
| [ADRs](docs/adr/) | Decisões de arquitetura |
| [Sprints](docs/sprints/) | Planejamento e retrospectivas |

## Stack (alvo)
Java 25 · Maven · JUnit 5 · PostgreSQL · Spring Boot · Spring Security (JWT → OAuth2/OIDC) · JPA/Hibernate · Flyway · Docker · GitHub Actions · Redis · Spring Batch · AWS (S3, SQS/SNS, ECS) · Spring AI

## Como executar
_A definir na Sprint 0 (issue de esqueleto do projeto)._

```java
./mvnw compile
java -cp target/classes com.odontoflow.cli.Main
```
