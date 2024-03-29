# 02 - Automatiser nos tests avec une CI

## Objectif

À la fin de cette journée, vous devriez être en mesure de comprendre le concept de CI et d'avoir un repository Github avec des tests automatisés à l'ouverture d'une PR.

## Qu'est-ce que la CI ?

La CI, ou Intégration Continue (Continuous Integration en anglais), est une pratique de développement logiciel qui vise à améliorer la qualité du code en automatisant le processus d'intégration des modifications apportées par plusieurs contributeurs au sein d'un projet. L'objectif principal de la CI est de détecter et de résoudre rapidement les conflits d'intégration, les erreurs de code et les problèmes de compatibilité entre différentes parties du logiciel pour éviter la regression de code.

Le processus de CI implique généralement les étapes suivantes :

1. **Gestion de Versions**: Utilisation d'un système de contrôle de version (comme `Git`) pour suivre les modifications apportées au code.
2. **Automatisation des Builds**: Les outils de CI automatisent la construction du logiciel à partir du code source, garantissant que le processus de build est reproductible et sans erreurs.
3. **Tests Automatisés**: L'exécution automatique de tests unitaires, de tests d'intégration et éventuellement de tests fonctionnels permet de s'assurer que les modifications n'ont pas introduit de régressions.
4. **Rapports de Code Coverage**: L'évaluation de la couverture de code peut également être intégrée pour mesurer la qualité des tests.
5. **Notification des Développeurs**: Les développeurs sont informés rapidement des résultats du processus CI, facilitant la correction immédiate des problèmes détectés.

Les avantages de la CI comprennent **la détection précoce des erreurs**, **l'amélioration de la collaboration** entre les membres de l'équipe, **la réduction** des conflits d'intégration, et la possibilité de livrer des logiciels de manière plus fréquente et fiable. Des outils tels que Jenkins, Travis CI, Github Actions (_nous allons utiliser celui-ci_) et GitLab CI/CD sont souvent utilisés pour mettre en œuvre des pipelines de CI.

## Configuration de son dépôt et son compte Github

- Sécuriser son dépôt Github
  - Protéger la branche principale
  - Obliger un nombre d'approve minimum
  - Interdire le merge sur la branche principale
- Activer les notifications sur son compte

## Implémentation de la CI avec GHA

> ℹ️ Ici nous allons travailler avec le projet `message` présent dans `02-automatiser-nos-tests-avec-une-ci/examples/`

`GHA` fait référence à GitHub Actions, un service de CI/CD fourni par GitHub. 
Les workflows GHA vous permettent d'automatiser des tâches telles que les tests, la construction et le déploiement directement à partir de votre dépôt GitHub. Un workflow GHA est défini dans un fichier YAML spécifique (par exemple, `.github/workflows/nom_du_workflow.yml`) à l'intérieur de votre projet.

### Éléments couramment présents dans un fichier de workflow GitHub Actions

1. **Nom du Workflow**: Un nom descriptif pour le workflow qui apparaîtra sur la page GitHub Actions.
   ```yaml
   name: Mon Workflow de Tests
   ```
   
2. **Événements déclencheurs**: Les événements qui déclenchent l'exécution du workflow. Cela peut être un `push` vers une branche, la création d'une Pull Request, un horaire programmé, etc.
  ```yaml
   on:
    push:
      branches:
        - main
    pull_request:
      branches:
        - feat/*
   ```

3. **Jobs**: Les tâches spécifiques à effectuer, organisées en jobs. Chaque job est exécuté sur une machine virtuelle distincte (pallalèlement ou non).
   ```yaml
   jobs:
      build:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout du code
          uses: actions/checkout@v2
        - name: "Install PHP with extensions"
          uses: shivammathur/setup-php@v2
          with:
            coverage: "xdebug"
            extensions: "intl, mbstring, pdo_sqlite, zip"
            php-version: 8.2
            tools: composer:v2
        - name: "Install dependencies"
          run: composer install --ansi --no-interaction --no-progress
   ```
4. **Steps**: Les étapes individuelles à l'intérieur d'un job. Chaque étape effectue une action spécifique, comme la récupération du code source, l'installation des dépendances, l'exécution de tests, etc. Les steps s'exécute les unes après les autres.
    ```yaml
    steps:
      - name: Checkout du code
        uses: actions/checkout@v2
      - name: "Install PHP with extensions"
        uses: shivammathur/setup-php@v2
        with:
          coverage: "xdebug"
          extensions: "intl, mbstring, pdo_sqlite, zip"
          php-version: 8.2
          tools: composer:v2
      - name: "Install dependencies"
        run: composer install --ansi --no-interaction --no-progress
    ```
5. **Actions**: Des actions sont des unités réutilisables de code qui peuvent être intégrées dans un workflow. Les actions peuvent être créées par la communauté ou par vous-même.
    ```yaml
    - name: "Install PHP with extensions"
      uses: shivammathur/setup-php@v2
      with:
        coverage: "xdebug"
        extensions: "intl, mbstring, pdo_sqlite, zip"
        php-version: 8.2
        tools: composer:v2
    ```
6. **Environnements**: Vous pouvez définir des environnements spécifiques pour les jobs, permettant de regrouper des jobs qui nécessitent le même contexte.
    ```yaml
    jobs:
      deploy:
        runs-on: ubuntu-latest
        environment: production
        steps:
          - name: Déployer sur production
            run: ./deploy.sh
    ```
7. **Matrices (Matrix)**: Permet d'itérer sur différentes configurations, ce qui est utile pour exécuter des tests sur plusieurs versions de langages ou de plates-formes.
    ```yaml
    jobs:
      test:
        name: "${{ matrix.operating-system }} / PHP ${{ matrix.php-version }}"
        runs-on: ${{ matrix.operating-system }}
        continue-on-error: false

        strategy:
          matrix:
              operating-system: ['ubuntu-latest']
              php-version: ['8.2', '8.3']
              include:
                - operating-system: 'macos-latest'
                  php-version: '8.2'
                - operating-system: 'windows-latest'
                  php-version: '8.2'
        
        steps:
            - name: "Checkout code"
              uses: actions/checkout@v3

            - name: "Install PHP with extensions"
              uses: shivammathur/setup-php@v2
              with:
                  coverage: "xdebug"
                  extensions: "intl, mbstring, pdo_sqlite, zip"
                  php-version: ${{ matrix.php-version }}
                  tools: composer:v2

            - name: "Print php version"
              run: "php --version"
    ```

## Exercices

Reprendre les exercices de la veille et ajoutez y les workflows de CI qui exécutent les tests.

- Pour les développeurs PHP
  - Utiliser en plus de PHPUnit des outils tels que : [PHPStan](https://phpstan.org/), [PHP_codeSniffer](https://github.com/squizlabs/PHP_CodeSniffer/wiki)

### Aller plus loin

- Essayez de comprendre le contenu du fichier `02-automatiser-nos-tests-avec-une-ci/helpers/coverage-checker.php` puis ajouter une step dans votre CI de tests pour que celle-ci fail si le coverage est inférieur à 80%. 
  - Pour les développeurs node: Vos workflows de CI doivent être en erreur si le pourcentage de coverage est inférieurà 80% (le faire avec `Jest`)
- Faire en sorte que chaque dossier `exercice-*` s'exécute en parrallèle (voir `strategy > matrix`)
