# java-workflow

В репозитории хранятся общие описания для сборок java/kotlin проектов.
Сборка наших проектов бывает разной, в зависимости от типа собираемого проекта:
 - Service - Maven сборка сервиса с деплоем docker image в docker hub
 - Thrift  - собираем thrift протокол и деплоим в maven central
 - Library - собираем библиотеку и деплоим в maven central
 - Swag    - собираем openapi, деплоим в maven central и публикуем в github pages

 Инструменты для сканирования:
 - Semgrep - сканирует по дефолтным правилам и выводит отчет в формате Sarif

Для сканирования проектов - инструмент Semgrep.
Чтобы начать использовать - добавьте в директорию файл `semgrep-scan.yml`
 
Чтобы начать использовать java-workflow в своем репозитории - добавьте в директорию `/.github/workflows/` файлы
`build.yml` и `deploy.yml`, файлов описания workflow не обязательно должно быть два, вы можете самостоятельно описать workflow с использованием `java-workflow`.
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
    uses: valitydev/java-workflow/.github/workflows/maven-service-build.yml@v1
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
    uses: valitydev/java-workflow/.github/workflows/maven-service-deploy.yml@v1
    secrets:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```

### Service
`build.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-service-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-service-deploy.yml@v1
secrets:
  github-token: ${{ secrets.GITHUB_TOKEN }}
  mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```
### Thrift
`build.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-thrift-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-thrift-deploy.yml@v1
secrets:
  server-username: ${{ secrets.OSSRH_USERNAME }}
  server-password: ${{ secrets.OSSRH_TOKEN }}
  deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
  deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
  mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```
`erlang-build-verify.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/erlang-thrift-build.yml@v1
```
### Library
`build.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-library-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-library-deploy.yml@v1
secrets:
  server-username: ${{ secrets.OSSRH_USERNAME }}
  server-password: ${{ secrets.OSSRH_TOKEN }}
  deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
  deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
  mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```
### Swag
`build.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-swag-build.yml@v1
```
`deploy.yml`
```yaml
uses: valitydev/java-workflow/.github/workflows/maven-swag-deploy.yml@v1
secrets:
  server-username: ${{ secrets.OSSRH_USERNAME }}
  server-password: ${{ secrets.OSSRH_TOKEN }}
  deploy-secret-key: ${{ secrets.OSSRH_GPG_SECRET_KEY }}
  deploy-secret-key-password: ${{ secrets.OSSRH_GPG_SECRET_KEY_PASSWORD }}
  github-token: ${{ secrets.GITHUB_TOKEN }}
  mm-webhook-url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
```

### Semgrep scan
`semgrep-scan.yml`
```yaml
name: Run Semgrep

on:
  pull_request:
    branches:
      - '*'

jobs:
  scan:
    uses: valitydev/java-workflow/.github/workflows/semgrep-scan.yml@v3.0.5
    secrets:
      mm-sa-wh-url: ${{ secrets.MATTERMOST_SA_WH_URL}}
```
