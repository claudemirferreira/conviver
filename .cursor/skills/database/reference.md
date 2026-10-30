# Referência — banco

## Migração

Arquivo: `V2__create_company.sql` (o número seguinte ao da última aplicada).

```sql
create table company (
    id uuid primary key default gen_random_uuid(),
    name varchar(200) not null,
    created_at timestamptz not null default now()
);
```

`V1__` fica livre para a primeira migração real do repositório. Não reutilize número. Não altere um `V` que já tenha rodado em algum ambiente.

Flyway com `out-of-order` desligado. Script repetível (`R__`) só para view ou função, não para tabela.

## Entidade

```java
@Entity
@Table(name = "company")
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Company {

    @Id
    private UUID id;

    @Setter
    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private Instant createdAt;
}
```

Getter, setter e construtor seguem a skill `lombok`. Sem `@Data` nesta classe.

O `id` nasce no banco (`@GeneratedValue` compatível com UUID do PostgreSQL na versão do Hibernate do projeto). Não gere UUID só no cliente e outro no banco.

Campo de senha e de hash de refresh não tem getter em DTO. A entidade não é serializada na API.

## Paginação

```java
PageRequest.of(page, size, Sort.by("createdAt").descending())
```

Recuse `size` acima de 100 com 400 e `ProblemDetail`. `page` ausente vale 0. `size` ausente vale 20.
