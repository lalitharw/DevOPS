
# Zero DownTime Deployment Load Balancer

```
name: CI/CD Yaml

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Disable Server 2 from LB
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.LB_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            sudo sed -i "s|server[[:space:]]*${{ secrets.SERVER_2_PRIVATE_HOST }}:80.*;|server ${{ secrets.SERVER_2_PRIVATE_HOST }}:80 down;|g" /etc/nginx/sites-available/load_balancer
            sudo nginx -t
            sudo systemctl reload nginx
            grep "${{ secrets.SERVER_2_PRIVATE_HOST }}" /etc/nginx/sites-available/load_balancer

      - name: Deploy to Server 2
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_2_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/laravel_authentication
            git pull origin main
            composer install --no-dev --no-interaction --prefer-dist --optimize-autoloader
            php artisan migrate --force
            php artisan optimize

      - name: Enable Server 2 in LB
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.LB_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            sudo sed -i "s|server[[:space:]]*${{ secrets.SERVER_2_PRIVATE_HOST }}:80.*;|server ${{ secrets.SERVER_2_PRIVATE_HOST }}:80 max_fails=3 fail_timeout=10s;|g" /etc/nginx/sites-available/load_balancer
            sudo nginx -t
            sudo systemctl reload nginx
            grep "${{ secrets.SERVER_2_PRIVATE_HOST }}" /etc/nginx/sites-available/load_balancer

      - name: Disable Server 1 from LB
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.LB_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            sudo sed -i "s|server[[:space:]]*${{ secrets.SERVER_1_PRIVATE_HOST }}:80.*;|server ${{ secrets.SERVER_1_PRIVATE_HOST }}:80 down;|g" /etc/nginx/sites-available/load_balancer
            sudo nginx -t
            sudo systemctl reload nginx
            grep "${{ secrets.SERVER_1_PRIVATE_HOST }}" /etc/nginx/sites-available/load_balancer

      - name: Deploy to Server 1
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_1_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/laravel_authentication
            git pull origin main
            composer install --no-dev --no-interaction --prefer-dist --optimize-autoloader
            php artisan migrate --force
            php artisan optimize

      - name: Enable Server 1 in LB
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.LB_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            sudo sed -i "s|server[[:space:]]*${{ secrets.SERVER_1_PRIVATE_HOST }}:80.*;|server ${{ secrets.SERVER_1_PRIVATE_HOST }}:80 max_fails=3 fail_timeout=10s;|g" /etc/nginx/sites-available/load_balancer
            sudo nginx -t
            sudo systemctl reload nginx
            grep "${{ secrets.SERVER_1_PRIVATE_HOST }}" /etc/nginx/sites-available/load_balancer
```

