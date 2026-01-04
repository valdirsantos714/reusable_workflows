# Reusable Workflows

Este repositório contém **workflows reutilizáveis do GitHub Actions** para Node.js e Java, projetados para:

* Facilitar integração com outros projetos
* Garantir padronização e qualidade de build
* Reduzir esforço de configuração em novos repositórios
* Implementar **versionamento semântico automático**
* Permitir **publicação de imagens Docker** de forma opcional e segura

---

## Como Usar Workflows Reutilizáveis

Para usar os workflows deste repositório em outro projeto, você pode chamá-los usando:

```text
valdirsantos714/reusable_workflows/.github/workflows/filename@nome-da-branch

```

> Todos os workflows suportam **inputs configuráveis** e **secrets do repositório caller**, garantindo segurança e flexibilidade. O `GITHUB_TOKEN` é usado automaticamente para versionamento semântico.

---

## Workflows Disponíveis (Motores de Build)

### 1️⃣ Build Node.js (`build-nodejs.yaml`)

* Suporta **Node.js com a versão customizável**
* Executa **checkout, instalação, testes e build**
* Opcional: **push Docker Hub** com validação de secrets
* Gera tags **SemVer automáticas** por branch

**Inputs principais:**

| Input          | Descrição                                               |
| -------------- | ------------------------------------------------------- |
| `working-directory` | Diretório de trabalho                                   |
| `node-version` | Versão do Node.js                                       |
| `push-image`   | Publicar imagem Docker (true/false)                     |
| `docker-image-name`   | Nome da imagem Docker                                   |

**Exemplo de uso:**

```yaml
jobs:
  build:
    uses: valdirsantos714/reusable_workflows/.github/workflows/build-nodejs.yaml@main
    with:
      working-directory: 'frontend'
      node-version: '20'
      push-image: true
      docker-image-name: valdir/minha-app
    secrets:
      DOCKERHUB_USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}
      DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}
```

---

### 2️⃣ Build Java (`build-java.yaml`)

* Suporta **Java com a versão e distribuição customizáveis**
* Suporta cache Maven/Gradle
* Executa **checkout, testes e build**
* Opcional: **push Docker Hub** com validação de secrets
* Gera tags **SemVer automáticas** por branch

**Inputs principais:**

| Input               | Descrição                                  |
| ------------------- | ------------------------------------------ |
| `working-directory` | Diretório de trabalho                      |
| `java-version`      | Versão do Java                             |
| `distribution`      | Distribuição do Java (temurin, zulu, etc.) |
| `cache`             | Cache de build (`maven` ou `gradle`)       |
| `push-image`       | Publicar imagem Docker (true/false)        |
| `docker-image-name` | Nome da imagem Docker                      |

**Exemplo de uso:**

```yaml
jobs:
  build:
    uses: valdirsantos714/reusable_workflows/.github/workflows/build-java.yaml@main
    with:
      working-directory: 'backend'
      java-version: '21'
      push-image: true
      docker-image-name: valdir/minha-api
    secrets:
      DOCKERHUB_USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}
      DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}
```

---

## Observações Importantes

* **Versionamento Semântico Automático**:

  * `main` → release estável (`v1.2.3`)
  * `develop` → pre-release (`v1.3.0-dev.1`)
  * `release/*` → release candidate (`v1.3.0-rc.1`)
  * `hotfix/*` → patch imediato (`v1.2.4`)

* **Docker opcional**:

  * Secrets obrigatórios (`DOCKERHUB_USERNAME` e `DOCKERHUB_TOKEN`) são validados antes do push.
  * Sem secrets → workflow falha com mensagem clara.

* **GITHUB_TOKEN**:

  * Usado automaticamente para criação de tags e versionamento semântico.
  * Não precisa ser configurado manualmente, mesmo em workflows reutilizáveis cross-repo.

* **Cross-repo**:

  * Workflows podem ser usados em qualquer repositório ou organização.
  * Sempre executam no contexto do **repositório que chama o workflow**, garantindo segurança e isolamento.

---

Se quiser, posso criar **uma versão ainda mais visual do README**, com **diagrama do fluxo completo** mostrando Node.js/Java → SemVer → Docker → Argo CD, que ajuda muito novos usuários a entenderem a arquitetura.

Quer que eu faça isso?
