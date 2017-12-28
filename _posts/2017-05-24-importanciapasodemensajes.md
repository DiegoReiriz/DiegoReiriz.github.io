---
title: Entendiendo la importancia del paso de mensajes
image: https://cdn-images-1.medium.com/max/800/0*7na_PaxW7A1VG6FN.png
category: development
tags: [arquitectura, paso, de, mensajes, desacoplamiento, ventajas]
---
Esta vez me apetece hablar de un concepto de diseño de software, como es el paso de mensajes, que ha calado en múltiples niveles del desarrollo del mismo, así como en la organización y la comunicación de los sistemas informáticos .

## En que cosiste el paso de mensajes?

El paso de mensajes, lo podemos considerar como un tipo de arquitectura de software orientada a la comunicación de varios elementos, que se caracteriza por tener muy poco acoplamiento entre los elementos participes. Como su nombre indica, la idea detrás de esta arquitectura reside en la comunicación de los elementos mediante el envió de mensajes, en lugar de realizar una interacción directa entre ellos.

## El paso de mensajes en su versión mas simple

La idea detrás de esta forma de diseñar software es bastante sencilla y natural para nosotros como seres humanos, tenemos a un productor de mensajes, un canal/medio en el que se publican mensajes y uno o varios receptores.

![assets/images/importanciapasodemensajes/mp0.png](assets/images/importanciapasodemensajes/mp0.png)

En este tipo de diseños, el emisor no necesita realmente especificar quien es el destinatario del mensaje que quiere enviar, su única preocupación es producir un mensaje y publicarlo en el canal de comunicación adecuado. Mientras que en el lado del Receptor, este simplemente se tiene que preocupar de si el mensaje se dirige a él y si entiende el contenido del mensaje.
Si aplicamos esta idea al desarrollo de software, podríamos tener un código (java en este caso) como el siguiente:

<script src="https://gist.github.com/DiegoReiriz/b62996ee9983060bc0460221dd092724.js"></script>

<script src="https://gist.github.com/DiegoReiriz/2206321423775b35f5489b12a7f11a32.js"></script>

<script src="https://gist.github.com/DiegoReiriz/8410eace6868f5bcd1312a8be6d1b2f7.js"></script>

<script src="https://gist.github.com/DiegoReiriz/5bf0ce54a2f6003f393ad23c55307fbb.js"></script>

<script src="https://gist.github.com/DiegoReiriz/ff60f6ce9f83d0ef02f33910a22a2034.js"></script>

**Resultado**

> Consumidor: Acabo de recibir: Hello World!
> Consumidor: Acabo de recibir: 123456

## Esto nos tiene que sonar
La implementación realizada anteriormente nos debería recordar al patrón de diseño Observer, donde un emisor mantiene una lista de Observadores que están a la espera de que el emisor emita algún tipo de evento.

![assets/images/importanciapasodemensajes/mp1.gif](assets/images/importanciapasodemensajes/mp1.gif)

La principal diferencia entre el diseño anterior y un patrón de diseño Observer puro, es que en nuestro diseño estamos desacoplando a los emisores y a los observadores mediante la introducción de un canal. Lo que realmente tenemos es una versión muy simple de un patrón de arquitectura de paso de mensajes, conocido como publica-subscribe.

![assets/images/importanciapasodemensajes/mp2.jpg](assets/images/importanciapasodemensajes/mp2.jpg)

## Que principales ventajas nos proporcionan este tipo de diseños?

- **Bajo acoplamiento**: como se puede apreciar en la implementación que realicé anteriormente, el productor únicamente necesitar tener acceso al canal y publicar un mensaje, por lo tanto se desentiende de quienes están recibiendo el mensaje, como lo reciben y que hacen con el mismo.
- **Alta escalabilidad**: como se puede observar en el código proporcionado, “teóricamente” podemos aumentar el número de consumidores y productores de un canal de forma indiscriminada. Así mismo, podemos disponer de diferentes canales en función de su propósito y tanto un productor, como un consumidor, puede participar de forma activa en varios canales simultáneamente.
- **Debuggear facilmente**: resulta muy sencillo el desarrollo de pruebas sobre este tipo de sistemas, porque podemos substituir cualquier productor o consumidor de mensajes por uno falso para asegurarnos de que el sistema se comporta como nosotros esperamos. Así mismo, también podemos introducir consumidores que actúen como sniffers en los canales, de forma que estos registren todos los mensajes que se envían por el canal y mantengan un log.

