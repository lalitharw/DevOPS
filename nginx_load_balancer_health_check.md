
# Nginx Health Check Config

### Check max_fail and fail_timeout
```
upstream backend {
    server 10.0.0.1:8000 max_fails=3 fail_timeout=10s;
    server 10.0.0.2:8000 max_fails=3 fail_timeout=10s;
}
```


### Inside Server Block
```
location / {
    proxy_pass http://backend;

    proxy_connect_timeout 3s;
    proxy_read_timeout 10s;
    proxy_send_timeout 10s;
}
```


