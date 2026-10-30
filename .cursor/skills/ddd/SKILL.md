---
name: ddd
description: >-
  Aplica DDD tático no Conviver (linguagem ubíqua, agregado, objeto de valor,
  invariante, repositório da raiz). Use ao modelar regra de negócio, entidade,
  agregado, bounded context, ou quando o usuário mencionar DDD, domínio ou
  linguagem ubíqua.
---

# DDD — Conviver

Um contexto: o estudo descrito na spec ativa. A linguagem ubíqua sai dessa spec, com código em inglês. Em `specs/001-estudo-web` os nomes são `Profile`, `DatedFact`, `Account`, `Quiz`, `Certificate` e `WorkplaceSituation`. Papéis: `Visitor`, `Student`, `Editor`. Não introduza `Religion`, `Tradition` ou `Content` como segundo nome do mesmo conceito.

Não crie segundo bounded context, shared kernel, CQRS nem event sourcing. Isso só entra quando uma spec nova tiver modelo e vocabulário próprios.

## Onde a regra mora

| Regra | Onde |
|---|---|
| Invariante de um agregado | método na raiz, no pacote `domain` |
| Caso de uso, transação, contagem, outro agregado | `service` |
| Leitura sem regra | `repository`, chamado pelo service |
| HTTP | `controller`, só DTO |

A entidade JPA é o objeto de domínio. Não duplique um modelo rico e outra entidade anêmica. Anotação JPA na entidade é permitida. Método de domínio não chama repositório, não conhece DTO e não devolve `ProblemDetail`. A exceção de domínio é específica; o handler web traduz para `ProblemDetail`.

## Agregado

A raiz é a única porta de alteração do conjunto. Entidade filha não tem repositório. Outro agregado entra só pelo `UUID`. Uma transação grava um agregado.

`DatedFact` pertence a `Profile`. `Certificate` guarda `accountId` e `quizId`; não carrega `Account` para decidir se o aluno passou.

Campo que participa de invariante não tem `@Setter`. A mudança passa por método (`publish`, `recordAttempt`). `@Setter` fica no campo que a spec trata como dado livre, conforme a skill `lombok`.

## Tipos

- Entidade: tem `UUID` e ciclo de vida.
- Objeto de valor: `record` imutável, sem id, no pacote `domain` (`SourceNote`, nota do quiz).
- Conjunto fechado da spec: `enum` (`RightsStatus`, `PublicationStatus`).
- Serviço de domínio: só quando a regra usa dois agregados e não cabe em nenhum dos dois. Contar perfis publicados para recusar o 8º é caso de uso: o service obtém a contagem e chama o método da raiz.

## Repositório

Spring Data na raiz do agregado. Sem interface de domínio paralela e sem repositório genérico. Coleção filha usa cascata a partir da raiz; filho que só existe dentro do agregado sai junto quando removido da coleção.

## Exemplo

```java
public void publish(int alreadyPublished) {
    if (alreadyPublished >= 7) {
        throw new ProfileLimitReachedException();
    }
    if (!readyToPublish()) {
        throw new ProfileNotPublishableException();
    }
    this.publicationStatus = PublicationStatus.PUBLISHED;
}
```

`readyToPublish` exige os cinco blocos, a fonte e `ORIGINAL_SUMMARY` ou `AUTHORIZED_QUOTATION`. `PENDING_QUOTATION` não publica. O service abre a transação, conta os publicados e chama `publish`.

## Fora deste DDD

Base de agregado, fábrica abstrata, camada `infra` só para esconder o Spring Data, e classe `Rules` ou `Helper` com a invariante que a raiz deveria ter. Evento de domínio segue a skill `design-patterns`: depois do commit, e só quando a spec já tiver esse efeito.
