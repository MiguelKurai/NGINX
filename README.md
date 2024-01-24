# NGINX

## Introducción

NGINX es un servidor web de código abierto que, desde su éxito inicial como servidor web, ahora también es usado como proxy inverso, cache de HTTP, y balanceador de carga.

Nginx fue creado originalmente por Igor Sysoev, y se lanzó al público en octubre de 2004. Inicialmente, el software era una respuesta al problema C10K, que se refiere al problema de rendimiento de manejar 10.000 conexiones concurrentes.

Debido a que originalmente nación con el fin de optimizar el rendimiento de las páginas web, Nginx a menudo supera a otros populares servidores web en pruebas de rendimiento (Benchmarks), especialmente en situaciones con contenido estático y/o un alto número de solicitudes concurrentes.

Nginx está diseñado para ofrecer un bajo uso de memoria y alta concurrencia. En lugar de crear nuevos procesos para cada solicitud web, Nginx usa un enfoque asincrónico basado en eventos donde las solicitudes se manejan en un solo hilo.

Con Nginx, un proceso maestro puede controlar múltiples procesos de trabajo. El proceso maestro mantiene los procesos de trabajo, y son estos los que hacen el procesamiento real.


1. [Comparativa con Apache](diferencias.md)

2. [Instalación]()

3. [Casos Prácticos]()

4. Referencias
  * [Kinsta](https://kinsta.com/es/base-de-conocimiento/que-es-nginx/)
