# Referência — logs

## Eventos de autenticação

| Evento | Nível | Campos |
|---|---|---|
| `auth.login.succeeded` | INFO | `userId`, `companyId` se houver |
| `auth.login.failed` | WARN | nenhum identificador digitado |
| `auth.login.locked` | WARN | `userId` |
| `auth.refresh.succeeded` | INFO | `userId` |
| `auth.refresh.reused` | WARN | `userId` |
| `auth.logout` | INFO | `userId` |

## Exemplo

```java
log.info("Login aceito event=auth.login.succeeded userId={}", userId);
log.warn("Login recusado event=auth.login.failed");
```

Com log estruturado, prefira a API de key-value do SLF4J fluente quando a versão do projeto expuser `atInfo().addKeyValue(...)`. Caso o binding não tenha key-value, mantenha a mensagem parametrizada acima. Não monte JSON à mão na string.

## Pedido HTTP

Um filtro, depois da autenticação, registra em INFO o fim do pedido: método, caminho, status e duração. Não registre query string que carregue e-mail ou token. Caminho de saúde (`/actuator/health`) fica de fora para não encher o log.
