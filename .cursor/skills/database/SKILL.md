---
name: database
description: >-
  Define banco de dados do Conviver (PostgreSQL, Flyway, JPA, UUID, transação,
  paginação, Testcontainers). Use ao criar tabela, entidade, repositório,
  migração, consulta, índice, ou quando o usuário mencionar banco, Postgres,
  Flyway, JPA ou SQL.
---

# Banco de dados — Conviver

PostgreSQL. Esquema só por Flyway, em `backend/src/main/resources/db/migration`. `ddl-auto: validate`. Sem `ddl-auto=update` e sem edição de migração já aplicada: a mudança nova é o próximo `V{n}__snake_case.sql`.

Não invente religiões, parceiros ou massa de demonstração. Seed de negócio só com dado que a história tiver nomeado. Tabela de usuário e refresh está na skill `authentication`.

## Modelo

- Chave `uuid` com `default gen_random_uuid()`.
- Instante em `timestamptz`. Data civil em `date`.
- Coluna `snake_case`, alinhada à estratégia padrão do Spring Boot a partir do nome camelCase do campo. Use `@Column` só quando o nome fugir dessa regra.
- `not null`, salvo campo que o material marca como opcional.
- Tabela de dado de uma empresa leva `company_id uuid not null` e índice. Consulta desse dado filtra pelo `companyId` do token.
- Chave de negócio vira `unique` (e-mail de usuário, por exemplo).
- Índice para toda chave estrangeira e para coluna usada em filtro ou ordenação. Nome `idx_<tabela>_<colunas>`.
- `@Version` quando duas pessoas podem gravar o mesmo agregado.
- Associação `LAZY`. `EAGER` não entra. A busca do caso de uso usa `@EntityGraph` ou `join fetch` quando a tela precisa da associação.

## Consulta e transação

- Repositório Spring Data no pacote da funcionalidade. Sem repositório genérico por cima.
- Nome de método enquanto o filtro for simples. `@Query` JPQL quando o nome não alcançar. SQL nativo só quando JPQL não expressar a consulta.
- Página no banco, tamanho máximo 100. O controller devolve `{ content, page, size, totalElements }`, não o `Page` do Spring.
- `@Transactional` no service. Leitura com `readOnly = true`. Transação de escrita não chama HTTP externo.
- Apagar é físico até a história pedir retenção. Se pedir, coluna `deleted_at timestamptz` e filtro nas consultas dessa tabela.

## Teste

Teste que abre banco usa Testcontainers com PostgreSQL, no major que o projeto já tiver declarado. Se nenhum estiver declarado, use o major estável atual e registre esse número num único lugar (compose ou propriedade de teste). Teste de slice sem SQL específico não sobe container.

Exemplo de migração e de entidade: [reference.md](reference.md).
