---
name: lombok
description: >-
  Define o uso de Lombok no backend do Conviver (anotações permitidas,
  entidade JPA, serviço, log, configuração). Use ao escrever Java no backend,
  ao gerar o projeto, ou quando o usuário mencionar Lombok, getter, setter,
  @Slf4j, @RequiredArgsConstructor ou boilerplate.
---

# Lombok — Conviver

Lombok só no `backend/`. DTO de entrada e saída continua `record`. Angular não usa Lombok.

A versão vem do parent do Spring Boot, sem número fixo no `pom.xml`. Se essa versão não compilar em Java 25, suba só o Lombok para a estável que declare suporte a Java 25.

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

Se o `maven-compiler-plugin` definir `annotationProcessorPaths`, inclua o Lombok nesse caminho. Sem isso, o processador deixa de rodar.

Na raiz de `backend/`, o arquivo `lombok.config`:

```
config.stopBubbling = true
lombok.addLombokGeneratedAnnotation = true
lombok.accessors.chain = false
lombok.equalsAndHashCode.callSuper = call
lombok.toString.callSuper = skip
```

## Onde usar

| Tipo | Anotações |
|---|---|
| Service, controller, componente | `@RequiredArgsConstructor` e `@Slf4j` |
| Entidade | `@Getter` e `@NoArgsConstructor(access = AccessLevel.PROTECTED)` |
| Campo da entidade sem invariante | `@Setter` no campo. Campo com regra muda por método do agregado, skill `ddd` |
| Associação, senha, hash, token | `@ToString.Exclude` se a classe tiver `@ToString` |

`@RequiredArgsConstructor` vale quando toda dependência é `private final`. Sem `@Autowired` no construtor.

`@Slf4j` cria o campo `log`. Não declare outro `Logger` na mesma classe. O que pode ir no log continua na skill `logging`.

## Fora da entidade

Sem `@Data`, `@Value`, `@Builder`, `@AllArgsConstructor`, `@EqualsAndHashCode` e `@ToString` em entidade. Igualdade de entidade, quando a história pedir, é explícita e usa só o `id` já persistido.

Sem `@SneakyThrows`, `@Synchronized`, `val`/`var` do Lombok, `@UtilityClass` e recurso experimental. Sem Lombok em `record`.

## Exemplo de entidade

```java
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Company {

    @Id
    private UUID id;

    @Setter
    @Column(nullable = false)
    private String name;
}
```

## Exemplo de service

```java
@Service
@RequiredArgsConstructor
@Slf4j
public class CompanyService {

    private final CompanyRepository companies;

    @Transactional(readOnly = true)
    public CompanyResponse find(UUID id) {
        log.info("Consulta de empresa event=company.find companyId={}", id);
        // ...
    }
}
```
