---
layout: post
title: Plataformas de Electrónica
author: David E. Barrera
date: 2018-12-15 14:32:00 -0500
tags: [developer, job skills, projects, electronics, software, hardware, arduino, programming language]
---

A que me dedico cuando no estoy en el trabajo? A peque&ntilde;os proyectos electr&oacute;nicos que en algun momento han de ser utiles... Hoy les presento algunos ejemplos...

## Arduino ##

<img src="/assets/arduino-1050655_1280.jpg" class="img-fluid">

El microcontrolador por excelencia... Pero muchos escuchan el nombre microcontrolador y ya piensan
que es complicad&iacute;simo, que se requiere un diplomado, masterado, o alg&uacute;n t&iacute;tulo
avalado por alguna universidad cara para poder trabajar con eso... Al contrario! Si puedes usar una
calculadora, estas trabajando con un microcontrolador (no exactamente, pero el concepto es ese)...

Qu&eacute; es un microcontrolador? En palabras simples es un chip que repetidamente hace lo que se le diga 
que haga... Por ejemplo, si yo le digo prende y apaga un foco, el microcontrolador prende y apaga ese
foco continuamente hasta que 1) le cambie las instrucciones o 2) lo desconecte de la fuente de energia.

Simple, verdad? Ok, entonces, c&oacute;mo le decimos al microcontrolador que instrucciones realizar?
Bueno, ah&iacute; es donde puede complicarse un poco la cosa... Para ciertos microcontroladores, se 
requieren equipos especializados y lenguaje de programaci&oacute;n de bajo nivel (Assembler). Pero,
con el nacimiento de la plataforma Arduino, esto de programar microcontroladores se descomplic&oacute;
de gran manera logrando que este arte pueda ser realizado por humanos comunes, como usted y yo...

Arduino[[1]][arduino_about] descomplica el uso de microcontroladores de la siguiente manera:

1. Facilita la comunicaci&oacute;n entre el programador y el microcontrolador haciendo uso de 
est&aacute;ndares de comunicaci&oacute;n como el USB. Toda computadora tiene al menos un puerto USB.

2. Permite usar lenguaje de programaci&oacute;n de alto nivel (lenguaje de programaci&oacute;n C++ principalmente)
para manipular el microcontrolador. Tambi&eacute;n pueden ser usados otros lenguajes como Python,
JavaScript, hasta Scratch!

Siendo as&iacute;, ahora la &uacute;nico que falta es como usar correctamente el lenguaje para que el
microcontrolador lo entienda. A continuaci&oacute;n, un ejemplo:

{% highlight c linenos %}
void setup() 
{
    pinMode(LED_BUILTIN, OUTPUT);
}

void loop()
{
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
}
{% endhighlight %}

Analicemos el c&oacute;digo:

* Lineas 1 y 6: Como hab&iacute;a dicho, un microcontrolador es un chip que hace algo repetidas veces.
La linea 1 establece la configuraci&oacute;n inicial. La linea 6 establece las instrucciones a ser repetidas.
* Linea 3: Como configuraci&oacute;n inicial decimos que el pin <code>LED_BUILTIN</code> sea de salida, esto es
que este pin enviar&aacute; informaci&oacute;n hacia afuera. Pero qu&eacute; significa <code>LED_BUILTIN</code>?
Las diferentes placas soportadas en la plataforma Arduino tienen un foquito, un LED, integrado, pero cada placa
lo tiene en un diferente pin, por ejemplo el modelo UNO lo tiene en el pin 13, el modelo MKR1000, en el pin 6.
Entonces, en vez de especificar manualmente por cada modelo el pin del LED, usamos esta variable que lo hace
por nosotros.
* Lineas 8 y 10: La instrucci&oacute;n <code>digitalWrite()</code> lo que hace es escribir en <code>LED_BUILTIN</code>
el estado, <code>HIGH</code>, encendido, o <code>LOW</code>, apagado.
* Lineas 9, 11: La instrucci&oacute;n <code>delay()</code> lo que hace es detenerse, hace una pausa por el tiempo que se
le indique en milisegundos; en nuestro caso, 1000 milisegundos que equivale a 1 segundo.

Una vez analizado el c&oacute;digo linea por linea, podemos ver que el script lo que hace es encender un led por un segundo
y lo apaga por un segundo y esto lo repite indefin&iacute;damente, o mas espec&iacute;ficamente, hasta que cambie las 
instrucciones o hasta que apague el microcontrolador, como hab&iacute;a mencionado antes.

