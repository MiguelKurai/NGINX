# Instalación Nginx

Para instalar Nginx necesitaremos dos máquinas; la primera para hacer de servidor principal, y la segunda para el balanceo de cargas.

## Paso 1: Instalación de paquetes

Primero, debemos actualizar los paquetes de nuestros servidores
```bash
sudo apt update
sudo apt upgrade -y
```

Una vez tengamos los paquetes actualizados, procederemos a instalar Nginx en ambos servidores
```bash
sudo apt install nginx -y
```

Ya tenemos nuestro servidor instalado, ahora debemos activar el servicio para que empiece a funcionar.
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Para comprobar si nuestro servidor está enfuncionamiento usaremos el siguiente comando:
```bash
sudo systemctl status nginx
```
## Paso 2: Configuración del servidor

Lo primero será editar el fichero /etc/nginx/nginx.conf:
```bash
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;
    root         /usr/share/nginx/html;
}
```

Para comprobar el correcto funcionamiento de nuestro servidor, eliminaremos el fichero index predeterminado para para crear uno nuevo con un texto de prueba(puede realizarle una copia previamente).
```bash
sudo rm -rf /var/www/html/index*
```

Ahora crearemos uno nuevo para añadirle nuestro propio texto.
```bash
sudo nano /var/www/html/index.html
```

Introducimos el texto

```bash
<html>
<title>nginx</title>
<body>
Mensaje de prueba
</body>
</html>
```

A continuación, configuramos nuestro sitio web en ambos servidores

```bash
sudo nano /etc/nginx/sites-available/default
```

Le introducimos la siguiente configuración

```bash
upstream backend {
    server 35.174.168.158; # Ip de nuestro servidor principal
    server 44.212.60.70;  # Ip de nuestro servidor secundario
}

server {
    listen      80;
    server_name mkurai.ddns.net; # Dominio registrado con la IP de nuestro servidor de balanceo de carga

    location / {
        proxy_redirect      off;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
        proxy_pass http://backend;
    }
}
```

Por último, le permitiremos el tráfico HTTPS en el firewall con los siguientes comandos:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

## Paso 3: Configuración del localhost

Ahora configuraremos nuestro fichero /etc/hosts para añadirle nuestro servidor como localhost y ya prodremos acceder a nuestro servidor desde dentro y desde fuera.
```bash
127.0.0.1 mkurai.ddns.net
```
