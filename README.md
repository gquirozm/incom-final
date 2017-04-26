## Inventario vs Competencia
Web Scraping para análisis de competencia en tiempo real. 

![jabbr.net](http://webdata-scraping.com/media/2013/11/web-scraping-services.png)


### Alcance del producto
Objetivo: extraer, validar, mapear y comparar datos (fuzzy match) entre **datos maestros** vs la **competencia** que ayuden a la toma de decisiones para desarrollar nuevos negocios o mejorar los ya existentes.

Las variables objetivo son:

* Nombre de Hotel
* Dirección
* Latitud
* Longitud
* Descripción
* Tarifa

Consideraciones

1. El análisis de las paginas web es complejo ya que se enfrenta a estructuras dinámicas para la automatización, el tiempo estimado es para extracción e ingesta de 6 campos (nombre, dirección, longitud, latitud, descripción, precio), entre menos campos el esfuerzo puedes ser menor.

2. Se corre el riesgo que las paginas cambien de forma dinámica su estructura por lo cual se debe tomar en cuanta mantenimiento correctivo para la operación del sistema.

3. Los componentes propuestos son open source del ecosistema Big Data, mismo que no generar costo por licenciamiento, además que la plataforma puede ser escalable de forma horizontal y flexible en el tiempo.

4. Se propone el uso de Spark para el escalamiento de poder de procesamiento y búsqueda (fuzzy match).

5. Se propone el uso de almacenamiento en base de datos relacional postgreSQL para guardar en un modelo básico la trazabilidad de los datos.

6. Se propone la explotación de indicadores a través de las herramientas de visualización como Tableau.

7. Se considera el uso de contenedores de Docker para la entrega del producto como un servicio.

8. Se recomienda desplegar el producto en la nube (Microsoft Azure) por lo cual considerar un costo de uso de procesamiento y almacenamiento en la nube.

##DIAGRAMA DE COMPONENTES

[Web Data Discovery (WDD)](images/InventariovsCompetencia.png)

##PIPE LINE

[Web Data Discovery (WDD)](images/WebDataDiscovery.png)

### DEFINICIÓN DEL AMBIENTE



Donde tenemos la siguiente estructura:

1. Vamos a utilizar Docker compose para instalar microservicios
__rw-rw-r-- 1 radianv radianv  416 jun 26 23:22 docker-compose.yml__


**ver archivo** [docker-compose](docker-compose.yml) 

```
qa-incom-data:
  build: docker-images/data
  volumes:
    - /data
qa-incom-scrapy:
  build: docker-images/scrapy
  ports:
    - "5432:5432"
  volumes_from:
    - qa-incom-data
  env_file: .env

qa-incom-jupyter:
  build: docker-images/jupyter
  hostname: qa-incom-jupyter

  links:
    - qa-incom-scrapy

  ports:
    - 8888:8888

  volumes_from:
    - qa-incom-data

```
	

2. Se ubican las images para cada uno de los contenedores que tendremos
__drwxrwxr-x 6 radianv radianv 4096 jun 26 23:23 docker-images__

```
total 16
drwxrwxr-x 2 radianv radianv 4096 abr 25 22:00 base
drwxrwxr-x 2 radianv radianv 4096 abr 25 22:00 data
drwxrwxr-x 2 radianv radianv 4096 abr 25 22:02 jupyter
drwxrwxr-x 2 radianv radianv 4096 abr 25 22:03 scrapy

```

3. Carpeta para poner la documentación del proyecto
__drwxrwxr-x 2 radianv radianv 4096 jul 10 02:09 documentacion__
	
-rw-rw-r-- 1 radianv radianv  520 jun 24 03:19 README.md
	

4. Carpeta para poner shell en caso de utilizarlos
__drwxrwxr-x 3 radianv radianv 4096 jun 30 23:51 shell__


5. Carpeta donde tendremos el codigo de los spiders que vamos a utilizarlo
__drwxrwxr-x 7 radianv radianv 4096 jul  9 23:25 spider__


### INCIALIZACIÓN DEL AMBIENTE

Validamos e Inicializamos el ambiente via `docker-compose up -d` para lista los servicios que usaremos, para este caso tenemos 3:

```
           Name                           Command               State            Ports          
------------------------------------------------------------------------------------------------
incomfinal_qa-incom-data_1      sh                               Exit 0                          
incomfinal_qa-incom-jupyter_1   tini -- start-notebook.sh        Up       0.0.0.0:8888->8888/tcp 
incomfinal_qa-incom-scrapy_1    /docker-entrypoint.sh postgres   Up       0.0.0.0:5432->5432/tcp

```

* data_1: contiene la persistencia de datos
* jupyter_1: procesamiento de datos y algoritmos de agrupación con Spark 1.6
* scrapy_1: motor de scrapeo y almacenamiento en psotgreSQL

### NOS CONECTAMOS AL AMBIENTE POSTGRES


![UNDER CONSTRUCTION](https://www.google.com.mx/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&cad=rja&uact=8&ved=0ahUKEwjg-JaNlMHTAhWCLyYKHRyOCh4QjRwIBw&url=http%3A%2F%2Fwww.wauchopeshowsociety.com.au%2Fpage-under-construction.html&psig=AFQjCNG9_q7CU1LTVJq8oYDkhiCmwNaFsw&ust=1493262945520435)


### NOS CONECTAMOS AL AMBIENTE SPARK

1. Vía lineade comandos tenemos `docker exec -it incomfinal_qa-incom-jupyter_1  /bin/bash`
2. entramos al directorio donde se ubica spark `/usr/local/spark` y ejecutamos `./bin/spark-shell`
3. 

```
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.6.1
      /_/
                        
Type --help for more information.

```
__NOTA:__ para trabajar de forma interactiva podemos ir al sitio de ![Jupyter](http://localhost:8888/tree/notebook#)

#### Adrián Vázquez
Maestría Ciencia de Datos ITAM.

