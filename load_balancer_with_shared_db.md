# Load Balancer with Shared DB

### Setup MYSQL in Server 1
Install mysql and create database and users
```
sudo apt install mysql-Server

CREATE DATABASE laravel;

CREATE USER 'lalit'@'%' IDENTIFIED BY '1234';

GRANT ALL PRIVILEGES ON laravel.* TO 'lalit'@'%';
FLUSH PRIVILEGES;
```

### Update MySQL Config
```
sudo micro /etc/mysql/mysql.conf.d/mysqld.cnf
```

Change:
```
bind-address = 0.0.0.0
```

Then:
```
sudo systemctl restart mysql
```

Then in .env Update
```
DB_HOST=PRIVATE_IP_OF_DB_SERVER
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=your_password
```


### We need to configure Security Group
```
1. For Load Balancer
80: 0.0.0.0/0 -> where all users can access it
22: 0.0.0.0/0 -> For testing purpose any user can ssh into it


2. For App Server
80/443: Load Balancer SG Group -> only load balancer can access it cannot be accessed directly

3. For DB Server
3306: App Server SG -> only App Server SG can access it cannot be accessed directly from internet
```


### Note
```
In Load Balancer use private IP address of Servers
```
