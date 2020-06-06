---
title: 'Electronica: Que es PWM?'
author: David E. Barrera
date: 2020/06/04
layout: post
comments: true
tags: ['electronics', 'digital', 'analog', 'signal']
---
Es sabido que el mundo de la electrónica se basa en 1s y 0s, encendido y apagado. La plataforma Arduino permitió acercar este maravilloso mundo a los humanos mortales, pues antes de esto era para unos viejos locos de los talleres que comían manuales e iban a todas partes con un medidor.

## En el comienzo...

Iniciemos por el principio, existen dos ramas de electrónica, análoga y digital. Nos enfocaremos en la digital, ya que esta se encarga de sistemas electrónicos en los que la información está codificada en estados discretos, a diferencia de los sistemas analógicos donde la información toma un rango continuo de valores.

Estados dicretos? Rango continuo? Analicemos esto graficamente.

<div class="row">
    <div class="col-md-6">
        <figure class="figure">
            <img src="https://cdn.sparkfun.com/assets/3/7/6/6/0/51c48875ce395f745a000000.png" class="figure-img img-fluid" alt="Onda de señal análoga">
            <figcaption class="figure-caption">Onda de señal análoga</figcaption>
        </figure>
    </div>
    <div class="col-md-6">
        <figure class="figure">
            <img  src="https://cdn.sparkfun.com/assets/c/8/5/b/e/51c495ebce395f1b5a000000.png" class="figure-img img-fluid" alt="Onda de señal digital">
            <figcaption class="figure-caption">Onda de señal digital</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágenes tomadas de <a href="https://learn.sparkfun.com/tutorials/analog-vs-digital/all">learn.sparkfun.com</a></span>
    </div>
</div>

En una señal analoga, aun cuando esta limitada en el rango de valores maximos y minimos, aun hay un infinito de posibles valores que existan dentro de ese rango. Por ejemplo, un tomacorriente. Este tiene un valor maximo de 120 Voltios y un minimo de -120 Voltios. Pero a medida que nos acercamos a un punto en el tiempo, tambien llamado aumento de resolucion, vemos que ese punto de tiempo puede tomar un infinito de posibles valores, por ejemplo 64.4V, 64.42V, 64.424V y asi sucesivamente aumentando la precision del valor.

Para los sistemas digitales esta precisión trae consigo un problema. De existir ruido, cualquier interferencia, el valor de la señal en ese punto del tiempo puede verse alterado, lo cual afectaría la información almacenada o siendo transmitida, lo cual haría que el sistema sea no confiable. Es por esto que se toman rangos de valores y se les asigna un valor especifico, esto son los estados discretos, como podemos ver en la siguiente gráfica.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <figure class="figure">
            <img src="https://upload.wikimedia.org/wikipedia/commons/4/47/Imagen_4.png" class="figure-img img-fluid" alt="Onda de señal digital con ruido">
            <figcaption class="figure-caption">Onda de señal digital con ruido</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágen tomada de <a href="https://commons.wikimedia.org/wiki/File:Imagen_4.png">Wikipedia</a></span>
    </div>
</div>

Como podemos ver, estas señales se mantienen en cierto valor de voltaje un determinado tiempo lo cual nos indica su valor lógico.

## Pulsos electronicos y nuestro cerebro

Es bien sabido que es facil engañar al cerebro, casi hasta el punto que el ser humano desea ser engañado, pero eso es otro tema. A que quiero llegar con esta premisa? 

Supongamos una bombilla normal, la encendemos durante un segundo y apagamos por un instante con un switch. Obviamente percibimos ese cambio de estado por limitaciones del sistema, pero supongamos que ese apagado sucede durante una milesima de segundo, podriamos realmente decir que la bombilla fue apagada? Para el ojo humano la bombilla siempre esta encendida, nunca hubo un cambio. 

Ok, ahora reduzcamos el tiempo que la bombilla esta encendida y el tiempo de apagado aumenta de manera equitativa, manteniendo el mismo tiempo total. El tiempo que le toma a la bombilla encenderse ahora es menor, por lo cual no enciende por completo y se vuelve a apagar. Para el ojo humano esto se vería como si brillara con menor intensidad. Esto es denominado **persistencia de vision**, el cual es un supuesto fenómeno visual descubierto por Peter Mark Roget que demostraría como una imagen permanece en la retina humana una décima de segundo más, antes de desaparecer por completo, esto permitiría que veamos la realidad como una secuencia de imágenes ininterrumpidas y que podamos calcular fácilmente la velocidad y dirección de un objeto que se desplaza, si no existiese, veríamos pasar la realidad como sucesión de imágenes independientes y estáticas.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <figure class="figure">
            <img src="https://cdn.sparkfun.com/assets/f/9/c/8/a/512e869bce395fbc64000002.JPG" class="figure-img img-fluid" alt="Onda de amplitud acotada">
            <figcaption class="figure-caption">Onda de amplitud acotada</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágen tomada de <a href="https://learn.sparkfun.com/tutorials/pulse-width-modulation">learn.sparkfun.com</a></span>
    </div>
</div>

Esto es lo que se llama _Modulación por Ancho de Pulsos_ (Pulse-Width Modulation, **PWM**). De manera formal, PWM es una técnica en la que se modifica el ciclo de trabajo de una señal periódica para controlar la cantidad de energía que se envía a una carga. En el grafico anterior podemos ver ciclos de trabajo de 50%, 75% y 25%.

## Ejemplos

Se puede usar esta técnica para controlar el brillo de un LED.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <figure class="figure">
            <img src="https://arduinogetstarted.com/images/tutorial/how-it-works-led-fade.gif" class="figure-img img-fluid" alt="Brillo de LED controlado por PWM">
            <figcaption class="figure-caption">Brillo de LED controlado por PWM</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágen tomada de <a href="https://arduinogetstarted.com/tutorials/arduino-led-fade">ArduinoGetStarted</a></span>
    </div>
</div>

De tener un LED RGB, podemos alterar el valor de cada color para obtener diferentes colores.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <figure class="figure">
            <img src="https://dynamoelectronics.com/wp-content/uploads/2017/08/RGB-animation-funcion.gif" class="figure-img img-fluid" alt="Colores de LED RGB controlado por PWM">
            <figcaption class="figure-caption">Colores de LED RGB controlado por PWM</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágen tomada de <a href="https://www.dynamoelectronics.com/tienda/led-rgb-5mm/">DynamoElectronics</a></span>
    </div>
</div>

Tambien se puede usar esta tecnica para controlar un motor servo.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <figure class="figure">
            <img src="https://i2.wp.com/blog.330ohms.com/wp-content/uploads/2019/08/f15_243_tg_servo_motor.gif?resize=216%2C271&ssl=1" class="figure-img img-fluid" alt="Motor servo controlado por PWM">
            <figcaption class="figure-caption">Motor servo controlado por PWM</figcaption>
        </figure>
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-md-6 offset-md-3">
        <span class="text-muted">Imágen tomada de <a href="https://blog.330ohms.com/2019/08/24/microbit-tutorial-6-como-utilizar-los-servomotores/">Blog 330ohms</a></span>
    </div>
</div>

La técnica de PWM se utiliza en una variedad de aplicaciones, particularmente para el control. Ya sabes que puede usarse para atenuar los LED y controlar el ángulo de los servomotores, y ahora puedes comenzar a explorar otros posibles usos.

En el siguiente video, podemos ver un LED RGB siendo controlado con PWM en un difusor:

<iframe width="560" height="315" src="https://www.youtube.com/embed/M9ySSC35E9Q?start=69" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
