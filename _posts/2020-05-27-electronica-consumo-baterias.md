---
title: "Determinar duracion de una bateria"
author: David E. Barrera
date: 2020/05/27
layout: post
tags: ['electronica', 'consumo', 'baterias']
comments: true
---
Mes tras mes aparecen nuevos teléfonos que prometen larga duración de batería, pero a la hora de usarlos, por lo general nos decepcionan. Por qué es esto? Hoy vamos a indagar un poco en el mundo de la electrónica y ver qué significa estos números que encontramos en las especificaciones de las baterías, con los cuales nos prometen larga duración.

<div class="row">
    <div class="col-md-4 offset-md-2">
        <img src="https://upload.wikimedia.org/wikipedia/commons/3/39/IPhone_battery_%281%29.JPG" class="img-fluid">
    </div>
    <div class="col-md-4">
        <img  src="https://cdn-shop.adafruit.com/970x728/1578-00.jpg" class="img-fluid">
    </div>
</div>

## Electricidad

Esto no es asombro, nuestros dispositivos electrónicos funcionan gracias a la electricidad. La electricidad es una forma de energía, la cual es facil de generar y convertir en otros tipos de energia. Esto nos permite usarla een diferentes aplicaciones de potencia, como hacer mover un motor, y tambien es ideal para manejar señales e informacion. Esto nos indica, pues, que tenemos dos formas de electricidad: estatica y dinamica. Nos enfocaremos en esta ultima, la electricidad dinamica.

La electricidad es el flujo de carga electrica a traves de un material conductor. Esta carga electrica es transportada por electrones, atomos neutros que permiten trasnportar electricidad. Un poco mas tecnico: los electrones tienen carga electrica negativa y se mueven en respuesta a un campo magnetico.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="https://i.imgur.com/mPmpCay.png" class="img-fluid">
    </div>
</div>

La electricidad, como flujo de carga eléctrica, es el movimiento promedio de todos estos electrones. En realidad, los electrones no se mueven en linea recta como en la imagen, sino se mueven en todas las direcciones, pero de manera promedio se mueven en una misma direccion. Algunos golpean con los limites del conductor y esto hace que pierdan energia. Recordemos tambien que la energia no se crea ni se destruye, sino que se transforma. Por lo cual la energia perdida del electron se convierte en otras formas de energia y una consecuencia indirecta de este efecto, es que podemos almacenar y procesar información usando electrones. Finalmente hay que tener en cuenta que la direccion de la electricidad, por convencion, fluye en direccion contraria al flujo de electrones.

Una analogia para entender mejor la electricidad es el agua en un rio. Este se mueve de un punto mas alto a un punto mas bajo. Mientras mayor sea la altura, mayor energia potencial tiene el agua y, a medida que va bajando, va perdiendo energia. Asimismo, el flujo de electricidad va desde un punto de potencial mas alto a un punto de menor voltaje. El **voltaje**, o potencial, es la energia que tiene un portador de carga y son más capaces de desarrollar trabajo útil.

Una bateria tiene un terminal positivo y uno negativo. El flujo electrico se mueve del punto de mayor potencial (la terminal positiva) hacia un punto de menor potencial (terminal negativa). En la imagen a continuacion, podemos ver que la corriente va desde el punto de mayor potencial, pasa por el foco, donde pierde energia, la cual se convierte en calor y luz y finalmente llega al punto de menor potencial.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="https://thumbs.dreamstime.com/z/ejemplo-del-vector-concepto-de-la-corriente-el%C3%A9ctrica-esquema-circular-el%C3%A9ctrico-movimiento-los-electrones-libres-y-%C3%A1tomos-116837847.jpg" class="img-fluid">
    </div>
</div>

Luego que perdio energia y regresa por el terminal negativo, el flujo se vuelve a cargar, se eleva el potencial de energia, y vuelve a salir por el terminal positivo.

Entonces, ¿qué es la electricidad? La electricidad es una forma de energía. Y además es el movimiento de cargas eléctricas a través de un circuito.

## Energia y Potencia

La energía en un circuito es una propiedad física que podemos medir y que describe el estado de un sistema, o de un objeto, o de un circuito. Existen varias formas de medir la energia, por ejemplo el _joule_, tambien esta el conteo de _calorias_, que mide el consumo de energia.

Hablemos de **potencia**. La potencia es la velocidad a la cual la energía va cambiando, ya sea convirtiéndose desde un sistema a otro, transformándose entre diferentes formas de energía, siendo consumida o generada. La unidad de medida de potencia en un circuito electrico es el _watt_.

Es posible calcular la potencia de un componente de un circuito electrónico como el producto entre voltaje y corriente, que es la forma más general. La corriente en un componente por el voltaje en sus terminales me da la potencia que está siendo consumida o entregada por ese componente y la energía es la acumulación de esa potencia en el tiempo.

