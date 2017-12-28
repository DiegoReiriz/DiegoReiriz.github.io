---
title: Conectando Apache Flink con Elasticsearch
image:  /images/elasticflink.png
category: development
tags: [apache, flink, elasticsearch, connectors, rest, scala, scalaj]
---

Hace poco he empezado a utilizar el framework para desarrollo de aplicaciones en Streaming **Apache Flink**, el cuál es considerado como la 4ª generación de herramientas para el análisis de datos en un contexto de Big Data.

![https://a248.e.akamai.net/secure.meetupstatic.com/photos/event/9/2/8/d/600_455497517.jpeg](https://a248.e.akamai.net/secure.meetupstatic.com/photos/event/9/2/8/d/600_455497517.jpeg)

En general, la facilidad de uso del framework me ha parecido sorprendente, ya que permite realizar todo tipo de acciones distribuidas sobre nodos mediante el uso de funciones muy conocidas como pueden ser **map** y **reduce**, al mismo tiempo que permite realizar operaciones complejas sobre los datos en streaming, mediante el uso de varios tipos de ventanas.

Pero no todo en el framework va a ser bueno, en estos momentos la herramienta se encuentra en la **versión 1.4** y **está creciendo** a un ritmo bastante sorprendente, pero el problema reside en que ni la documentación de la herramienta, ni la comunidad, están creciendo al mismo ritmo que el framework. Esto lo pude observar cuando intenté realizar una operación que debería ser relativamente sencilla, la cual consistía en almacenar los **datos procesados por un streaming**, en un contenedor de **Docker** sobre el que se ejecutaba una imagen de **elasticsearch 5.4**. A pesar de que en la documentación del framework Apache Flink existe un apartado de **conectores**, en el que se documenta como conectar el framework a la plataforma elasticsearch, **en mi caso no he conseguido hacer funcionar el conector** tras haberlo intentado un par de veces y tras realizar alguna que otra búsqueda de ejemplos, parece que **no soy el único** que se ha encontrado con esta barrera.

# Abordando el problema

Para empezar, existen **dos formas** de conectarse a elasticsearch:

1. Mediante la API Java que se expone en el puerto 9300 por defecto
2. Mediante la API REST que se expone en el puerto 9200 por defecto

En una situación ideal, preferiría conectarme a elasticsearch mediante la API Java, pero he podido observar que las diferentes versiones de Apache Flink varían bastante y es necesario usar en cada versión los conectores específicos disponibles que ofrece el framework, para poder conectarnos a elasticsearch y esto genera unas dependencias en el proyecto con las que no me siento nada cómodo trabajando.

Entonces he optado por pasarme a mi segunda opción que es utilizar la API REST para conectarme a elasticsearch y de esta forma **me olvido de los problemas de compatibilidad entre las diferentes versiones** de elasticsearch con Apache Flink y sus conectores.

# Manos a la obra

En mi caso estoy utilizando el framework **Apache Flink** en su versión **1.3**, con la versión **2.10** del lenguaje **Scala**.

Lo primero que he realizado es añadir como dependencia al proyecto la libreía [scalaj-http](https://github.com/scalaj/scalaj-http) que actúa como un wrapper de  [java.net.HttpURLConnection](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html) y añade bastante azucar sintáctico. Para ello basta con añadir a nuestro **pom.xml** la siguiente dependencia.

<script src="https://gist.github.com/DiegoReiriz/518b3ad1323b6c1e8a9bce9b7e61e3a2.js"></script>

Una vez que ya hemos añadido la dependencía, basta con realizar una simple petición POST contra la API de elasticsearch para insertar un dato, contra un **índice que crearamos previamente**. En este caso insertaremos un dato en el índice tfg:

<script src="https://gist.github.com/DiegoReiriz/83df80346f932d347d581e1c5ca1f661.js"></script>

Si queremos aplicar esto sobre los datos de nuestro streaming en Flink, bastaría con aplicar una operación map sobre el streaming:

<script src="https://gist.github.com/DiegoReiriz/7f2b410c9950bccb379e4721bbe24f33.js"></script>

Con esto, ya estaríamos insertando de **forma sencilla** nuestros datos en elasticsearch. Obviamente, esta **no es la forma más eficiente** de insertar los datos, ya que estamos generando una petición http por cada elemento de nuestro stream, cuando lo más razonable sería **esperar a que se cumpla una ventana temporal** o se **acumule un volumen de datos** considerable antes de realizar una insercción. Pero eso ya son cuestiones de diseño dependientes del comportamiento de cada sistema.

Por último, si queremos que esto tenga un toque **un poco más profesional**, en vez de formar en un String el JSON que queremos enviar en el cuerpo de la petición POST, sería conveniente utilizar un motor de renderizado de plantillas como puede ser [mustache](https://mustache.github.io/) o [pug](https://pugjs.org/api/getting-started.html).
