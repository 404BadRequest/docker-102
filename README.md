# Laboratorio Docker Compose

Este laboratorio se basa en el mismo ejercicio que corrimos en Replit con Python y Flask.

## Preparación

Haz un fork de este repo.

Clona el fork en tu pc.

## Paso 1

Ejecuta `docker init`.

Selecciona Python como el `application platform`.

Cuando aparezca la pregunta `What is the command to run your app?`, escribe `python main.py`.

Abre el archivo `compose.yaml`, en la línea 16, debajo de la sección `ports` agrega estas líneas:

```
    enviroment:
      - PORT
      - HOST
      - DB
      - DB_USERNAME
      - DB_PASSWORD
```

Asegurate que `ports` y `enviroment` estén alineados en la misma columna.

Revisa el archivo `.env`, configura las variables con los valores que usaste en el ejercicio en Replit (es decir, usa las credenciales de ElephantSQL).

Levanta la aplicación con el siguiente comando:

```
docker compose up --build
```

Revisa que todo está bien navegando a la dirección `localhost:8000`.

### Preguntas

¿Qué pasa si cambias el valor de la variable PORT?

- La aplicación no es accesible a través del nuevo puerto.

¿Qué cambios debes hacer para cambier el port a 8080?

- Se debe cambiar el archivo .env indicando el puerto 8080 en la variable PORT.
- Se debe cambiar el archivo compose.yaml en la sección del ports

## Paso 2

Deten `docker compose` presionando `control-c`.

Elimina los comentarios a partir de la línea 28.

Asegurate de crear un archivo en la carpeta `db` llamado `password.txt`, coloca en este una clave cualquiera.

Modifica el archivo `.env` para que se pueda conectar al servicio `db`.

Tips: 
  - El valor para `HOST` es `db`
  - los valores que necesitan están declarados en la definición del servicio, debes hacer el "match" de las variables allí definidas con las que necesitas.

Esta vez ejecuta el comando `docker compose` de este modo:

```
docker compose up -d --build 
```

La opción `-d` permite dejar ejecutando los servicios y libera la consola.

### Actividades

Ejecuta `docker ps`. 

¿Qué obtienes?.

```

CONTAINER ID   IMAGE               COMMAND                  CREATED              STATUS                        PORTS                              NAMES
aab2b554e683   docker-102-server   "/bin/sh -c 'python …"   About a minute ago   Up 58 seconds                 8000/tcp, 0.0.0.0:8080->8080/tcp   docker-102-server-1
9d82f033313e   postgres            "docker-entrypoint.s…"   About a minute ago   Up About a minute (healthy)   5432/tcp                           docker-102-db-1

```

Ejecuta `docker images`. 

¿Cuál es el tamaño de la imagen del servidor flask?
```
docker-102-server    latest  59db602b0e9f   46 minutes ago   209MB
```
¿Cuál es el tamaño de la imagen postgres?
```
postgres             latest  a80d0c1b119c   4 minutes ago    613MB
```
¿Cuándo fueron creadas cada una de las imágenes?
```
postgres                           latest           a80d0c1b119c   4 minutes ago    613MB
docker-102-server                  latest           59db602b0e9f   46 minutes ago   209MB
```
Ahora ejecuta `docker compose logs -f`, esto te permite revisar el log de los contenedores.

Si navegas hacia la aplicación (http://localhost:8000/) se produce un error, revisa el log.

¿Cuál es la causa del error?

El error se debe a que la base de datos no tiene una tabla llamada books
```

docker-102-server-1  |   File "/app/main.py", line 24, in index
docker-102-server-1  |     cur.execute("SELECT * FROM books;")
docker-102-server-1  | psycopg2.errors.UndefinedTable: relation "books" does not exist
docker-102-server-1  | LINE 1: SELECT * FROM books;

```
Cierra el log presionando `control-c`, luego detén la aplicación con este comando:

```
docker compose down
```

