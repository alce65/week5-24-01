# SonarCloud

Configuración para el uso de monorepos de Angular en SonarCloud.
Cada uno de ellos se definirá como un proyecto independiente en SonarCloud.

- Se añade el análisis del proyecto configurando el repositorio Github en SonarCloud como monorepo

- Analyze new project
- Set up a monorepo
- Choose an organization from GitHub
- Select repository (monorepo)
  (Puede ser uno repo ya enlazado, para transformarlo en momo repo)

  Add new project
  - Project key: angular_monorepo_project
    - e.g. alce65_week4.ng_demo1
  - Display Name: angular_monorepo_project

- Analyze a project with a GitHub Action

- SONAR_TOKEN
- df9cf9e8ee9b5b1efbdfd2f02fb34e3b08f07c27

- Se añade el secret SONAR_TOKEN_{{PROJECT}} en el repositorio de GitHub

- Se incorporan localmente las Github Actions .github/workflows/sonar.project.yml
  - Testing: referencia a la carpeta como "working-directory:"
  - Sonar: referencia al Secret y a la carpeta como "with: projectBaseDir:"

```yml

name: SonarCloud Analysis

on:
  push:
    branches: ["main"]
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:

permissions:
  pull-requests: read # allows SonarCloud to decorate PRs with analysis results

jobs:
  SonarCloud:
    name: SonarCloud
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Shallow clones should be disabled for a better relevancy of analysis
      - name: Install modules
        run: npm ci
      - name: Testing coverage
        run: npm run test:prod:demo #Change for a valid npm script
        working-directory: projects/demo1 # The directory containing the source code
      - name: SonarCloud Scan
        uses: SonarSource/sonarcloud-github-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # Needed to get PR information, if any
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN_DEMO1 }} # Generate a token on Sonarcloud.io, add it to the secrets of this repo with the name SONAR_TOKEN (Settings > Secrets > Actions > add new repository secret)
        with:
          projectBaseDir: projects/demo1 # The directory containing the source code
```


Se añade un package.json a nivel del proyecto, con el script test:prod

```json
"test:prod": "ng test --code-coverage --no-watch --browsers=ChromeHeadless"
```

- Se añade el fichero de configuración de Sonar a nivel del proyecto

```properties
sonar.projectKey=alce65_week4.ng_demo1
sonar.organization=alce65

# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
sonar.sources=src
sonar.test.inclusions=src/**/*.spec.*
sonar.javascript.lcov.reportPaths=../../coverage/demo1/lcov.info
sonar.coverage.exclusions=src/**/*.spec.*, src/**/*.module.*,

```
