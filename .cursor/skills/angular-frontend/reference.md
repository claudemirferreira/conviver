# Referência — frontend Conviver

## proxy.conf.json

Na raiz de `frontend/`:

```json
{
  "/api": {
    "target": "http://localhost:8080",
    "secure": false
  }
}
```

Em `angular.json`, no target `serve` do projeto:

```json
"options": {
  "proxyConfig": "proxy.conf.json"
}
```

Subir com `npx ng serve`. A API Spring Boot precisa estar na porta 8080.

## Interceptor

Um interceptor em `core/http/api.interceptor.ts`:

- não altera URL que já começa com `/api`
- se no futuro houver token, envia `Authorization` e não grava o segredo em `environment.ts`

`environment.ts` guarda só flags de build (`production`). A base da API é sempre `/api/v1`.

## Tela que lista um recurso

```
features/religion/
  religion.routes.ts
  religion-api.ts
  religion.ts
  religion-list/
    religion-list.ts
    religion-list.html
    religion-list.scss
    religion-list.spec.ts
```

`ReligionApi` chama `GET /api/v1/religions`. `ReligionList` trata lista vazia, espera e `ProblemDetail`. Seletor: `cv-religion-list`.

O nome `religion` é exemplo de pasta. Não crie religiões que o material não nomeou.

## Build de produção

`npx ng build` gera `frontend/dist`. O backend não embute esses arquivos nesta fase; são apps separados no repositório.
