---
name: authentication
description: >-
  Implementa autenticação do Conviver (login, JWT de acesso, refresh em
  cookie, papéis, isolamento por empresa, guarda no Angular). Use ao trabalhar
  em login, sessão, JWT, Spring Security, papel, permissão, rota protegida,
  ou quando o usuário mencionar autenticação.
---

# Autenticação — Conviver

Stateless. A API emite o token; não há servidor de autorização externo nem sessão `JSESSIONID`. Segredo só em variável de ambiente. A skill `spring-boot-backend` continua valendo: não incluir Security na geração inicial do projeto. Inclua quando a história pedir login.

Papéis citados no material e ainda em aberto: visitante, aluno, administrador da empresa, editor, moderador. Use só os papéis que a história nomear. Se ela não nomear, pergunte antes de gravar enum ou regra de acesso. Cada empresa vê só os seus alunos: o `companyId` vem do token, não do corpo do pedido.

Cadastro de escola ou de menor fica de fora até existir regra de privacidade no material.

## Tokens

- Acesso: JWT curto (15 minutos), só na memória do Angular, enviado como `Authorization: Bearer`.
- Refresh: valor opaco, aleatório, guardado no banco como hash SHA-256. Cookie `HttpOnly`, `Secure` fora do perfil local, `SameSite=Strict`, `Path=/api/v1/auth`, 14 dias.
- Claims do acesso: `iss=conviver`, `aud=conviver-api`, `sub` (UUID do usuário), `roles` (lista), `companyId` (ausente quando não houver empresa). Sem e-mail no token.
- Chave HMAC em `JWT_SECRET` (no mínimo 32 bytes). A aplicação não sobe se a variável estiver vazia. Sem valor padrão no yaml.
- Rotacione o refresh a cada uso. Se um refresh já rotacionado reaparecer, revogue todos os refresh daquele usuário e registre `auth.refresh.reused`.

O browser fala com um único origin. Em desenvolvimento o proxy do Angular faz isso; o cookie não depende de chamada direta a `localhost:8080`.

## API

Pacote `com.setebit.conviver.auth`.

| Endpoint | Acesso |
|---|---|
| `POST /api/v1/auth/login` | anônimo |
| `POST /api/v1/auth/refresh` | cookie de refresh + CSRF |
| `POST /api/v1/auth/logout` | cookie de refresh + CSRF |
| `GET /api/v1/auth/me` | autenticado |

Login recebe e-mail e senha e devolve `{ "accessToken", "expiresIn" }` mais o cookie. Senha com bcrypt (força 12), coluna `password_hash`. Mensagem única em português para senha errada, usuário inexistente ou conta bloqueada. Após 5 falhas, bloqueie por 15 minutos.

Demais endpoints exigem autenticação, salvo os que a história marcar como públicos. Negativa é 401 ou 403 com `ProblemDetail`, sem revelar se o recurso existe quando o chamador nem pode vê-lo.

CSRF no refresh e no logout: cookie legível pelo script e cabeçalho `X-XSRF-TOKEN`, pelo mecanismo de cookie do Spring Security. O acesso Bearer dispensa CSRF.

## Angular

Estado em `core/auth`, signal na memória. Sem `localStorage` e sem `sessionStorage` para token. Interceptor acrescenta `Authorization` quando houver acesso, e envia o cabeçalho CSRF no refresh e no logout. Em 401 de chamada comum, tente um refresh; se falhar, limpe a memória e volte à tela de login. Guarda de rota só checa papel que a história tiver definido.

## Log

Siga a skill `logging`. Não registre senha, hash, token nem cookie.

Tabelas, claims e filtro de segurança: [reference.md](reference.md).
