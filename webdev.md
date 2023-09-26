# Web development

## Tailwind

## Node.js

- `__dirname`. The absolute path of the _currently executing file_.
  - Useful when you want to know the **immediate containing folder.**
    - Making new directories
    - Pointing to directories
    - Adding files to a directory
- `process.cwd()`. The absolute path of _where you started the Node.js process_.


## NGINX

### Setup website with HTTPS

```conf
# nginx.conf

server {
  listen 443 ssl;
  server_name <++>;

  ssl on;
  ssl_certificate /path/to/public-cert-chain/fullchain.pem;
  ssl_certificate_key /path/to/private-key/privkey.pem;

  location / {
    proxy_pass http://<++>:<++>;
  }
}

server {    # redirect http to https
  listen 80;
  server_name <++>;
  return 301 https://$host$request_uri;
}
```