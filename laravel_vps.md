
# Laravel Setup Using VPS

This readme helps in setting up laravel using VPS
### Install Dependencies for Linux
```
sudo apt install -y apache2 mysql-server unzip git composer \
php8.3 php8.3-cli php8.3-common php8.3-mysql php8.3-sqlite3 \
php8.3-xml php8.3-mbstring php8.3-curl php8.3-zip php8.3-bcmath php8.3-intl
```

### Check if apache is Installed
```
sudo systemctl status apache2
```

### Enable apache to start on boot
```
sudo systemctl enable apache2
```

### Setup MySQL
```
sudo mysql_secure_installation
```

### Configure MySQL User and Database
`
sudo mysql
`

#### Change root password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_strong_password';
FLUSH PRIVILEGES;
```

#### Create a database
```
CREATE DATABASE database_name;
```

#### Create a dedicated Laravel Database User
```
CREATE USER 'lalit'@'localhost' IDENTIFIED BY 'your_strong_password';  ---
GRANT ALL PRIVILEGES ON database_name.* TO 'lalit'@'localhost';
FLUSH PRIVILEGES;
```

## Note
#### Do not use the MySQL root user inside your Laravel .env file.
#### Always create a separate database user for the application.


### Clone Laravel Project
```
cd /var/www
sudo git@github.com:lalit/project_folder
cd project_folder
```

### Install Laravel Dependencies

```
composer install --no-dev --no-interaction --prefer-dist
```

### Set Correct File Permissions

Set Ownership:

```
sudo chown -R $USER:www-data /var/www/project_folder
```

Set writeable Permissions for laravel storage and cache directories:

```
sudo chmod -R 775 /var/www/project_folder/storage /var/www/project_folder/bootstrap/cache
```

### If Using sqlite
Create Sqlite file if it does not exists:

```
touch /var/www/project_folder/database/database.sqlite
```


Then set Permissions:

```
sudo chown -R $USER:www-data /var/www/project_folder/database
sudo chmod -R 775 /var/www/project_folder/database
sudo chmod 664 /var/www/project_folder/database/database.sqlite
```


### Configure Apache Virtual Host

Create a new Apache site configuration:

```
sudo nano /etc/apache2/sites-available/project_folder.conf
```

Paste this config:

```
<VirtualHost *:80>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /var/www/project_folder/public

    <Directory /var/www/project_folder/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/project_folder_error.log
    CustomLog ${APACHE_LOG_DIR}/project_folder_access.log combined
</VirtualHost>
````

#### Enable required Apache modules
```
sudo a2enmod rewrite
```

#### Enable the new site
```
sudo a2ensite project_folder.conf
```

#### Disable default apache site
```
sudo a2dissite 000-default.conf
```

#### Test Apache Configuration
```
sudo apache2ctl configtest
```

#### Restart Apache
```
sudo systemctl restart apache2
```

#### Optimize Laravel for Production
```
php artisan optimize
```

