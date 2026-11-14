# ADR-0004 — Domínio isolado de frameworks

- **Status:** Aceito
- **Data:** 2026-11-14

## Contexto
As regras de negócio (RN-xxx) são o ativo mais valioso do sistema. Frameworks, bancos e protocolos mudam. Além disso, o plano de estudos passa meses em Java puro antes de chegar ao Spring.

## Decisão
O pacote `com.odontoflow.domain` **não depende** de nenhum framework (Spring, JPA, Jackson…) nem de I/O. Persistência, notificação e relógio são acessados por **interfaces** definidas no domínio e implementadas fora dele (*ports and adapters*).

## Alternativas consideradas
- **Arquitetura em camadas "tradicional" do Spring desde o início:** é mais simples no começo, mas acopla as regras ao framework e inviabiliza a Fase 1.

## Consequências
- O domínio é testável com JUnit puro, rápido e sem banco.
- As Fases 2 e 3 trocam apenas adaptadores.
- Em troca, há mais classes e algum mapeamento entre camadas. Na Fase 3 vamos reavaliar o quanto de separação vale a pena (ver nota em `06-arquitetura.md`).
