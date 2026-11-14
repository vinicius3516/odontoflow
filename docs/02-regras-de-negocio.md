# 02 — Regras de Negócio

Toda regra tem um identificador **RN-XXX**. Issues, PRs, testes e exceções devem citar a RN que implementam.
Exemplo de teste: `shouldRejectOverlappingAppointmentForSameDentist_RN051()`.

> Regras mudam. Uma mudança de RN é feita **por PR neste arquivo**, com justificativa. Nunca "direto no código".

Valores entre colchetes, como **[padrão: 15]**, são **configuráveis por clínica**.

---

## Clínica (RN-001 a RN-009)

- **RN-001 — Fuso horário.** Cada clínica tem um fuso horário (ex.: `America/Sao_Paulo`). Regras de expediente, exibição de horários e cálculo de "hoje" usam o fuso da clínica. Instantes são persistidos em UTC.
- **RN-002 — Expediente semanal.** Para cada dia da semana, a clínica tem zero ou mais intervalos de funcionamento `[abertura, fechamento)`. Exemplo: seg 08:00–12:00 e 13:00–18:00. Um dia sem intervalos é um dia fechado. Os intervalos de um mesmo dia não podem se sobrepor, e a abertura deve ser anterior ao fechamento.
- **RN-003 — Granularidade.** A agenda trabalha em blocos de 10, 15, 20 ou 30 minutos **[padrão: 15]**. Todo agendamento começa num horário alinhado à granularidade, contada a partir da meia-noite (com granularidade 15: 08:00, 08:15, 08:30…).
- **RN-004 — Higienização.** Entre dois atendimentos no **mesmo consultório** deve existir um intervalo de higienização de 0 a 30 minutos **[padrão: 10]**. O intervalo não se aplica ao dentista, só à cadeira.

## Dentista (RN-010 a RN-019)

- **RN-010 — CRO.** Todo dentista tem registro no CRO (número + UF). A combinação número + UF é única na clínica.
- **RN-011 — Escala.** O dentista tem uma escala semanal (intervalos por dia da semana), que deve estar **contida** no expediente da clínica (RN-002).
- **RN-012 — Habilitação.** O dentista só realiza os procedimentos para os quais está habilitado. Para cada habilitação, ele pode ter **duração própria** (respeitando RN-030) e **preço próprio**. Sem valor próprio, valem os do procedimento.
- **RN-013 — Inativação.** Dentista inativo não recebe novos agendamentos. A inativação é recusada enquanto houver agendamentos futuros ativos dele. Nesse caso, o sistema lista os agendamentos para que sejam remarcados ou cancelados.

## Consultório (RN-020 a RN-029)

- **RN-020 — Recurso obrigatório.** Todo agendamento ocupa exatamente um consultório (cadeira).
- **RN-021 — Consultório inativo.** Consultório inativo não recebe novos agendamentos. A inativação segue a mesma lógica da RN-013.

## Procedimento (RN-030 a RN-039)

- **RN-030 — Dados do procedimento.** Nome único na clínica, duração padrão entre 15 e 240 minutos e **múltipla da granularidade** (RN-003), preço de referência ≥ 0 e a flag `agendavelOnline`.
- **RN-031 — Exige avaliação.** Procedimentos marcados com `exigeAvaliacao` (ex.: canal, implante) só podem ser agendados **pelo paciente** se ele tiver uma consulta de avaliação **CONCLUÍDA** nos últimos **[padrão: 180]** dias. A recepção pode agendar sem essa restrição.

## Paciente (RN-040 a RN-049)

