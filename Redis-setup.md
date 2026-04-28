# Redis Setup

### Create new server for Redis
```
sudo apt install redis-server
```

### Configure Redis
```
1. Protected Mode ON
Redis Protected Mode is basically a safety feature to stop random people on the internet from connecting to your Redis server and messing with your data.

2. Bind to Specific IP
bind 127.0.0.1 your_server_private_ip

3. requirepass yourStrongPassword
```

