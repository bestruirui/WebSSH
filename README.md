## WebSSH

Fork [huashengdun/webssh](https://github.com/huashengdun/webssh.git)

### Preview

![image](https://user-images.githubusercontent.com/89796393/213195808-baa64711-bf1d-462a-8391-7187134c073a.png)

###  Docker
```
 docker run -d -p 8888:8888  bestrui/WebSSH
```

### Nginx
```nginx
server {
    listen                  443 ssl http2;
    listen                  [::]:443 ssl http2;
    server_name             sample.com;

    # SSL
    ssl_certificate         /nginx/bestrui.crt;
    ssl_certificate_key     /nginx/bestrui.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
    ssl_prefer_server_ciphers on;
    client_max_body_size 20m; 

    # reverse proxy
    location / {
        proxy_pass        http://IP:8888/;
        proxy_http_version 1.1;
        proxy_read_timeout 300;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Real-PORT $remote_port;
    }
    location /wss/  {   
        proxy_pass http://IP:8888/;        #通过配置端口指向部署websocker的项目
        proxy_http_version 1.1;
        proxy_read_timeout 300;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Real-PORT $remote_port;     }
}

# HTTP redirect
server {
    listen      80;
    listen      [::]:80;
    server_name sample.com;

    location / {
        return 301 https://sample.com$request_uri;
    }
}

```






