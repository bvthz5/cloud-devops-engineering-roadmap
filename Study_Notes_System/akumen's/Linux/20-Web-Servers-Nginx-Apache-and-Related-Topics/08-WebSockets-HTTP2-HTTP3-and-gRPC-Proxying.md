# 8. WebSockets, HTTP/2, HTTP/3 & gRPC Proxying

## 1. WebSocket Proxying

WebSockets require upgrading an initial HTTP/1.1 connection to a persistent bi-directional TCP socket.

```nginx
location /ws/ {
    proxy_pass http://127.0.0.1:8080;

    # Mandatory WebSocket Upgrade Headers
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;

    # Prevent connection timeout for idle sockets
    proxy_read_timeout 86400s;
    proxy_send_timeout 86400s;
}
```

## 2. HTTP/2 & HTTP/3 (QUIC) Enabling

```nginx
server {
    # HTTP/2 support over TLS
    listen 443 ssl http2;
    
    # HTTP/3 (QUIC) support over UDP (Nginx 1.25+)
    listen 443 quic reuseport;
    add_header Alt-Svc 'h3=":443"; ma=86400';

    server_name example.com;
    ...
}
```

## 3. gRPC Reverse Proxying

gRPC relies on HTTP/2 multiplexing and binary framing. Nginx uses `grpc_pass` to proxy gRPC calls.

```nginx
server {
    listen 50051 ssl http2;
    server_name grpc.example.com;

    ssl_certificate /etc/ssl/certs/grpc.crt;
    ssl_certificate_key /etc/ssl/certs/grpc.key;

    location /helloworld.Greeter/ {
        grpc_pass grpc://127.0.0.1:50052;
    }
}
```