Con el despertar de la comunidad maker gracias a esta plataforma, muchos componentes existen para trabajar con Arduino.
Mis favoritos son:

* Display OLED 0.96&quot; - Display OLED monocrom&aacute;tico de 128x64 pixeles
* DHT11 - Sensor de Temperatura y Humedad
* BMP180 - Sensor de Presion Atmosferica y Temperatura
* HC-SR04 - Sensor de Distancia Ultrasonico
* LDR - Sensor de Luz
* Modulo Relay de n Canales - Control de equipos de Alto Voltaje
* HC-SR501 - Sensor de Movimiento Infrarrojo
* 44E Hall Effect - Sensor Magnetico

Existen muchos m&aacute;s, pero estos en particular son mis favoritos. Pero lo interesante de Arduino es que no solo trabaja
con m&oacute;dulos externos, sino tambien permite ser expandido usando lo que se denomina '''shield'''s. Un shield es una
placa que se coloca por encima del Arduino y expande la funcionalidad, como por ejemplo:

* [Easy Module Shield](https://www.banggood.com/Multifunction-Expansion-Board-DHT11-LM35-Temperature-Humidity-For-Arduino-UNO-p-996800.html?p=DQ30066511122014069J&utm_campaign=educ8stv&utm_content=huangwenjie&cur_warehouse=CN)
* [Adafruit Motor Shield](https://www.adafruit.com/product/1438)
* [GSM Shield](https://www.ebay.com/itm/221236220201)
* [Joystick Shield](https://www.sparkfun.com/products/9760)
* [GBK IoT Shield](https://www.gbkrobotics.com.br/boards)
* [Arduino Ethernet Shield](https://www.amazon.com/SunFounder-Ethernet-Shield-W5100-Arduino/dp/B00HG82V1A/)

Y as&iacute; como con los m&oacute;dulos, existen muchos m&aacute;s.

Podemos ver que es una plataforma bastante flexible, donde podemos hacer volar nuestra imaginaci&oacute;n para crear.

## Raspberry Pi ##

<img src="/assets/raspberry-pi-950490_1920.jpg" class="img-fluid">

En el mundo actual de la electr&oacute;nica, Arduino no es la &uacute;nica plataforma que permite el desarrollo de
proyectos. Tambi&eacute;n hay otro jugador. Raspberry Pi. A diferencia del Arduino que es un microcontrolador, el
Raspberry Pi es una computadora en toda la extensi&oacute;n de la palabra, no solo ejecuta un conjunto de intrucciones
preprogramadas una y otra vez, sino que ejecuta procesos y podemos interactuar con este como lo hacemos con una
computadora normal.

Muchas veces la confusi&oacute;n radica en su apariencia f&iacute;sica y en su enfoque. Por ejemplo, ambos pueden ser
usados con m&oacute;dulos y HATs (como Shields, pero para Raspberry Pi). Pero con el Raspberry Pi no solo estamos limitados
a hacer una sola acci&oacute;n, sino que podemos realizar varias acciones a la vez. Por ejemplo, podemos leer datos 
atmosf&eacute;ricos y almacenarlos en una base de datos para poder ser visualizado en una pagina web, todo esto en el mismo 
dispositivo. Saliendo del mundo de la electr&oacute;nica, la plataforma Raspberry Pi esta dise&ntilde;ada para 
ser una computadora peque&ntilde;a y accesible para aprender a programar. [[2]][raspberry_about]

### Programaci&oacute;n ###

Como Raspberry Pi es una computadora con su propio sistema operativo, no necesitamos programarla, pero podemos escribir
programas para que trabajen con algun HAT en particular, o tan solo usando algunos componentes exteriores. El lenguaje
de programaci&oacute;n que podemos usar son varios, Python y Scratch son los mas populares. Usemos Python para recrear el
ejemplo de arriba.

{% highlight python linenos %}
from gpiozero import LED  # importamos el objeto LED de la libreria gpiozero
from time import sleep    # importamos la funcion sleep() de la libreria time

led = LED(17)             # decimos que el LED esta conectado al pin 17

while True:               # creamos un loop infinito
    led.on()              # enciende led
    sleep(1)              # espera 1 segundo
    led.off()             # apaga led
    sleep(1)              # espera 1 segundo
{% endhighlight %}

Aunque con los comentarios podemos ver qu&eacute; es lo que sucede, aqu&iacute; podemos ver que las instrucciones son
un poco m&aacute;s claras. Tambi&eacute;n podemos ver unas instrucciones especiales al inicio, <code>from gpiozero import LED</code>
y <code>from time import sleep</code>. A diferencia de Arduino, que usa una modificaci&oacute;n de C++ dise&ntilde;ado para
programar microcontroladores, Python es un lenguaje de programaci&oacute;n general que desconoce que existen los pines para
conectar componentes externos a la computadora, es por esto que se debe agregar al c&oacute;digo una libreria [[3]][python_libraries]
, o al menos un componente de esa libreria, en nuestro caso importamos el m&oacute;dulo LED de la librer&iacute;a gpiozero
[[4]][gpiozero_docs], haciendo m&aacute;s sencillo el uso de componentes en proyectos de eletr&oacute;nica.

## Proyectos ##

Como podr&aacute;n ver, mi entusiasmo por este tema es alto. Por lo cual me he involucrado en varios proyectos en los
&uacute;ltimos a&ntilde;os. A continuaci&oacute;n los m&aacute;s destacados:

## Medidor de Calorias ###

Creo que este fue uno de mis primeros proyectos. El objetivo de este proyecto era medir las calor&iacute;as consumidas basado 
en el giro de rueda de una bicicleta est&aacute;tica. Para este proyecto us&eacute; los siguientes materiales:

* Im&aacute;n
* Sensor de Efecto Hall
* Pantalla LCD 16x2
* Arduino
* Bicicleta est&aacute;tica

El concepto es sencillo, basado en la fuerza ejercida para mover la rueda, calcular las calor&iacute;as consumidas. Suena 
sencillo, verdad? El problema fue encontrar los c&aacute;lculos f&iacute;sico-matem&aacute;ticos para llegar a esto, especialmente
para la &uacute;ltima parte. Pero como todo problema, es de resolverlo por partes. Lo primero era lograr 
[medir la velocidad del giro de la rueda][tachometer]. Luego era [calcular la fuerza ejercida para tener esa velocidad][fisica_bicicleta].
Finalmente era calcular las calorias consumidas al ejercer dicha fuerza[[1]][calories_one][[2]][calories_two][[3]][calories_three].
Como hab&iacute;a dicho, esto &uacute;ltimo fue lo m&aacute;s complicado ya que la informaci&oacute;n es escasa y la f&oacute;rmula 
es variable de acuerdo al peso y estatura de la persona, valores que deberi&aacute;n ser ajustados en cada sesi&oacute;n.

### Botonera Inal&aacute;mbrica ###

No s&eacute; si el nombre es el correcto, pero expresa bien el concepto. La idea es esta: un profesor publica un cuestionario 
de opciones m&uacute;ltiples y los estudiantes responden desde estos dispositivos. Pero lo interesante de este proyecto fueron
dos cosas: 1) hab&iacute;a que hacer el lado administrativo para el profesor (crear las preguntas, las respuestas, tener un 
cuadro estad&iacute;stico de las respuestas por cuestionario) y el web service para la botonera obtener las preguntas y opciones
y mostralas en una pantalla que tiene cada botonera. Veamos los materiales:

* Arduino Y&uacute;n
* Pantalla LCD 20x4
* Botones

Para este proyecto tuve que hacer mucha investigaci&oacute;n ya que nunca hab&iacute;a trabajado con un Y&uacute;n. 
T&eacute;cnicamente deber&iacute;a ser sencillo, ya que he trabajado con Arduino por mucho tiempo, pero la interfaz 
inal&aacute;mbrica era manejada por un microprocesador al cual acced&iacute;a mediante otras librer&iacute;as que no eran las
normales. Y para rematar, no existe mucha documentaci&oacute;n acerca del uso del Y&uacute;n, lo cual complicaba las cosas.
Por ah&iacute; encontr&eacute; una gu&iacute;a de como hacer un [HTTP Request usando un bot&oacute;n][yun_httprequest]. Pero
el ejemplo funcionaba para un bot&oacute;n... Yo ten&iacute;a varios! Entonces encontr&eacute; un [video][arduino_hotkeypanel] 
que usa una especie de interrupciones para sensar los botones. Mezcl&eacute; ambos conceptos y logr&eacute; que funcione. Cada 
bot&oacute;n enviaba un HTTP Request distinto para almacenar diferentes respuestas en la base de datos. 

Todo iba bien hasta que todo cambi&oacute;... En la pantalla no solo deb&iacute;a mostrar las opciones de respuesta, sino 
tambi&eacute;n las preguntas! Un metodo adicional al Web Service, un HTTP Request adicional en el Arduino. Lo que qued&oacute;
pendiente fue manejar la longitud de la pregunta, ya que usaba una linea de la pantalla para mostrar la pregunta, si exced&iacute;a
los 20 caracteres, se cortaba la pregunta.

[[C&oacute;digo]](https://gist.github.com/dbarrerap/cec2f5deaaa3b321426103c168efb6d6)

### Parqueo Inteligente ###

Este proyecto lo hice dos veces, pero el primero fall&oacute;. Pero de esa falla, aprend&iacute; para mejorar en el segundo.
En la segunda versi&oacute;n hubieron complicaciones, pero todo sali&oacute; bien al final. Vamos con los materiales:

* Arduino Mega
* Sensores de Distancia HC-SR04
* Sensor de Obstaculos FC-51
* Motores Servo
* Pantalla LCD 16x2 con Interfaz I2C
* HC-05 BT
* Cables, muchos cables

Este proyecto se ha convertido en un cl&aacute;sico para tesis o proyectos de grado. Honestamente, la idea es sencilla, sensar cada
parqueo y mostrar el estado de cada uno en una pantalla. Aunque en esta parte sencilla es donde la mayoria falla. Tienden a crear
una variable por cada sensor de distancia en vez de hacer un <kbd>array</kbd>. Lo interesante de esta segunda vez fue que hab&iacute;a
que tomar en cuenta los parqueos de accesibilidad reducida por separado. Lo nuevo, manejar tambi&eacute;n la pluma de acceso con una
aplicaci&oacute;n m&oacute;vil via Bluetooth, mostrando un mensaje de acuerdo al estado uno de bienvenida o uno diciendo que no hay 
disponibilidad.

[[C&oacute;digo]](https://gist.github.com/dbarrerap/5d606b0341682664e98fbc1536f4ecca)

### Estaci&oacute;n Metereol&oacute;gica ###

Este es mi proyecto actual. A&uacute;n no he escrito ning&uacute;n c&oacute;digo, solo he probado los componentes. 
Hasta el momento tengo:

* Arduino Uno
* DHT11
* BMP180

Con esto tengo temperatura, presi&oacute;n y humedad, lo suficiente como para pronosticar lluvia (quiza?). 
Al menos el concepto es sencillo, alta humedad y presi&oacute;n, baja temperatura indican alta probabilidad de lluvia. 
La matem&aacute;tica detr&aacute;s, probablemente sea la mas complicada. A&uacute;n no hago mi investigaci&oacute;n. 
Y por &uacute;ltimo har&aacute; un registro horario de las mediciones en una base de datos, por lo cual he de necesitar 
un m&oacute;dulo WIFI (ESP8266) o cambiarme de plataforma a Wemos (D1 Mini parece ser el mejor candidato, aunque a&uacute;n no
lo puedo dominar completamente). Otra cosa que quiero agregar es un [medidor de indice UV][adafruit_veml6075]. Hasta ah&iacute; 
va mi idea por ahora.

[arduino_about]: https://www.arduino.cc/en/Guide/Introduction
[python_libraries]: https://docs.python.org/3/tutorial/modules.html
[raspberry_about]: https://www.raspberrypi.org/about/
[gpiozero_docs]: https://gpiozero.readthedocs.io/en/stable/
[tachometer]: http://engineerexperiences.com/tachometer-using-arduino.html
[fisica_bicicleta]: https://jaivan.wordpress.com/2011/03/25/fisica-de-la-bicicleta-ii/
[calories_one]: http://board.crossfit.com/showthread.php?t=37494
[calories_two]: https://www.reddit.com/r/crossfit/comments/7bj678/assault_bike_rpm_watts_to_calories_graph/
[calories_three]: http://board.crossfit.com/showthread.php?t=86229
[yun_httprequest]: https://forum.arduino.cc/index.php?topic=472732.0
[arduino_hotkeypanel]: https://www.youtube.com/watch?v=1wWT9QnN1YI
[adafruit_si1145]: https://www.adafruit.com/product/1777
[adafruit_veml6075]: https://www.adafruit.com/product/3964