- **RN-040 — Identificação.** O paciente é identificado por CPF **válido** (com dígitos verificadores), único na clínica. Nome completo, data de nascimento e telefone são obrigatórios. O e-mail é opcional, mas obrigatório para quem usa o autoagendamento.
- **RN-041 — Menor de idade.** Paciente com menos de 18 anos (calculado na data de hoje, no fuso da clínica) deve ter um responsável legal com nome, CPF válido e telefone.
- **RN-042 — LGPD.**
  - O consentimento para tratamento de dados é registrado com data e hora.
  - O paciente pode pedir a **exportação** dos seus dados e a **anonimização** do cadastro. Na anonimização, o histórico de agendamentos é mantido, mas sem dados pessoais.
  - CPF, telefone e e-mail **nunca** aparecem em logs. Quando inevitável, aparecem mascarados (`***.456.789-**`).

## Agendamento (RN-050 a RN-059)

- **RN-050 — Composição.** Um agendamento reúne paciente, dentista, procedimento, consultório e início. **Fim = início + duração efetiva**, sendo a duração efetiva a da habilitação do dentista ou, se não houver, a do procedimento (RN-012).
- **RN-051 — Conflito de dentista.** Um dentista não pode ter dois agendamentos ativos com intervalos sobrepostos.
- **RN-052 — Conflito de consultório.** Um consultório não pode ter dois agendamentos ativos sobrepostos, considerando a higienização (RN-004): o próximo atendimento só pode começar em `fim anterior + higienização` ou depois.
- **RN-053 — Conflito de paciente.** Um paciente não pode ter dois agendamentos ativos sobrepostos.
- **RN-054 — Dentro da janela válida.** O agendamento inteiro (início e fim) deve estar dentro do expediente da clínica (RN-002) **e** da escala do dentista (RN-011), e não pode tocar nenhum bloqueio (RN-070).
- **RN-055 — Sem passado.** Não é permitido criar um agendamento com início no passado.
- **RN-056 — Antecedência no autoagendamento.** Quando o paciente agenda sozinho, o início deve respeitar uma antecedência mínima **[padrão: 2h]** e uma máxima **[padrão: 60 dias]**. A recepção não tem antecedência mínima.
- **RN-057 — Limite no autoagendamento.** Pelo autoagendamento, o paciente pode ter no máximo **[padrão: 2]** agendamentos futuros ativos.
- **RN-058 — Procedimento online.** Pelo autoagendamento, só é possível escolher procedimentos com `agendavelOnline = true`.

> "Agendamento **ativo**" é aquele com status `AGENDADO`, `CONFIRMADO` ou `EM_ATENDIMENTO`.

## Ciclo de vida do agendamento (RN-060 a RN-069)

- **RN-060 — Estados e transições.** Somente as transições abaixo são permitidas:

| De \ Para | CONFIRMADO | EM_ATENDIMENTO | CONCLUIDO | CANCELADO | FALTOU |
|---|:---:|:---:|:---:|:---:|:---:|
| **AGENDADO** | ✅ | ✅ | — | ✅ | ✅ |
| **CONFIRMADO** | — | ✅ | — | ✅ | ✅ |
| **EM_ATENDIMENTO** | — | — | ✅ | — | — |

```
AGENDADO ──► CONFIRMADO ──► EM_ATENDIMENTO ──► CONCLUIDO
   │  └──────────────────────────▲
   ├──► CANCELADO ◄── CONFIRMADO
   └──► FALTOU    ◄── CONFIRMADO
```

- **RN-061 — Cancelamento.** Todo cancelamento exige motivo (`PACIENTE_DESISTIU`, `REMARCADO`, `CLINICA_INDISPONIVEL`, `OUTRO`), e registra quem cancelou (paciente ou clínica) e quando. Se o paciente cancelar com menos de **[padrão: 24h]** de antecedência, o cancelamento é **tardio**.
- **RN-062 — Falta.** `FALTOU` só pode ser marcado depois de `início + tolerância` **[padrão: 15 min]**.
- **RN-063 — Remarcação.** Remarcar significa cancelar o original (motivo `REMARCADO`) e criar um novo agendamento **vinculado** ao original, numa única operação: ou as duas coisas acontecem, ou nenhuma. Pelo autoagendamento, uma cadeia permite no máximo **[padrão: 2]** remarcações.
- **RN-064 — Política de faltas.** **3** ocorrências (`FALTOU` ou cancelamento tardio) em **180** dias bloqueiam o autoagendamento do paciente. A partir daí, só a recepção agenda para ele. O desbloqueio é manual, feito por `ADMIN`.
- **RN-065 — Imutabilidade.** `CONCLUIDO`, `CANCELADO` e `FALTOU` são estados finais: nenhum dado do agendamento muda depois deles.
- **RN-066 — Check-in.** `EM_ATENDIMENTO` só pode ser marcado no dia do agendamento, a partir de **[padrão: 30 min]** antes do início.

