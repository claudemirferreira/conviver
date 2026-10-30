# Referência — backend Conviver

## Árvore de uma funcionalidade

```
backend/src/main/java/com/setebit/conviver/religion/
  controller/ReligionController.java
  service/ReligionService.java
  repository/ReligionRepository.java
  domain/Religion.java
  dto/ReligionResponse.java
  dto/ReligionRequest.java
backend/src/main/resources/db/migration/V1__create_religion.sql
backend/src/test/java/com/setebit/conviver/religion/ReligionControllerTest.java
```

O nome `religion` é exemplo de pacote. Não crie religiões que o material não nomeou.

## ProblemDetail

Handler único em `com.setebit.conviver.shared.web`. Mapeie:

- validação → 400, `errors` com o nome do campo
- recurso ausente → 404
- regra de negócio violada → 409 ou 422, `detail` em português, curto, sem jargão interno

`type` é um caminho estável `/problems/<slug>`. Não invente domínio.

## Perfis

- `application.yaml`: porta 8080, Flyway ligado, `ddl-auto: validate`, datasource pelas variáveis de ambiente.
- Testes de integração: Testcontainers define a URL. Não apontar teste para um Postgres de desenvolvimento.

## springdoc

Dependência: `org.springdoc:springdoc-openapi-starter-webmvc-ui`. Versão compatível com o Spring Boot do `pom.xml`. UI em `/swagger-ui.html`. Não duplique a documentação em markdown de endpoint.
