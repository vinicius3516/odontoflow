# 06 — Arquitetura

> Documento vivo. Ele descreve o **estado atual** e o **alvo**. Decisões importantes viram ADR em [`adr/`](adr/).

## Princípio central
**O domínio é o coração e não conhece nada de fora.** Banco de dados, HTTP, e-mail e frameworks são detalhes que se conectam ao domínio por **interfaces** (*ports and adapters*).

```
          ┌───────────── Adaptadores de entrada ──────────────┐
          │  CLI (Fase 1)   REST/Spring MVC (Fase 3+)   Batch  │
          └───────────────────────┬────────────────────────────┘
                                  ▼
                 ┌──────── Aplicação (casos de uso) ────────┐
                 │ BookAppointment, CancelAppointment, ...  │
                 └───────────────────┬──────────────────────┘
                                     ▼
                 ┌──────────────── Domínio ─────────────────┐
                 │ Appointment, Dentist, Room, Availability │
                 │ regras RN-xxx, exceções de domínio       │
                 │ interfaces: AppointmentRepository, ...   │
                 └───────────────────┬──────────────────────┘
                                     ▲ implementam
          ┌───────────── Adaptadores de saída ────────────────┐
          │ CSV (F1) → JDBC (F2) → JPA (F3)   E-mail   S3  ...│
          └────────────────────────────────────────────────────┘
```

## Evolução por fase

| Fase | Estrutura |
|---|---|
| 1 | Um módulo Maven: pacotes `domain`, `application`, `infrastructure` (CSV) e `cli` |
| 2 | Adaptador `infrastructure.jdbc` + PostgreSQL |
| 3 | Spring Boot; adaptadores `web` e `infrastructure.jpa`. O domínio continua sem anotações do Spring |
| 4+ | Segurança, observabilidade, mensageria e integrações como novos adaptadores |

> **Nota de pragmatismo (Fase 3):** manter as entidades JPA separadas do modelo de domínio custa mapeamento. A decisão entre separar ou usar o mesmo objeto será tomada **na Fase 3, via ADR**, com você já conhecendo os dois lados. Não antecipamos complexidade.

## Pacotes (alvo da Fase 1)
```
com.odontoflow
├── domain
│   ├── clinic        (Clinic, BusinessHours, TimeRange)
│   ├── dentist       (Dentist, CroRegistration, WorkSchedule, DentistProcedure)
│   ├── room          (Room)
│   ├── procedure     (Procedure)
│   ├── patient       (Patient, Guardian, Cpf)
│   ├── scheduling    (Appointment, AppointmentStatus, ScheduleBlock, Availability)
│   └── shared        (exceções de domínio, tipos comuns)
├── application       (casos de uso)
├── infrastructure    (implementações de repositório: memória, CSV, ...)
└── cli               (Main e menus)
```
Organização **por contexto de negócio** (`patient`, `scheduling`), não por tipo técnico (`models`, `services`).

## Stack

| Item | Escolha | ADR |
|---|---|---|
| Linguagem | Java 25 (LTS) | [0002](adr/0002-java-25-e-maven.md) |
| Build | Maven (com Maven Wrapper) | [0002](adr/0002-java-25-e-maven.md) |
| Testes | JUnit 5, AssertJ; depois Mockito, Testcontainers e RestAssured | — |
| Banco | PostgreSQL | [0005](adr/0005-postgresql.md) |
| Tempo | `java.time`, `Instant` em UTC, `ZoneId` por clínica | [0006](adr/0006-tratamento-de-data-e-hora.md) |
| Framework (Fase 3+) | Spring Boot (versão estável vigente) | a decidir |
| CI/CD (Fase 4) | GitHub Actions | a decidir |
