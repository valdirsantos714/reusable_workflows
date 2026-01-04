# Reusable Workflows

Este repositório contém workflows reutilizáveis do GitHub Actions, projetados para serem facilmente integrados e utilizados em outros projetos. O objetivo é promover a padronização e a reusabilidade de automações, economizando tempo e esforço no desenvolvimento de novos repositórios.


## Como Usar Workflows Reutilizáveis

Para usar os workflows deste repositório em outro projeto, você pode chamá-los usando a sintaxe `owner/repo/.github/workflows/filename@ref`.

### Exemplo de Chamada do Workflow `build.yaml`

Suponha que este repositório esteja em `seu-usuario/reusable_workflows` e você queira chamar o `build.yaml` na branch `main`.

Crie um arquivo de workflow no seu outro repositório (ex: `.github/workflows/my-project-ci.yml`):

```yaml
name: CI for My Project

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  call_reusable_build:
    # Substitua 'seu-usuario' pelo seu nome de usuário ou organização do GitHub
    # Substitua 'main' pela branch, tag ou SHA do commit que você quer usar
    uses: seu-usuario/reusable_workflows/.github/workflows/build.yaml@main
```

Com esta configuração, sempre que houver um `push` ou `pull_request` na branch `main` do seu projeto, o workflow `build.yaml` deste repositório será executado, realizando as etapas de build e teste.
