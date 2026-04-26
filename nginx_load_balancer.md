# Nginx Load Balancer

### Use the Below code the configure load Balancer
```
 upstream laravel_lb {
        server 13.60.11.100:80;
        server 13.51.193.80:80;

    }

    server {
        listen 80;
        server_name 13.49.76.29;

        location / {
            proxy_pass http://laravel_lb;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```

### Note

#### APP_KEY Must be same or all server or it will break sessions
