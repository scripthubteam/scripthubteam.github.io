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

Como se mencionó antes, la conexión a la DBO de nuestra aplicación puede ser establecida por una URI.  
Heroku almacena dicha URI como una variable de entorno llamada *DATABASE_URL*, por lo que solo se puede acceder a ella desde dentro del entorno Heroku.

La manera de acceder a dicha variable de entorno varía según el lenguaje.

### Python

Para usar PostgreSQL en Python necesitaremos la librería **psycopg2**, que podremos instalar con el comando `pip install psycopg2-binary` o `pipenv install psycopg2-binary` si estamos en un *Virtual Enviorment*.

![output-psycopg2](https://i.imgur.com/NyX1ekL.png)

Para establecer una conexión con nuestra Base de Datos bastará usar la librería `os` de Python para obtener la variable de entorno *DATABASE_URL* y la libreria `psycopg2` para establecer la conexión.

```python
import os
import psycopg2

# Obtenemos variable de entorno
DATABASE_URL = os.environ['DATABASE_URL']

psycopg2.connect(DATABASE_URL, sslmode='require')
```

>También podemos establecer dicha conexión señalando dato a dato cada uno de los credenciales que obtengamos con `heroku pg:credentials:url -a nombre-app`.

```python
import psycopg2

psycopg2.connect(dbname='d4kcqemt1v7ebm'
                 user='foulctbnasfphk'
                 host='ec2-54-217-245-53.eu-west-1.compute.amazonaws.com'
                 password='c8e92783d05010830805d0488daaeee164f6379839975afa3e5f8583f93b88f8'
                 sslmode='require')
```

>Obviamente estos credenciales son únicos para cada aplicación y no tienen que coincidir con los que tenga cada usuario.

### Java

Para usar PostgreSQL en Java necesitaremos la dependencia `postgresql`, que puede ser fácilmente añadida a nuestro proyecto Maven cambiando 