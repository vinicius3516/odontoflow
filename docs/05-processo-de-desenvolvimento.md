# 05 — Processo de Desenvolvimento

## Papéis

| Papel | Quem | Responsabilidades |
|---|---|---|
| **Tech Lead / PO** | Claude | Produto, regras de negócio, backlog, refinamento, arquitetura, revisão de PRs, acompanhamento das sprints |
| **Desenvolvedor** | Vinicius | **Todo o código**, testes, abertura de PRs, respostas a revisões, merge, deploy |

### Acordo de trabalho
1. **O Tech Lead não escreve código de produção.** Ele explica conceitos, aponta caminhos, indica documentação e revisa. Se travar, peça uma **dica**, não a solução.
2. Dúvida de regra de negócio se resolve **na issue** (comentário), não "de cabeça". Se a regra mudar, o PR altera o [`02-regras-de-negocio.md`](02-regras-de-negocio.md).
3. Ninguém faz commit direto na `main`.
4. Quando não concordar com uma revisão, argumente. Revisão é conversa, não ordem.

## Sprints

- **Duração:** 2 semanas.
- **Planning (início):** escolhemos as issues da coluna *Ready* compatíveis com a seção do curso em que você está e definimos o **objetivo da sprint**.
- **Durante a sprint:** mantenha o board atualizado. Se estiver bloqueado, comente na issue e coloque a label `bloqueado`.
- **Review + Retro (fim):** registramos em `docs/sprints/sprint-XX.md` o que foi entregue, o que você aprendeu, o que travou e o que muda na próxima.
- **Refinamento:** o Tech Lead detalha as issues da próxima sprint antes da planning. Issues com a label `precisa-refinamento` **não** podem ser puxadas.

## Kanban (GitHub Projects)

| Coluna | Significado |
|---|---|
| **Backlog** | Ideias e issues ainda não refinadas |
| **Ready** | Refinada, atende à *Definition of Ready* |
| **In Progress** | Você está trabalhando. **Limite: 2 itens** |
| **In Review** | PR aberto aguardando revisão |
| **Done** | PR mergeado e critérios de aceite atendidos |

## Definition of Ready (DoR)
Uma issue está pronta para ser puxada quando tem:
- [ ] contexto (o porquê) e RNs relacionadas;
- [ ] critérios de aceite verificáveis;
- [ ] pré-requisito de estudo indicado (`secao:XX`) e já estudado;
- [ ] tamanho que cabe em no máximo ~3 dias de trabalho.

## Definition of Done (DoD)
- [ ] Critérios de aceite atendidos.
- [ ] Código compila e `mvn verify` passa localmente (e no CI, quando existir).
- [ ] Testes cobrindo as RNs tocadas (a partir da Sprint 4).
- [ ] Sem código comentado, `System.out` de debug ou TODO sem issue.
- [ ] Documentação atualizada, se o comportamento mudou.
- [ ] PR revisado, comentários resolvidos e mergeado com *squash*.

## Git

### Branches
Fluxo **trunk-based**: a `main` sempre funciona, e as branches são curtas (vivem dias, não semanas).

```
<tipo>/<numero-da-issue>-<descricao-curta>
feat/12-calcular-termino-consulta
fix/31-cpf-com-mascara
refactor/40-migrar-para-java-time
docs/5-ajuste-rn-061
```
Tipos: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`.

### Commits — [Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0/), em inglês
```
feat(scheduling): calculate appointment end time
fix(patient): accept CPF with punctuation
refactor(schedule): replace int minutes with LocalTime
test(appointment): cover RN-051 dentist overlap
docs: add RN-066 check-in window
```
- Um commit = uma mudança lógica. Use o imperativo ("add", não "added").
- Escopos sugeridos: `clinic`, `dentist`, `room`, `procedure`, `patient`, `scheduling`, `availability`, `cli`, `build`.

### Pull Requests
1. Um PR resolve **uma** issue. O corpo do PR traz `Closes #N`.
2. PR **pequeno**: o ideal é menos de 300 linhas alteradas. Se crescer, divida.
3. Antes de pedir revisão, **revise o próprio diff** no GitHub. Você vai achar coisas.
4. Peça a revisão aqui no Claude Code, por exemplo: *"revise o PR #12"*.
5. Merge com **squash and merge**; a branch é apagada em seguida.

### Como funciona a revisão
O Tech Lead revisa pelo GitHub com comentários **no próprio PR**. Como a conta do GitHub é a sua, o GitHub não permite "Approve"/"Request changes" formais. Por isso, os comentários usam prefixos:

| Prefixo | Significado |
|---|---|
| `[bloqueante]` | Precisa mudar antes do merge |
| `[sugestão]` | Melhoraria o código; você decide, mas justifique se recusar |
| `[pergunta]` | Quero entender sua decisão |
| `[nit]` | Detalhe cosmético |
| `[elogio]` | Ficou bom e vale repetir |

O PR está liberado para merge quando o Tech Lead comentar **`LGTM ✅`**.

## Labels

| Grupo | Labels |
|---|---|
| Tipo | `tipo:epico`, `tipo:historia`, `tipo:task`, `tipo:bug`, `tipo:refactor`, `tipo:spike`, `tipo:docs` |
| Fase | `fase:0-setup` … `fase:5-produto` |
| Estudo | `secao:04` … `secao:24` (pré-requisito do curso) |
| Status | `precisa-refinamento`, `bloqueado` |

## Versionamento
[SemVer](https://semver.org/lang/pt-BR/). Cada fase concluída gera uma tag (`v0.1.0`…) e uma release no GitHub com notas.
