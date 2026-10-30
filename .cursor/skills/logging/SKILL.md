---
name: logging
description: >-
  Define logs do Conviver (SLF4J, log estruturado, correlação, o que não
  registrar, erro no Angular). Use ao adicionar log, auditoria, rastreio,
  MDC, ou quando o usuário mencionar log, logger ou observabilidade.
---

# Logs — Conviver

SLF4J. No backend, `@Slf4j` do Lombok e o campo `log`. Mensagem parametrizada (`{}`), nunca concatenação. Sem `System.out` e sem `console.log` solto.

O código do evento é inglês estável, em minúsculas com ponto: `auth.login.failed`. A mensagem humana é português curto.

## Nível

| Nível | Quando |
|---|---|
| ERROR | falha inesperada com stack no servidor |
| WARN | regra recusada ou dependência instável da qual o pedido se recuperou |
| INFO | subida da aplicação, login, recusa de autenticação, mudança de estado de negócio |
| DEBUG | detalhe de diagnóstico, só no perfil local |

Não use INFO para cada leitura de lista.

## Campos

No perfil local, texto plano. Nos outros perfis, log estruturado do Spring Boot:

```yaml
logging:
  structured:
    format:
      console: ecs
```

Inclua, quando existirem: `event`, `userId`, `companyId`, `httpMethod`, `httpPath`, `httpStatus`, `durationMs`. Use o `traceId` do Micrometer. Não crie filtro próprio de correlação se o tracing da aplicação já preenche o MDC.

## Proibido no log

Senha, hash, segredo JWT, valor de `Authorization`, cookie, corpo de login ou refresh, e-mail completo, texto de resposta de quiz, conteúdo de perfil religioso e dado de menor. Em falha de login registre o evento e o status, sem o identificador que a pessoa digitou.

O `ProblemDetail` de resposta também não leva stack. A stack fica no log ERROR.

## Angular

Um interceptor em `core/http` registra, em build de desenvolvimento, status e caminho quando a API falha. Não registre corpo, cabeçalho `Authorization` nem cookie. Build de produção não escreve esse detalhe no console.

Lista de eventos de autenticação e o formato esperado: [reference.md](reference.md).
