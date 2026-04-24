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
    server_name your_domain_or_ip;
    root /var/www/<project_name>/public;
    index index.php index.html index.htm;

    # Security Headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' http: https: data: blob;" always;

    # Gzip Compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied expired no-cache no-store private must-revalidate auth;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss;

    # Handle Laravel Routes
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # PHP Processing
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    # Static Files Caching
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Security: Deny access to hidden files
    location ~ /\. {
        deny all;
    }

    # Security: Deny access to sensitive files
    location ~ /(\.env|\.git|composer\.(json|lock)|package\.json) {
        deny all;
    }

    # Favicon and robots.txt
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
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
