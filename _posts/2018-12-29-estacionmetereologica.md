---
layout: post
title: Estacion Metereologica
author: David E. Barrera
date: 2018-12-29
comments: false
tags: [proyectos, electronica, iot, python, javascript, json, sqlite]
---
Implement&eacute; mi peque&ntilde;a estaci&oacute;n meteorol&oacute;gica 
y la visualizaci&oacute;n de datos es bell&iacute;sima...

Como les hab&iacute;a contado antes, ten&iacute;a en mente hacer una peque&ntilde;a estaci&oacute;n 
metereol&oacute;gica para medir patrones del clima, especialmente temperatura, 
humedad y presi&oacute;n, para tener una idea de las condiciones correctas para 
la lluvia, para &eacute;pocas secas, etc. Hasta ah&iacute; parec&iacute;a c&aacute;lculos num&eacute;ricos 
y cosas aburridas... Pero cuando hice la visualizaci&oacute;n de datos mediante 
gr&aacute;ficos, fue absolutamente hermoso...

Iniciemos con los materiales:

* Raspberry Pi Zero (Recomiendo usar el W)
* DHT11
* BMP180
* 128x64 OLED Display

Lenguajes usados:

* Python
* JavaScript

## Procedimiento ##

Al principio ten&iacute;a la idea de hacer toda una aplicaci&oacute;n web para leer 
los datos de una base de datos, tanto as&iacute; que hasta pensaba en usar 
MySQL! Pero durante el dise&ntilde;o del programa para obtener los datos, me di 
cuenta que usar MySQL ser&iacute;a un gran desperdicio de recuros, por lo cual 
lo reduje a SQLite, ya que se usa una sola tabla donde se almacenan 4 
datos: Temperatura, Presi&oacute;n, Humedad y Tiempo de Lectura. Hasta ah&iacute;, 
todo viento en popa. Pero luego dije, as&iacute; como fue con la base de datos, 
ser&aacute; conveniente usar todo un framework de aplicaci&oacute;n web para una base 
de datos (tabla de datos?) as&iacute; de peque&ntilde;a? Aunque la cantidad de datos 
si es considerable, ya veremos esto m&aacute;s luego. 

Entonces decid&iacute;, aparte de guardar en una base de datos, almacenar en un archivo por dia, 
como un log. Escog&iacute; el formato JSON para hacerlo m&aacute;s f&aacute;cil de escribir y leer 
por cualquier lenguaje de programaci&oacute;n. El script escrito en Python lee cada minuto los
sensores y almacena los datos en SQLite y JSON. Que tengo hasta ahora? Leo datos, almacenos datos... 
Me falta ver datos! 

Manteniendo la idea de usar la web para mostrar los datos, y sin usar un framework, me puse en 
la busqueda de leer archivos JSON usando JavaScript, porque tiene algo de sentido, verdad? 
Fue entonces que encontre [este sitio](https://codepen.io/KryptoniteDove/post/load-json-file-locally-using-pure-javascript)
donde explica c&oacute;mo usar <code>XMLHttpRequest</code> para leer un archivo.
Perfecto, ahora c&oacute;mo hago para cargar un archivo en particular? Pues f&aacute;cil,
lo meto en una funci&oacute;n, le paso el nombre del archivo como par&aacute;metro y listo!

Hasta ah&iacute;, leo el archivo JSON, lo cargo en una tabla en HTML y bonito (14000+ registros)...
Pero los sitios meteorol&oacute;gicos no solo ofrecen informaci&oacute;n texto, usan
muchos gr&aacute;ficos para visualizar datos! Recuerdo que cuando hice la
botonera inal&aacute;mbrica, hice uso de una librer&iacute;a para visualizar datos
de una base de datos (pero estaba usando PHP entonces), entonces tuve
que buscar algo diferente, que use JsvaScript. Encontr&eacute; la librer&iacute;a perfecta, 
[chartjs](https://www.chartjs.org/). Al principio quer&iacute;a poner las tres mediciones
en un solo gr&aacute;fico, pero al no tener unidades similares, el gr&aacute;fico se
ver&iacute;a muy mal, por lo cual decid&iacute; hacer tres gr&aacute;ficos diferentes.

Al final de todo, yo creo que es hermoso. Al ver la fluctuaci&oacute;n de los
valores (especialmente la presi&oacute;n), fue un momento muy chevere, el poder
visualizar estos cambios a lo largo del d&iacute;a que rara vez notamos, a menos 
que el cambio sea agresivo (por ejemplo, lluvia).

<img src="/assets/dataVisualization-thp.png" class="img-fluid">

Si desean, pueden revisar el c&oacute;digo (y darle una estrellista) en 
[Github](https://github.com/dbarrerap/weatherStation).
