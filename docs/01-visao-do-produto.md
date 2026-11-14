# 01 — Visão do Produto

## Declaração de visão

> **Para** clínicas odontológicas de pequeno e médio porte
> **que** perdem tempo e dinheiro gerenciando a agenda por WhatsApp, caderno e planilha,
> **o OdontoFlow** é uma plataforma de agendamento online
> **que** organiza a agenda de dentistas e consultórios, permite que o paciente marque sozinho e reduz faltas com confirmações e lembretes automáticos.
> **Diferente de** agendas genéricas, o OdontoFlow entende o dia a dia de uma clínica: cadeira odontológica como recurso, tempo de higienização entre atendimentos, procedimentos que exigem avaliação prévia e política de faltas.

## O problema

| Dor | Quem sente | Consequência |
|---|---|---|
| Agenda espalhada (WhatsApp, caderno, planilha) | Recepção | Retrabalho, conflitos de horário e encaixes errados |
| Paciente só consegue marcar em horário comercial | Paciente | Desiste ou procura outra clínica |
| Faltas sem aviso (no-show) | Dono(a) da clínica | Cadeira ociosa = receita perdida |
| Cadeira livre, mas dentista ocupado (ou o contrário) | Recepção / dentistas | Subutilização dos recursos |
| Nenhum dado sobre ocupação e faltas | Dono(a) da clínica | Decisões no "achismo" |

## Personas

**Dra. Ana — dona da clínica (perfil `ADMIN`)**
Dentista e gestora de uma clínica com 3 dentistas e 2 consultórios. Quer ver a ocupação da semana, reduzir faltas e parar de depender da recepção para tudo.

**Carla — recepcionista (perfil `RECEPCAO`)**
Atende telefone e WhatsApp o dia inteiro. Precisa encontrar um horário livre em segundos, remarcar sem bagunçar a agenda e saber quem confirmou para amanhã.

**Dr. Pedro — dentista associado (perfil `DENTISTA`)**
Atende 3 dias por semana. Quer ver só a própria agenda do dia, marcar quem faltou e não ser surpreendido por encaixes fora da escala.

**João — paciente (perfil `PACIENTE`)**
Trabalha em horário comercial. Quer marcar a limpeza às 22h pelo celular, receber um lembrete e confirmar com um clique.

## Proposta de valor (o que entregamos)

1. **Agenda única e sem conflitos**: dentista, consultório e paciente nunca têm dois compromissos sobrepostos.
2. **Autoagendamento 24/7** com regras da clínica (antecedência, procedimentos permitidos online).
3. **Menos faltas**: confirmação, lembrete, lista de espera e política de faltas.
4. **Visão gerencial**: ocupação, taxa de faltas e produção prevista por dentista.
5. **(Fase 5) Assistente com IA** que conversa com o paciente e agenda de verdade.

## Escopo

### Dentro do escopo (MVP — fim da Fase 4)
- Cadastro e configuração da clínica (fuso horário, expediente, granularidade, higienização).
- Cadastro de dentistas (CRO), escalas, consultórios e procedimentos.
- Cadastro de pacientes (com responsável legal para menores) e consentimento LGPD.
- Agendamento, remarcação, cancelamento, confirmação, check-in, conclusão e falta.
- Consulta de horários disponíveis.
- Bloqueios de agenda (férias, feriado, manutenção).
- Autenticação, perfis de acesso e isolamento entre clínicas (multi-tenant).
- E-mail de confirmação e recuperação de senha.

### Pós-MVP (Fase 5)
Lembretes automáticos em lote, lista de espera, painel em tempo real, upload de logo e fotos, relatórios, cache, mensageria entre módulos, login com OAuth2/OIDC, deploy na AWS e assistente com IA.

### Fora do escopo (decisão consciente)
- **Prontuário eletrônico e odontograma**: têm regulação própria (CFO), são outro produto.
- **Financeiro, faturamento, convênios/TISS e nota fiscal.**
- **Estoque, prescrição e laboratório.**
- **Frontend**: o produto é a **API**. Ela é consumida via Swagger/Postman. Um frontend pode vir depois, como projeto separado.

> Saber o que **não** fazer é tão importante quanto saber o que fazer. Qualquer pedido fora do escopo vira discussão de produto, não uma "tarefinha rápida".

## Métricas de sucesso do produto (quando estiver em uso)
- Percentual de agendamentos feitos pelo próprio paciente.
- Taxa de faltas antes e depois do uso.
- Taxa de ocupação das cadeiras.
- Tempo médio para a recepção encontrar um horário.
