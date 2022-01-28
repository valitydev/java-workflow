# base-workflow

В репозитории хранятся общие описания для сборок java/kotlin проектов.
Сборка наших проектов бывает разной, в зависимости от типа собираемого проекта:
 - Service - Maven сборка сервиса с деплоем docker image в docker hub
 - Thrift  - собираем thrift протокол и деплоим в maven central
 - Library - собираем библиотеку и деплоим в maven central
 - Swag    - собираем openapi, деплоим в maven central и публикуем в github pages
 
Чтобы начать использовать base-workflow в своем репозитории - добавьте в директорию `/.github/workflows/` файлы
`build.yml` и `deploy.yml`, файлов описания workflow не обязательно должно быть два, вы можете самостоятельно описать workflow с использованием `base-workflow`.
Ниже приведен пример для `service` типа проекта. Аналогично и для других типов, изменяется только название файла и передаваемые параметры.

`build.yml`
```yaml
name: Build Maven Artifact

on:
  pull_request:
    branches:
      - '*'

jobs:
  build:
    uses: valitydev/base-workflow/.github/workflows/maven-service-build.yml@v1
```
`deploy.yml`
```yaml
name: Deploy Docker Image

on:
  push:
    branches:
      - 'master'

jobs:
  build-and-deploy:
    uses: valitydev/base-workflow/.github/workflows/maven-service-deploy.yml@v1
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```

### Service
`build.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-service-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-service-deploy.yml@v1
  secrets:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```
### Thrift
`build.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-thrift-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-thrift-deploy.yml@v1
  secrets:
    server-username: ${{ secrets.OSSRH_USERNAME }}
    server-password: ${{ secrets.OSSRH_TOKEN }}
    deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
    deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
```
`erlang-build-verify.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/erlang-thrift-build.yml@v1
```
### Library
`build.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-library-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-library-deploy.yml@v1
  secrets:
    server-username: ${{ secrets.OSSRH_USERNAME }}
    server-password: ${{ secrets.OSSRH_TOKEN }}
    deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
    deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
```
### Swag
`build.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-swag-build.yml@1
```
`deploy.yml`
```yaml
uses: valitydev/base-workflow/.github/workflows/maven-swag-deploy.yml@v1
  secrets:
    server-username: ${{ secrets.OSSRH_USERNAME }}
    server-password: ${{ secrets.OSSRH_TOKEN }}
    deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
    deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
    github-token: ${{ secrets.GITHUB_TOKEN }}
```