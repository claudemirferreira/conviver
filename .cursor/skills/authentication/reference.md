# Referência — autenticação

## Dependências

Declare os starters de Spring Security e de OAuth2 Resource Server que o BOM do `pom.xml` já gerencia. Confira o `artifactId` nessa geração do Spring Boot antes de fixar o nome. Emissor e validador usam a API `JwtEncoder` / `JwtDecoder` do Spring Security, com a mesma chave HMAC.

Converta a claim `roles` em autoridades `ROLE_*`. Não use o prefixo `SCOPE_`.

## Cadeia

- CSRF ligado para `/api/v1/auth/refresh` e `/api/v1/auth/logout`.
- Login anônimo e sem sessão.
- CORS com credencial só para a origin do Angular. No perfil local, a origin do `ng serve`.
- Sem `permitAll` amplo em `/api/**`.

## Tabelas

O esquema entra por Flyway, como na skill `database`.

```sql
create table app_user (
    id uuid primary key default gen_random_uuid(),
    email varchar(320) not null,
    password_hash varchar(100) not null,
    name varchar(200) not null,
    role varchar(40) not null,
    company_id uuid,
    failed_attempts integer not null default 0,
    locked_until timestamptz,
    created_at timestamptz not null default now(),
    constraint uq_app_user_email unique (email)
);

create table refresh_token (
    id uuid primary key default gen_random_uuid(),
    user_id uuid not null references app_user (id),
    token_hash varchar(64) not null,
    expires_at timestamptz not null,
    revoked_at timestamptz,
    constraint uq_refresh_token_hash unique (token_hash)
);

create index idx_refresh_token_user_id on refresh_token (user_id);
```

`role` é texto validado no enum Java da história. Sem check no banco enquanto a lista de papéis estiver em aberto. `password_hash` não sai em DTO.

## Angular

```
frontend/src/app/core/auth/
  auth-api.ts
  auth-session.ts
  auth.interceptor.ts
  auth.guard.ts
```

`auth-session` guarda o acesso num signal. O refresh permanece no cookie.
