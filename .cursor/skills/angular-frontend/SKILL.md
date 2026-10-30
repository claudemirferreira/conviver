---
name: angular-frontend
description: >-
  Implementa e altera o frontend Angular do Conviver (componentes standalone,
  rotas, formulários, chamadas à API). Use ao trabalhar em frontend/, Angular,
  componentes, telas, templates, rotas, ou quando o usuário mencionar Angular
  ou frontend.
---

# Frontend Angular — Conviver

Responda em português. Código (arquivos, classes, métodos, seletores) em inglês. Texto visível ao usuário em português.

Antes de criar tela de negócio, leia `documentacao-projeto/prompt-plataforma-religiao.md` e a spec da funcionalidade em `.specify/`. Funcionalidade nova segue o Spec Kit: `/speckit-specify`, `/speckit-plan`, `/speckit-tasks` e só então `/speckit-implement`. Não invente religiões, parceiros, preços ou prazos. Se faltar decisão de produto, marque em aberto e pergunte. `documentacao-projeto/prompt-uri-org.md` não é especificação de tela.

## Layout

- App em `frontend/`.
- Angular atual da CLI, componentes standalone, roteamento, SCSS, sem SSR.
- O repositório git já existe: gerar com `--skip-git`.
- Sem NgModules em código novo. Sem NgRx até uma funcionalidade precisar de estado compartilhado complexo.

Quando `frontend/` ainda não existir, na raiz do repositório, com a CLI estável atual:

```powershell
npx -y @angular/cli@latest new frontend --directory frontend --routing --style scss --prefix cv --no-ssr --skip-git
```

`--skip-git` não inicia outro repositório; o `.gitignore` fica em `frontend/`. Se a CLI perguntar, confirme standalone. Leia a versão gerada em `frontend/package.json` e use essa daí em diante. Siga o guia de nomes que a CLI gravou (padrão 2025: `religion-list.ts`, não `religion-list.component.ts`).

## Pastas

Agrupe por funcionalidade, não por tipo (`components/`, `services/`, `pages/`).

```
frontend/src/app/
  core/http/                 # interceptor e config da API — um por app
  shared/                    # peças sem regra de negócio
  features/<nome>/
    <nome>.routes.ts
    <nome>-api.ts            # classe <Nome>Api
    <nome>.ts                # interfaces da API, se forem pequenas
    <recurso>/
      <recurso>.ts
      <recurso>.html
      <recurso>.scss
      <recurso>.spec.ts
```

Rotas de funcionalidade em lazy load a partir de `app.routes.ts`. Prefixo de seletor: `cv`. Injete dependências com `inject()`. Membros lidos só pelo template ficam `protected`. `input`, `output` e `model` ficam `readonly`.

## API

O browser chama caminho relativo `/api/v1/...`. Em desenvolvimento, `proxy.conf.json` encaminha `/api` para `http://localhost:8080`. Registre esse arquivo em `angular.json` (`serve.options.proxyConfig`). Não grave `localhost:8080` nos serviços.

- Sucesso: o corpo é o recurso. Não há envelope `{ data, success }`.
- Lista paginada: `{ content, page, size, totalElements }`, `page` desde 0.
- Erro: `ProblemDetail`. Mostre `detail` ao usuário. Validação lê `errors`.
- Datas ISO-8601. IDs são string UUID.
- `provideHttpClient(withInterceptors(...))` em `app.config.ts`. Login, token e cookie seguem a skill `authentication`. Não grave token no cliente antes dessa skill.

Interfaces da API ficam em `<nome>.ts`. Formulários reativos. Estado de tela com signals no componente. Chamada HTTP na classe `<Nome>Api`, que devolve `Observable`.

## Interface

- Rótulo visível em todo campo. Botão com texto, não só ícone.
- Estado vazio, carregando e erro em toda lista que vem da API.
- Layout responsivo. O material pede site web, não app nativo.

Fora desta fase: app nativo, loja, fórum, mapa, role-play e ranking.

## Verificação

Na pasta `frontend/`:

- `npx ng build` após mudança de compilação ou rota.
- `npx ng test --watch=false` quando alterar serviço ou regra de tela coberta por spec.
- Mudança de tela, fluxo, estilo ou dado renderizado: exercite no browser (clique, formulário, navegação), não só confira o snapshot.

Detalhe de proxy, interceptor e página: [reference.md](reference.md).
