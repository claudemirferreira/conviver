<!--
Sync Impact Report
- Version change: 1.1.0 → 1.2.0
- Modified principles: IV. Stack vinculante (inclui a skill ddd); V. Simplicidade (DDD tático no pacote da funcionalidade)
- Added sections: none
- Removed sections: none
- Follow-up TODOs: none
-->

# Conviver Constitution

## Core Principles

### I. Spec antes do código

Toda funcionalidade de negócio DEVE passar por spec, plano e tarefas
antes da implementação. O código DEVE cumprir a spec da funcionalidade
em `.specify/` e NÃO DEVE acrescentar comportamento que ela não peça.

### II. Fidelidade ao material

Fato, inferência e recomendação DEVEM permanecer separados.
O agente NÃO DEVE inventar religiões, parceiros, preços ou prazos.
Decisão em aberto DEVE ser marcada e perguntada.
`documentacao-projeto/prompt-uri-org.md` é referência de site, não especificação.

### III. Comportamento verificado

Mudança de comportamento no backend DEVE passar em `mvn -q verify`.
Mudança de compilação ou rota no frontend DEVE passar em `npx ng build`.
Mudança de tela DEVE ser exercitada no browser.
Teste cobre o comportamento alterado, no formato `method_condition_result`.

### IV. Stack vinculante

Backend: Java 25, Spring Boot da estável que aceite Java 25, Maven,
pacote `com.setebit.conviver`, API `/api/v1`.
Frontend: Angular atual da CLI, componentes standalone, SCSS, prefixo `cv`.
Implementação DEVE seguir as skills em `.cursor/skills/`
(`spring-boot-backend`, `angular-frontend`, `design-patterns`,
`clean-code`, `logging`, `authentication`, `database`, `lombok`, `ddd`).

### V. Simplicidade

A estrutura por funcionalidade É o padrão de projeto.
Strategy, evento ou Specification SÓ entram quando a spec já traz o segundo caso.
Mapper é manual. Sem biblioteca nova se o build ainda não a tiver
e a spec não a pedir. Lombok é exceção: já faz parte da stack do backend.
DDD é tático, dentro do pacote da funcionalidade, conforme a skill `ddd`.

## Restrições técnicas

- PostgreSQL, Flyway e `ddl-auto: validate`. Migração aplicada não se edita.
- Erro de API em `ProblemDetail`. Log sem senha, token, cookie, e-mail completo
  nem conteúdo religioso.
- Autenticação só quando a spec pedir login. Então: JWT de acesso em memória
  e refresh em cookie `HttpOnly`. Papel só entra se a spec o nomear.
- Fora desta fase, até a spec dizer o contrário: app nativo, loja, fórum,
  mapa, role-play e ranking.

## Fluxo

Funcionalidade de produção segue:

1. `/speckit-specify`
2. `/speckit-clarify` quando houver ambiguidade
3. `/speckit-plan`
4. `/speckit-checklist`
5. `/speckit-tasks`
6. `/speckit-analyze`
7. `/speckit-implement`
8. `/speckit-converge` até o relatório dizer que convergiu

Correção pequena, sem regra de negócio nova, PODE ir direto à implementação
desde que não altere contrato, esquema nem permissão.

## Governance

Esta constituição rege o Conviver. Em conflito com uma spec de funcionalidade,
a spec vence no comportamento do produto e esta constituição vence
na stack, no fluxo e nas proibições de invenção.

Emenda exige nova versão semântica neste arquivo:
MAJOR quando um princípio muda de forma incompatível,
MINOR quando um princípio ou seção é acrescentado,
PATCH quando o texto só esclarece.

Toda spec, plano e implementação DEVE ser conferida contra esta constituição.
Orientação de código em tempo de desenvolvimento: `.cursor/skills/`.

**Version**: 1.2.0 | **Ratified**: 2026-10-30 | **Last Amended**: 2026-10-30
