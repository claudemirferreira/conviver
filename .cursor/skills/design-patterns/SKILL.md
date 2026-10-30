---
name: design-patterns
description: >-
  Escolhe padrões de projeto no Conviver (camadas, mapper, strategy, eventos,
  componentes). Use ao desenhar uma funcionalidade, ao ouvir padrão de projeto,
  design pattern, strategy, factory, mapper, ou quando a estrutura de uma
  classe ficar grande demais para o caso de uso.
---

# Padrões de projeto — Conviver

A estrutura padrão já é o padrão. Pacote por funcionalidade, como em `spring-boot-backend` e `angular-frontend`. Só acrescente outro padrão quando a condição abaixo for verdadeira.

## Backend

| Situação | Padrão | Onde |
|---|---|---|
| Um caso de uso com transação | Application service | um método público em `service` |
| Regra que o objeto precisa manter sozinho | Agregado | skill `ddd`, método na raiz em `domain` |
| Campos de entidade e DTO divergem | Mapper explícito | classe package-private no pacote da funcionalidade |
| Duas ou mais implementações escolhidas em tempo de execução | Strategy | interface no pacote + beans Spring |
| Efeito que não pode desfazer o caso de uso (aviso, auditoria) | Evento Spring depois do commit | só quando o efeito já existe ou a falha dele não pode responder erro ao usuário |
| Três ou mais filtros opcionais na mesma consulta | Specification | no repositório dessa funcionalidade |

Mapper manual. Sem MapStruct, ModelMapper ou Dozer.

Condição para strategy e evento: a segunda implementação ou o segundo ouvinte já faz parte da história. Uma interface com uma classe só não entra.

## Angular

- A tela chama a classe `*Api` e guarda o estado em signals.
- Extraia componente de apresentação quando o mesmo trecho aparecer em duas telas.
- Lógica pura (formatação, regra de formulário) fica em função no arquivo da funcionalidade, fora do componente.

## Fora do projeto

Não crie base genérica de CRUD, repositório que embrulha o Spring Data, service locator, singleton manual, abstract factory, camada `util` de regra de negócio, nem pacote `pattern` separado da funcionalidade. Bean Spring dispensa singleton. Configuração fica em `@Configuration` da funcionalidade.
