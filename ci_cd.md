
# CI/CD Pipeline for Testing and Deploying Code on Production

### Create a folder in root of your laravel application
.github/workflows/ci-cd.yaml

```
name: CI/CD Pipeline

on:
    push:
        branches:
            - main
jobs:
    testing:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout Code
              uses: actions/checkout@v2
            - name: Setup PHP
              uses: shivammathur/setup-php@v2
              with:
                php-version:"8.2"
                extensions: mbstring, xml, curl, pdo_sqlite, sqlite
            - name: copy .env
              run: cp .env.example .env
            - name: Install Dependencies
              run: composer install --no-interaction --prefer-dist --optimize-autoloader
            - name: Generate App Key
              run: php artisan key:Generate
            - name: Run migration
              run: php artisan migrate --force
            - name: Run tests
              run: php artisan test

    production:
        needs: testing
        runs-on: ubuntu-latest
        ....
```
