---
name: clean-code
description: >-
  Aplica clean code no Conviver (nomes, métodos, erros, null, testes, Angular).
  Use ao escrever ou refatorar código, ao ouvir clean code, legibilidade,
  nomeação, code review, ou quando uma classe acumular mais de um motivo para mudar.
---

# Clean code — Conviver

Responda em português. Identificadores em inglês. Texto de tela e `detail` de erro em português.

Altere o código do caso de uso. Não reformatar arquivo que a história não tocou. Lombok no backend segue a skill `lombok`. Não acrescentar MapStruct nem biblioteca de estilo se o `pom.xml` ou o `package.json` ainda não a tiverem.

## Java

- O método público do service descreve um caso de uso: `publishProfile`, não `process` ou `handle`.
- Saia cedo com guarda. Sem argumento booleano que mude o comportamento: separe os métodos.
- Exceção de negócio é específica e vira `ProblemDetail` no handler único. Não capture `Exception` para engolir falha.
- `Optional` só como retorno de consulta que pode não achar registro. Sem `Optional` em campo, parâmetro ou entidade.
- Comentário explica porquê (regra de produto ou limite). O nome do método explica o quê.
- Constante nomeada no lugar de número ou texto solto de regra. Papel, caminho e tamanho de página moram numa classe da funcionalidade.
- Entidade e DTO separados. Controller devolve DTO.
- Teste: `method_condition_result`. Um comportamento por teste. Sem teste que só repete o getter.

Se a classe tiver um segundo motivo para mudar, extraia outro tipo no mesmo pacote da funcionalidade. Não crie `Helper` nem `Util` de regra.

## Angular

- Signal para estado de tela. `input`, `output` e `model` são `readonly`. Membro só de template é `protected`.
- HTTP na classe `*Api`, que devolve `Observable`. Componente não monta URL.
- Encadeie com operadores (`switchMap`, `catchError`). Sem `subscribe` aninhado.
- Função de template faz uma coisa óbvia. Regra maior vai para o TypeScript.
- Nome de handler é a ação (`saveProfile`), não o evento (`handleClick`).
- Spec no mesmo diretório do arquivo, cobrindo o comportamento que a mudança alterou.

## Limite

Função longa o bastante para não caber na tela sem rolar merece extração. Não persiga contagem fixa de linhas se o método é uma sequência clara de um caso de uso.
