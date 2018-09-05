# Índice

- [Índice](#%C3%ADndice)
- [Introducción](#introducci%C3%B3n)
- [Asignando DBO a una aplicación](#asignando-dbo-a-una-aplicaci%C3%B3n)
- [Instalación de PostgreSQL](#instalaci%C3%B3n-de-postgresql)
- [Conectando Aplicación a DBO](#conectando-aplicaci%C3%B3n-a-dbo)
    - [Credenciales](#credenciales)
    - [Conexiones](#conexiones)
        - [Python](#python)
        - [Java](#java)

# Introducción

El hoster *Heroku* trabaja con diferentes sistemas de Bases de Datos (*SQL*) de los cuáles el único gratuito es PostgreSQL.  
Este sistema open-source cuenta con gran cantidad de características que lo hacen muy útil y poderoso, entre las que están:

* Sistema de interrelación de tablas.
* Subconsultas avanzadas.
* Funciones (Procedimientos almacenados).
* Manejo de usuarios según los permisos.

Entre otros añadidos también propios de otros sistemas SQL.

# Asignando DBO a una aplicación

*Heroku* tiene un sistema de Addons disponibles para ser asignados a una aplicación.  
Podemos verlos todos escribiendo en la consola `heroku addons:services`, pero en este caso nos vamos a centrar en uno que se llama **heroku-postgresql**, debido a que el sistema de DBO PostgreSQL es el único con plan gratuito en Heroku.

![addons-heroku](https://i.imgur.com/6Mjyy0p.png)

Debido a que *Heorku* es un sistema de Hosting que va más allá del desarrollo por estudios, hay varios planes de pago y algunos gratuitos, podemos ver todos los planes de un Addon escribiendo en consola `heroku addons:plans nombre-addon`, por ejemplo; si escribimos en consola `heroku addons:plans heroku-postgresql` obtendremos los planes de pago de *Heroku* para PostgreSQL.

![planes-pago-postgresql](https://i.imgur.com/JorgtGQ.png)

>Esta guía abarcará el plan gratuito `Hobby Dev` para hacerla más accesible a todos los públicos.

Para añadir dicho Addon a nuestra aplicación bastará con usar el comando `heroku addons:create` seguido del nombre del addon más el plan de servicio y, obviamente, la bandera que apuntará a nuestra aplicación.

Por ejemplo, si quisiéramos añadir `heroku-postgresql` de tipo `hobby-dev` a nuestra aplicación `scripthubteam-app` recurriríamos a la siguiente línea de comando:  
`heroku addons:create heroku-postgresql:hobby-dev -a scripthubteam-app`.

Deberíamos obtener un output parecido al siguiente:

![output-heroku-postgres](https://i.imgur.com/4TY0bxf.png)

Con esto ya habríamos asignado una Base de Datos a una aplicación.

# Instalación de PostgreSQL

![postgresql-logo](https://www.redeszone.net/app/uploads/2016/02/postgresql-logo.png?x=634&y=309)

[PostgreSQL](https://www.postgresql.org/) es un sistema de Bases de Datos Relacionales basado en el código abierto.  
Su instalación irá guiada por un instalador que obtendremos de la [página oficial de descarga](https://www.postgresql.org/download/), por lo que bastará con hacer los ajustes pertinentes e instalar.

Para comprobar la correcta instalación de PostgreSQL accederemos a la consola y escribiremos el siguiente comando `psql --version`, obteniendo una salida parecida a la siguiente:

![psql-version](https://i.imgur.com/BkzU1di.png)

>Necesitamos PostgreSQL instalado localmente ya que, de otra manera, se nos haría imposible su conexión con Heroku a través del comando `heroku psql -a <aplicacion>`.

Para realizar ejecuciones SQL a través de la propia consola podemos recurrir al comando `heroku psql -a nombre-app`.  
Por ejemplo, podemos usar `heroku psql -a scripthubteam-app` para establecer conexión con la consola PostgreSQL asignada a la aplicación **scripthubteam-app**.

![conectando-postgres](https://i.imgur.com/WgtyC5z.png)

# Conectando Aplicación a DBO

Para establecer una conexión SQL en nuestra aplicación primero hay que entender unos pocos conceptos, como las credenciales o la ruta en la que está alojada nuestra DBO.

Una conexión SQL se compone principalmente de cinco elementos:

- **Nombre** (*dbname*) de la Base de Datos a la que nos vamos a conectar.
- **Servidor** (*host*) donde está alojada nuestra Base de Datos.
- **Usuario** (*username*) con el que se realizará la conexión.
- **Puerto** (*port*) en el que se establecerá la conexión.
- **Contraseña** (*password*) requerida para la conexión.

Todos estos datos se denominan credenciales y son cruciales para usar la base de datos asignada a nuestra apliación.

## Credenciales

Podemos ver las credenciales de la Base de Datos asignada a nuestra aplicación escribiendo el comando `heroku pg:credentials:url -a nombre-app`, es decir, usaríamos `heroku pg:credentuals:url -a scripthubteam-app` para ver las credenciales de la aplicación **scripthubteam-app**.

Obteniendo un salida parecido a la siguiente, pero con diferente valores:

![credentials-output](https://i.imgur.com/egpwrtu.png)

Si nos fijamos en las palabras remarcadas en rojo, podemos ver que son los credenciales básicos mencionados anteriormente y, si nos fijamos en el enlace posterior, veremos que son los mismos datos pero expuestos en una URL completa (*postgres://...*).

>Esta URL se denomina URI y se usa para simplificar la conexión a una base de datos, dado que en lugar de exponer dato a dato, simplemente se da dicho enlace.

>`sslmode='require'` hace referencia a un sistema de encriptado muy usado cuando las conexiones a DBO se realizan de manera externa al servidor.

## Conexiones

### Python

### Java