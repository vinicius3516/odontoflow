# 03 — Glossário (Linguagem Ubíqua)

A **documentação e o negócio** falam português. O **código** fala inglês ([ADR-0003](adr/0003-idioma-codigo-e-documentacao.md)).
Esta tabela é a ponte entre os dois. Se um termo novo surgir, ele entra aqui **antes** de entrar no código.

| Termo (negócio) | Código | Definição |
|---|---|---|
| Clínica | `Clinic` | Cliente do SaaS (tenant). Tudo pertence a uma clínica. |
| Expediente | `BusinessHours` | Horário de funcionamento da clínica por dia da semana (RN-002). |
| Intervalo de horário | `TimeRange` | Par `[início, fim)` em um dia. |
| Granularidade | `slotGranularity` | Tamanho do bloco da agenda (RN-003). |
| Horário (vago) | `Slot` | Início possível para um agendamento. |
| Higienização | `cleaningInterval` | Intervalo entre atendimentos na mesma cadeira (RN-004). |
| Dentista | `Dentist` | Profissional que realiza procedimentos. |
| CRO | `CroRegistration` | Registro no Conselho Regional de Odontologia (número + UF). |
| Escala | `WorkSchedule` | Horários de trabalho do dentista por dia da semana (RN-011). |
| Habilitação | `DentistProcedure` | Vínculo dentista × procedimento, com duração e preço próprios (RN-012). |
| Consultório / cadeira | `Room` | Recurso físico onde ocorre o atendimento. |
| Procedimento | `Procedure` | Serviço odontológico (limpeza, avaliação, canal…). |
| Paciente | `Patient` | Pessoa atendida. |
| Responsável legal | `Guardian` | Responsável por paciente menor (RN-041). |
| Agendamento | `Appointment` | Reserva de dentista + consultório + paciente + procedimento num horário. |
| Status do agendamento | `AppointmentStatus` | `SCHEDULED`, `CONFIRMED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED`, `NO_SHOW`. |
| Agendado / Confirmado / Em atendimento / Concluído / Cancelado / Faltou | (ver acima) | Estados da RN-060. |
| Remarcação | `reschedule` | Cancela e cria um novo agendamento vinculado (RN-063). |
| Falta | `NO_SHOW` | Paciente não compareceu (RN-062). |
| Cancelamento tardio | `lateCancellation` | Cancelado pelo paciente com menos de 24h (RN-061). |
| Bloqueio de agenda | `ScheduleBlock` | Período indisponível (RN-070). |
| Disponibilidade | `Availability` | Cálculo dos horários livres (RN-080). |
| Autoagendamento | `selfService` | Agendamento feito pelo próprio paciente. |
| Lista de espera | `Waitlist` | Fila de interessados em horários (RN-100). |
| Usuário | `User` | Quem acessa o sistema; tem um perfil (`Role`). |
| Perfil | `Role` | `ADMIN`, `RECEPTIONIST`, `DENTIST`, `PATIENT` (RN-110). |
