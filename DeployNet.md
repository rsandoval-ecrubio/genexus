# Sincrum - White paper para Deploy de Aplicaciones GeneXus .Net
por [Daniel Monza](https://uy.linkedin.com/in/daniel-monza-62515112)

## Tabla de Contenidos

  1. [Glosario](#glosario)
  1. [Configuración de Internet Information Server (IIS)](#configuración-de-internet-information-server-(iis))
  1. [Deploy de la aplicación GeneXus](#deploy-de-la-aplicación-genexus)


## Glosario

- IIS : Internet Information Services
- RDP : Remote Desktop
- HSTS: HTTP Strict Transport Security

## Configuración de Internet Information Server (IIS)

### Instalar Asp.Net 4.0 o superior y WebSockets

![IIS Features](features.png)

Es utilizado por GeneXus para enviar WebNotifications.

### Instalar módulo Rewrite

El módulo rewrite tiene varios usos, entre ellos sirve para publicar servicios rest en GeneXus.

Para ello, primero se debe instalar Microsoft Web Platform Installer:
https://www.microsoft.com/web/downloads/platform.aspx

### Crear la carpeta donde se publicará la aplicación

Para ello, siempre nos conviene crear una carpeta donde estarán todas nuestras aplicaciones, Ej.: C:\Sites

Luego creamos la carpeta de nuestra app, ej.: c:\Sites\MyApp

### Agregar HSTS Header

Si queremos que nuestro sitio sea accedido solo bajo HTTPS, es encesario definir el encabezado HSTS. De esta forma, los navegadores interpreten que nuestro sitio debe ser accedido de forma segura.

Para ello, accedemos a nuestro sitio dentro del IIS e ingresamos al módulo "HTTP Response Headers" donde agregamos el siguiente encabezado:

Name : Strict-Transport-Security
Value: max-age=10886400; includeSubDomains; preload

[Imagen]

### Aislamiento del exterior
Corroborar que nuestro sitio sea únicametne accesible por los puertos TCP 80 y TCP 443 desde el exterior.

Tener en cuenta si accedemos al servidor por RDP, filtrar el acceso para que solo se pueda ingresar desde la IP pública desde donde estamos accediento. Lo mismo para la base de datos.

**[Volver al inicio](#tabla-de-contenidos)**

## Deploy de la aplicación GeneXus

### Configurar encripción de parámetros

Para configurar ésta propiedad, revisar la siguiente url:
https://wiki.genexus.com/commwiki/servlet/wiki?8068,Encrypt%20URL%20Parameters%20property

### Cambiar clave de encripción por defecto

Se pueden generar nuevas claves con la función [GetEncryptionKey](https://wiki.genexus.com/commwiki/servlet/wiki?8385,GetEncryptionKey+Function) de GeneXus.

En GeneXus 15 esto se puede hacer directamente desde la opción Deploy.

Aquí dejo los links para hacerlo en versiones anteriores a GeneXus 15.

https://www.genexus.com/developers/websac?en,,,29874;;
https://www.genexus.com/developers/websac?en,,,29369;;

### Configurar Servidor de Sesiones


### Definir página por defecto

Como GeneXus generea la aplicación Asp.Net de forma compilada (con dll's), es necesario crear en la carpeta base (donde está el web.config), un archivo con el nombre de nuestro Webpanel por defecto, ej.: home.aspx

Luego en IIS, dentro de nuestro sitio, en el módulo "Default Document", agregamos el hombre de nuestro panel por defecto "home.aspx".

Lo mejor es que solo quede éste, de forma que no haya posibilidad que quede otro panel por defecto, ya sea por un descuido o intrusión.

**[Volver al inicio](#tabla-de-contenidos)**