## Que damos a cambio?

Cuando aplicamos cualquier tipo de patrón de diseño, así como de arquitectura, siempre estamos sacrificando algo. En este caso, el rendimiento de nuestro programa/sistema será inferior al de uno en el que todos los elementos del mismo se conecten directamente, pero es un precio justo si consideramos todo lo que estamos ganando.

## Evolucionando el diseño base

Lo realmente interesante respecto al patrón de arquitectura que se ha presentado, es su gran versatilidad, ya que sin complicarnos mucho la vida podemos implementar y combinar cualquiera de los patrones de comunicación que se nos ocurra.

![assets/images/importanciapasodemensajes/mp3.png](assets/images/importanciapasodemensajes/mp3.png)

Por otro lado, si simplemente modificamos el canal que forma parte de nuestro diseño actual, también podemos lograr comportamientos bastante interesantes. A continuación se muestran posibles modificaciones:

- **Adición de persistencia al canal**: puede ser de gran utilidad que el canal registre y mantengan un número determinado de mensajes y que los consumidores elijan cuando consumirlos, dando lugar al famoso patrón productor-consumidor.

![assets/images/importanciapasodemensajes/mp4.jpg](assets/images/importanciapasodemensajes/mp4.jpg)

- **Soporte de operaciones sobre el canal**: independientemente de si el canal dispone o no de persistencia para los mensajes, es interesante barajar la opción del diseño de un canal que aplique una operación (normalmente una función pura) a todos los mensajes que este contiene, ya sea aplicándola a todos los elementos actuales almacenados en el canal o de forma individual cuando estos se añaden al mismo. El tipo de operaciones que se pueden realizar son de todo tipo: operaciones de filtrado, operaciones de ordenación, operaciones aritméticas (si fijamos el tipo de datos del canal), operaciones de transformación…

![assets/images/importanciapasodemensajes/mp5.png](assets/images/importanciapasodemensajes/mp5.png)

- **Unión y división de canales**: es interesante hacer que un mismo canal se pueda subdividir en varios bajo una condición, o justamente lo contrario, que varios canales agrupen sus mensajes.

![assets/images/importanciapasodemensajes/mp6.png](assets/images/importanciapasodemensajes/mp6.png)

Otra opción que tenemos a la hora de evolucionar el diseño presentado, es la adición de un nuevo elemento, el broker. El principal propósito de este es coordinar las llamadas a servicios en arquitecturas orientadas a los mismos, además de poder validar si las peticiones cumplen con el formato esperado. Si queremos generalizar el comportamiento de este, para la situación que hemos planteado, podemos considerarlo como una fachada que elige a que canales se dirigen los mensajes. Donde en este caso, lo más probable es que los canales dispongan de una serie de acciones o filtros que adecuan el formato de los mensajes a los consumidores del canal.

![assets/images/importanciapasodemensajes/mp7.png](assets/images/importanciapasodemensajes/mp7.png)

Podría continuar sugiriendo más modificaciones sobre el diseño base, pero creo que ha quedado más que claro que el diseño es bastante versátil y al final todo consiste en adaptarlo a las necesidades que tengamos en cada caso.

## Donde se usa?

Actualmente el diseño presentado se usa a varios niveles y cada producto/software/sistema lo implementa a su manera, pero los conceptos clave acaban siendo los mismos en todos o presentando pequeñas variaciones. A continuación se muestran algunos ejemplos:

- **ReactiveX**: framework disponible en múltiples lenguajes de programación, que combina el patrón observer, con el patrón iterator y programación funcional. http://reactivex.io/
- **Apache Kafka**: plataforma de streaming distribuido. https://kafka.apache.org
- **RabbitMQ**: broker de mensajes. https://www.rabbitmq.com/
- **Message Passing Interface (MPI)**: sistema de paso de mensajes orientado a la computación paralela. https://www.open-mpi.org/

## Epílogo

Gracias por leer todo este rollo que he soltado y que hago en mis ratos libres! Al final, para mi esto es una forma de aportar un poco de mis conocimientos con la finalidad de que puedan llegar a ser útiles para otros, al igual que yo he leído gran cantidad de posts publicados por gente que sabe mucho más que yo.

Se acepta cualquier sugerencia, impertinencia, improperio, comentario, etc. siempre que sea con fundamento y afán de mejorar/corregir/aumentar el contenido de este artículo.
