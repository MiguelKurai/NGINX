# Instalación Nginx

Para instalar Nginx necesitaremos dos máquinas; la primera para hacer de servidor principal, y la segunda para el balanceo de cargas.

* Instalación de paquetes

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
* Configuración del servidor

Para comprobar el funcionamiento de nuestro servidor, eliminaremos el fichero index predeterminado para para crear uno nuevo con un texto de prueba
```bash
sudo rm -rf /var/www/html/index*
```

Ahora crearemos uno nuevo para añadirle nuestro propio texto
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
sudo nano /etc/nginx/conf.d/balancing.conf
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

* 
