# Laravel Setup with Nginx with VPS

## Install nginx web server
```
sudo apt install nginx
```

## Make sure it starts on boot
```
sudo apt enable nginx
```

## Install PHP dependencies
```
sudo apt install php8.3 php8.3-fpm php8.3-cli /
php8.3-mysql php8.3-mbstring php8.3-bcmath php8.3-xml php8.3-sqlite3 php8.3-zip php8.3-gd unzip git mysql-server composer -y
```

## Configure MYSQL
```
sudo mysql

#Create Database
CREATE DATABASE <database_name>;


#Create User
CREATE USER 'lalit'@'localhost' IDENTIFIED BY mysql_native_password BY '1234';
GRANT ALL PRIVILEGES database_name.* TO 'lalit'@'localhost';
FLUSH PRIVILEGES;
```


## Git clone the project
```
cd /var/www

git clone git@github.com:lalitharw/project-name

cd project-name

composer install

cp .env.example .env

php artisan key:generate
```

## Setting Proper File Permission
```
sudo chown -R $USER:www-data /var/www/project-name
sudo chmod -R 775 /var/www/project-name
sudo chmod -R 775 storage
sudo chmod -R 775 boostrap/cache
```

## Create Nginx Virtual Host
#### Go to /etc/nginx/sites-available/project_name
```

server {
    listen 80;
    listen [::]:80;
    server_name 13.51.193.80;
    root /var/www/laravel_load_balancer_test_code/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

## Note
```
In nginx Virtual Host php<version>-fpm.sock will be according to the php version installed on machine or else it will not work
```

## Enabling the virtual Host
Create a symbolic link to enable the site:
```
sudo ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
```

### Testing and Reloading Nginx
```
sudo nginx -t
sudo systemctl reload nginx
```

### To disable site
Remove the symbolic link for sites-enabled
```
sudo rm /etc/nginx/sites-enabled/project-name
```
