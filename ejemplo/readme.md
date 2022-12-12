
## Example Deploy

referencia [Cómo configurar Django con Postgres, Nginx y Gunicorn en Ubuntu](https://www.digitalocean.com/community/tutorials/como-configurar-django-con-postgres-nginx-y-gunicorn-en-ubuntu-18-04-es)

## 1

```bash
sudo nano /etc/systemd/system/example.socket
```

- Ingresar lo siguiente :

```bash
[Unit]
Description=example socket

[Socket]
ListenStream=/run/example.sock

[Install]
WantedBy=sockets.target
```

---
## 2

```bash
sudo nano /etc/systemd/system/example.service
```

- Ingresar lo siguiente :


```bash
[Unit]
Description=example daemon
Requires=example.socket
After=network.target

[Service]
User=user
Group=user
WorkingDirectory=/home/user/proyect/proyect.api
ExecStart=/home/user/proyect/env/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/example.sock \
          example1.wsgi:application

[Install]
WantedBy=multi-user.target
```

---

## 3

```bash
server {
    listen 80;
    server_name 161.97.162.17;
    charset utf-8;

    location /static/ {
        root /home/user/proyect/proyect.api;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/example/p2p.sock;
    }
}
```

staticfiles/
