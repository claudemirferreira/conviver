---
name: spring-boot-backend
description: >-
  Implementa e altera o backend Spring Boot do Conviver (API REST, JPA,
  Flyway, testes). Use ao trabalhar em backend/, Java, Maven, controllers,
  services, entidades, migrações, ou quando o usuário mencionar Spring Boot,
  API ou backend.
---

# Backend Spring Boot — Conviver

Responda em português. Pacotes, classes, métodos e endpoints em inglês.

Antes de criar módulo de negócio, leia `documentacao-projeto/prompt-plataforma-religiao.md` e a spec da funcionalidade em `.specify/`. Funcionalidade nova segue o Spec Kit: `/speckit-specify`, `/speckit-plan`, `/speckit-tasks` e só então `/speckit-implement`. Não invente religiões, parceiros, preços ou prazos. Se faltar decisão de produto, marque em aberto e pergunte. `documentacao-projeto/prompt-uri-org.md` é só referência de site; não copie funções de lá para a API.

## Layout

- Código em `backend/`.
- Pacote raiz: `com.setebit.conviver`.
- Java 25. `java.version` e o release do compilador ficam em 25. A versão do Spring Boot é a estável que o Initializr gerar, desde que aceite Java 25. Leia `backend/pom.xml` e use só starters dessa geração. Se o `pom.xml` vier com outra versão de Java, ajuste para 25.
- Maven. Não introduza Gradle.
- Porta `8080`. Prefixo: `/api/v1`.

Quando `backend/` ainda não existir, na raiz do repositório:

```powershell
curl.exe -L "https://start.spring.io/starter.zip?type=maven-project&language=java&javaVersion=25&groupId=com.setebit&artifactId=conviver&name=conviver&packageName=com.setebit.conviver&dependencies=web,data-jpa,validation,postgresql,flyway,lombok&baseDir=backend" -o backend.zip
Expand-Archive -Path backend.zip -DestinationPath . -Force
Remove-Item backend.zip
```

Depois acrescente `springdoc-openapi-starter-webmvc-ui` alinhado ao Spring Boot gerado. Não adicione Spring Security nesta geração.

## Camadas

`controller` → `service` → `repository`. Controller não acessa repositório.

- Entidade JPA fica na persistência. Entrada e saída são `record` em `dto`. Lombok segue a skill `lombok`.
- Jakarta Validation no DTO de entrada.
- `@Transactional` no service; leitura com `readOnly = true`.
- Erro com `ProblemDetail`. Sem stack trace na resposta.
- ID `UUID`. Data `Instant` ou `LocalDate`, JSON ISO-8601, campos camelCase.
- Flyway em `backend/src/main/resources/db/migration`. Sem `ddl-auto=update`.
- PostgreSQL no perfil de execução. Teste de contrato com `MockMvc`. Teste que exige banco usa Testcontainers com PostgreSQL.

Pacote por funcionalidade: `com.setebit.conviver.<funcionalidade>` com `controller`, `service`, `repository`, `domain`, `dto`. Documente o endpoint no springdoc (`/v3/api-docs`).

Não adicione Spring Security na geração inicial. Quando a história pedir autenticação, siga a skill `authentication`. Esquema, migração e consulta seguem a skill `database`. Log segue a skill `logging`. Lombok segue a skill `lombok`. Domínio segue a skill `ddd`. Padrão de projeto e estilo seguem `design-patterns` e `clean-code`.

Fora desta fase: app nativo, loja, fórum, mapa, role-play e ranking.

## Contrato HTTP

- Sucesso devolve o recurso ou a lista, sem envelope `{ data, success }`.
- Lista que pode crescer: `{ "content", "page", "size", "totalElements" }`. `page` começa em 0.
- Erro: `ProblemDetail` (`type`, `title`, `status`, `detail`, `instance`). Validação inclui `errors` (campo → mensagem).
- O Angular em desenvolvimento faz proxy de `/api` para `http://localhost:8080`. Não exija host absoluto no cliente.

## Configuração

Credencial só por variável de ambiente: `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, `SPRING_DATASOURCE_PASSWORD`. Não grave senha em arquivo versionado.

## Comandos

Na pasta `backend/`:

- `mvn -q verify` antes de concluir mudança de comportamento.
- `mvn spring-boot:run` para subir a API.

Detalhe de árvore, erro e perfil: [reference.md](reference.md).