## Bloqueios de agenda (RN-070 a RN-079)

- **RN-070 — Tipos de bloqueio.** Um bloqueio é um período `[início, fim)` com motivo, de um destes tipos:
  - **dentista** (férias, curso, ausência);
  - **consultório** (manutenção);
  - **clínica** (feriado, recesso), que bloqueia todos os dentistas e consultórios.
- **RN-071 — Bloqueio com conflito.** Criar um bloqueio que se sobrepõe a agendamentos ativos é recusado. O sistema devolve a lista de agendamentos em conflito para serem tratados antes.

## Disponibilidade (RN-080 a RN-089)

- **RN-080 — Horários disponíveis.** Dado um **procedimento**, uma **data** e, opcionalmente, um **dentista**, o sistema retorna os inícios possíveis (alinhados à granularidade) em que existe dentista habilitado **e** consultório livre que satisfaçam as RN-051 a RN-056. Sem dentista informado, cada horário vem com os dentistas possíveis.
- **RN-081 — Escolha de consultório.** Ao criar o agendamento sem consultório informado, o sistema escolhe automaticamente o primeiro consultório livre (ordem pelo nome).

## Notificações (RN-090 a RN-099)

- **RN-090 — Eventos notificados.** O paciente é notificado na criação, na remarcação e no cancelamento do agendamento, e recebe um lembrete **[padrão: 24h]** antes, com link de confirmação.
- **RN-091 — Prazo de confirmação.** O paciente só pode confirmar até o horário de início.

## Lista de espera (RN-100 a RN-109) — Fase 5

- **RN-100 — Entrada.** O paciente entra na lista de espera informando procedimento, período desejado (data inicial e final) e, se quiser, dentista.
- **RN-101 — Ordem de oferta.** Quando um horário compatível é liberado, os **[padrão: 3]** primeiros da fila (por ordem de entrada) são notificados. O primeiro que confirmar fica com o horário.

## Acesso e segurança (RN-110 a RN-119)

- **RN-110 — Perfis.**

| Ação | ADMIN | RECEPCAO | DENTISTA | PACIENTE |
|---|:---:|:---:|:---:|:---:|
| Configurar clínica, dentistas, consultórios, procedimentos | ✅ | — | — | — |
| Cadastrar/editar pacientes | ✅ | ✅ | — | só o próprio |
| Agendar / remarcar / cancelar | ✅ | ✅ | — | só os próprios (RN-056/057) |
| Ver agenda | todas | todas | só a própria | só os próprios |
| Check-in / concluir / marcar falta | ✅ | ✅ | só os próprios | — |
| Criar bloqueio | ✅ | ✅ | só para si | — |
| Relatórios | ✅ | — | só os próprios | — |

- **RN-111 — Isolamento (multi-tenant).** Nenhum usuário acessa dados de outra clínica. Uma tentativa de acesso a recurso de outra clínica responde **404**, não 403, para não revelar que o recurso existe.
- **RN-112 — Senhas.** Mínimo de 8 caracteres, com letras e números. A senha é armazenada com hash (BCrypt), nunca em texto puro.

## Auditoria (RN-120 a RN-129)

- **RN-120 — Trilha de auditoria.** Toda criação ou alteração de agendamento registra quem, quando, a ação, o estado anterior e o estado novo.