La energía eléctrica puede ser almacenada en baterías y en otros medios. Hay diversas opciones de batería, que es la forma más común que tenemos para almacenar energía eléctrica. Estan las baterias alcalinas (1.5 Voltios por celda), de níquel-hidruro metálico (1.2 Voltios por celda), de plomo acido (6 o 12 Voltios, estos son los que se encuentran en carros) y de ión de litio o polimero de litio (3.7 Voltios por celda). Éstas son las que se utilizan, por ejemplo, en muchos teléfonos celulares o computadores.

## Capacidad

La capacidad de una bateria se mide en miliamperes-hora, **mAh**. Amperes es la unidad con la que se mide la corriente. Entonces esto es una unidad de corriente multiplicada por hora, por tiempo, esto es corriente por tiempo. Si multiplicamos amper por tiempo por volt, volt por amper me da potencia, y potencia por tiempo me da energía, perfecto.

Entonces, un miliamper-hora significa que una batería es capaz de entregarme un miliamper durante una hora, o medio miliamper durante 2 horas o 2 miliamper durante media hora.

Algo importante tambien a tener en cuenta es como se conectan las baterias.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="http://1.bp.blogspot.com/-6FMsupTeGqw/VLKoFRarVXI/AAAAAAAAAYA/q4aZfC5cn44/s1600/Battery%2BConnections-%2Bparallel%2Bfor%2Bhigh%2Bcurrent%2Band%2Bseries%2Bfor%2Bhigh%2Bvoltage.jpg" class="img-fluid">
    </div>
</div>

Las baterias en serie suman su voltaje manteniendo su capacidad. En cambio, en paralelo, las baterias mantienen su voltaje pero su capacidad aumenta. Asumamos que las baterias de la imagen anterior son de 12 Voltios con una capacidad de 1000 miliamper-hora, 1 amper-hora. Al conectarlas en paralelo, mantienen el voltaje pero la capacidad aumenta de 1 amper-hora a 3 amper-hora. Por otro lado, al conectarlas en serie el voltaje total seria de 36 Voltios con la misma capacidad de 1 amper-hora.

**IMPORTANTE**: Nunca conecten baterías en serie que tengan capacidades diferentes porque unas se van a descargar primero que otras y pueden provocar problemas. Nunca conecten en paralelo baterías que tengan voltajes diferentes porque van a provocar un cortocircuito.

## Implementacion

Apliquemos este conocimiento a un proyecto de electronica. En particular, yo tengo una placa desarrollo Feather Huzzah EPS8266. Segun sus especificaciones este tiene un consumo de 67 mA con picos de 250 mA, aunque realmente depende de lo que se este usando. Usaremos un [valor promedio](https://diyi0t.com/how-to-reduce-the-esp8266-power-consumption/) de 70 mA. Si lo conectamos con una bateria de 3.7 Voltios con una capacidad de 500 mAh podemos tener una duracion de 7 horas aproximadamente, asumiendo que usamos la bateria al 100% de efectividad, lo cual sabemos que no es real. Digamos que tenemos un 70% de efectividad, eso nos reduce la duracion a 5 horas. Ok, lo admito, estoy obviando ciertos parametros, como por ejemplo el modo de consumo de energia del modulo, pero esto es otro tema.

Otro ejemplo, un telefono movil. Tomemos el iPhone X. Segun [estas especificaciones](https://www.gsmarena.com/apple_iphone_x-8858.php) nos dicen que tiene una bateria de 2716 mAh y tiene un _talk time_ de 21 horas, esto es desactivando todas las diferentes formas de conectividad (WiFi, Bluetooth) y los demas parametros en el modo de menor consumo (pantalla en bajo brillo, por ejemplo). Veamos cual es el consumo del telefono. Si dividimos la capacidad para el tiempo de uso, obtendremos el consumo, esto es 129 mA. Pero en el caso de los telefonos moviles, en realidad, no es un valor fijo ya que el consumo depende de los modulos activos y uso del propietario, aunque existen tecnologias que ayudan a que el telefono movil sea mas eficiente, en materia de consumo. [Fuente](https://www.electronicdesign.com/technologies/pmics/article/21795444/what-drains-the-smartphone-battery)

## Conclusion

Despues de todo, que realmente esta sucediendo con las baterias y su duracion? La verdad, nada. Todo funciona de manera correcta. El problema es el marketing. Nos dicen que cada nuevo telefono tiene una bateria de mayor capacidad, lo cual es verdad. Pero el consumo tambien aumenta ya que se implementan nuevas tecnologias, lo cual nos lleva a un empate tecnico. Aumenta la capacidad, aumenta el consumo, se mantiene la duracion. Si usaramos una bateria de hoy en dia en un telefono de bajo consumo (uno de hace 10 años, con menos tecnologias) entonces veriamos mayor duracion y nos sorprenderian de verdad con el marketing de la capacidad de la bateria.

