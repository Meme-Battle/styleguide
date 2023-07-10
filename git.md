# Git

## Fluxo Git

O fluxo recomendado é um simples modelo de feature branch, com as seguintes características:

1. A branch `main` sempre está num estado "deployable", ou seja, pode ir para produção.
2. A branch `develop` será onde iremos trabalhar.
3. Todas as mudanças são feitas através de pull requests + merge, por meio da criação de features branches. Os commits direto na `main` ou `develop` não são permitidos.
4. Para resolver conflitos e atualizar com a `develop`, use rebase na sua feature branch.

Fluxo de desenvolvimento de uma nova funcionalidade segue os seguintes passos:

1. Dev -> Cria uma nova branch `feat/[nome da feature]`.
2. Dev -> Durante o desenvolvimento, realiza commits na branch criada.
3. Dev -> Gera builds da feature branch conforme necessidade.
4. Dev -> Ao término do desenvolvimento da feat, irá fazer `git rebase develop`, para atualizar com o estado atual da branch `develop` e resolver possíveis conflitos.
5. Dev -> Faz o push da branch e cria um PR(pull request) no repositório para a branch `develop`, colocando as pessoas da equipe como reviewers.
6. Equipe -> Analisa o código, uma vez que não encontrem problemas, aprova o PR(pull request).
7. Dev -> Após o PR(pull request) aprovado, faz o merge via interface do repositório.

## Definição de nomenclatura

### Branches

Os nomes dos branches criados devem seguir o seguinte padrão de nomenclatura.

Se houver um card criado no Jira, deve-se utilizar o card id:

```
tipo/CARD_ID
```

Exemplos:
- `feat/MB-13`
- `fix/MB-38`

Se não houver um card criado no Jira, deve-se utilizar o escopo da mudança:

```
tipo/ESCOPO
```

Exemplos:
- `feat/add-logs`
- `fix/input-mask`

O tipo deve-se relacionar ao que aquela branch busca resolver, por exemplo, se será desenvolvida uma funcionalidade nova, se será implementada uma correção, etc.

Alguns tipos possíveis são os seguintes:

- feat (será implementada uma nova funcionalidade)
- fix (será implementada uma correção para resolver um problema existente)
- refactor (será realizado algum refactor em parte do código que já funciona)
- chore (será implementa alguma melhoria de código e/ou infraestrutura)
- test (será desenvolvido algum novo cenário de teste para a aplicação)
- docs (será feita alguma mudança na documentação)

### Mensagens de commit

Para as mensagens de commit deve-se seguir o seguinte esquema de nomenclatura:

```
tipo(ESCOPO): mensagem
```

Aqui, o tipo e o ESCOPO devem se referir à branch que está sendo utilizada para o desenvolvimento. Já a mensagem deve descrever brevemente o conteúdo daquela alteração.

### Mensagens de commit com descrição longa

Para as mensagens de commit que necessitarem de uma descrição mais detalhada do que foi implementado, recomendamos o seguinte padrão:

```
tipo(ESCOPO): resumo da mensagem

A mensagem detalhada da implementação.
```

## Trabalhando com branches

Dado que uma branch `<sua_branch>` está atrás da `develop`, você pode seguir o seguinte processo, para atualizar ele com a `develop`:

1. Verifique se todas as mudanças feitas estão commitadas e que nada ficou no em `stash`;
2. Faça um push para a branch remota: `git push origin <sua-branch>`
3. Retorne a branch develop: `git checkout develop`
4. Atualize a develop: `git pull origin develop`
5. Volte para a sua branch: `git checkout <sua-branch>`
6. Faça rebase da develop: `git rebase develop`
7. Se conflitos aparecerem, resolva eles e então adicione as resoluções (`git add <arquivo-arrumado>`), e após resolver todos os conflitos continue o rebase (`git rebase --continue`)
8. Após o rebase você agora precisa atualizar a sua branch remota: `git push -f` (sim -f, porquê após o rebase os seus commits tiveram seus hashes atualizados)
9. Agora você pode finalmente criar o Pull Request (PR) e pegar um pouco de café.