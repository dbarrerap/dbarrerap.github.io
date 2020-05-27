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

Esto no es asombro, nuestros dispositivos electrónicos funcionan gracias a la electricidad. La electricidad es una forma de energía, la cual es facil de generar y convertir en otros tipos de energia. Esto nos permite usarla en diferentes aplicaciones de potencia, como hacer mover un motor, y tambien es ideal para manejar señales e información. Esto nos indica, pues, que tenemos dos formas de electricidad: estática y dinámica. Nos enfocaremos en esta última, la electricidad dinámica.

La electricidad es el flujo de carga eléctrica a través de un material conductor. Esta carga eléctrica es transportada por electrones, átomos neutros que permiten transportar electricidad. Un poco más técnico: los electrones tienen carga eléctrica negativa y se mueven en respuesta a un campo magnético.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="https://i.imgur.com/mPmpCay.png" class="img-fluid">
    </div>
</div>

La electricidad, como flujo de carga eléctrica, es el movimiento promedio de todos estos electrones. En realidad, los electrones no se mueven en línea recta como en la imagen, sino que se mueven en todas las direcciones, pero de manera promedio se mueven en una misma dirección. Algunos golpean con los límites del conductor y esto hace que pierdan energía. Recordemos tambien que la energía no se crea ni se destruye, sino que se transforma. Por lo cual la energía perdida del electrón se convierte en otras formas de energía y una consecuencia indirecta de este efecto, es que podemos almacenar y procesar información usando electrones. Finalmente, hay que tener en cuenta que la dirección de la electricidad, por convención, fluye en dirección contraria al flujo de electrones.

Una analogía para entender mejor la electricidad es el agua en un río. Este se mueve de un punto más alto a un punto más bajo. Mientras mayor sea la altura, mayor energía potencial tiene el agua y, a medida que va bajando, va perdiendo energia. Asimismo, el flujo de electricidad va desde un punto de potencial más alto a un punto de potencial más bajo. El **voltaje**, o potencial, es la energia que tiene un portador de carga y son más capaces de desarrollar trabajo útil. Esta mide en Voltios.

Una batería es usada para almacenar energía y tiene un terminal positivo y uno negativo. El flujo eléctrico se mueve de la terminal positiva (punto de mayor potencial) hacia la terminal negativa (punto de menor potencial), como habíamos mencionado. En la imagen a continuación, podemos ver que la corriente va desde el punto de mayor potencial, pasa por el foco, donde pierde energia, la cual se convierte en calor y luz y finalmente llega al punto de menor potencial.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="https://thumbs.dreamstime.com/z/ejemplo-del-vector-concepto-de-la-corriente-el%C3%A9ctrica-esquema-circular-el%C3%A9ctrico-movimiento-los-electrones-libres-y-%C3%A1tomos-116837847.jpg" class="img-fluid">
    </div>
</div>

Luego que perdió energía y regresa por el terminal negativo, el flujo se vuelve a cargar, se eleva el potencial de energía y vuelve a salir por el terminal positivo.

Entonces, _¿qué es la electricidad?_ La electricidad es una forma de energía. Y además es el movimiento de cargas eléctricas a través de un circuito.

## Energia y Potencia

La energía en un circuito es una propiedad física que podemos medir y que describe el estado de un sistema, o de un objeto, o de un circuito. Existen varias formas de medir la energia, por ejemplo el _joule_, tambien esta el conteo de _calorías_. Lo cual nos lleva a **potencia**. La potencia es la velocidad a la cual la energía va cambiando, ya sea convirtiéndose desde un sistema a otro, transformándose entre diferentes formas de energía, siendo consumida o generada. La unidad de medida de potencia en un circuito electrico es el _watt_.

Es posible calcular la potencia de un componente de un circuito electrónico como el producto entre voltaje y corriente, que es la forma más general. La corriente en un componente por el voltaje en sus terminales me da la potencia que está siendo consumida o entregada por ese componente y la energía es la acumulación de esa potencia en el tiempo.

La energía eléctrica puede ser almacenada en baterías y en otros medios. Hay diversas opciones de batería, están las baterías alcalinas (1.5 Voltios por celda), de níquel-hidruro metálico (1.2 Voltios por celda), de plomo acido (6 o 12 Voltios, estos son los que se encuentran en carros) y de ión de litio o polímero de litio (3.7 Voltios por celda). Éstas son las que se utilizan, por ejemplo, en muchos teléfonos celulares o computadores.

## Capacidad

La capacidad de una batería se mide en miliamperes-hora, **mAh**. Amperes es la unidad con la que se mide la corriente. Entonces la capacidad es una unidad de corriente multiplicada por tiempo Si multiplicamos amper por tiempo por volt, volt por amper me da potencia, y potencia por tiempo me da energía, perfecto.

Entonces, un miliamper-hora (1 mAh) significa que una batería es capaz de entregarme un miliamper durante una hora, o medio miliamper durante 2 horas o 2 miliamper durante media hora.

Algo importante tambien a tener en cuenta es como se conectan las baterias.

<div class="row">
    <div class="col-md-4 offset-md-4">
        <img src="http://1.bp.blogspot.com/-6FMsupTeGqw/VLKoFRarVXI/AAAAAAAAAYA/q4aZfC5cn44/s1600/Battery%2BConnections-%2Bparallel%2Bfor%2Bhigh%2Bcurrent%2Band%2Bseries%2Bfor%2Bhigh%2Bvoltage.jpg" class="img-fluid">
    </div>
</div>

Las baterías en serie suman su voltaje manteniendo su capacidad. En cambio, en paralelo, las baterías mantienen su voltaje pero su capacidad aumenta. Asumamos que las baterías de la imagen anterior que son de 12 Voltios tienen una capacidad de 1000 miliamper-hora, 1 amper-hora. Al conectarlas en paralelo, mantienen el voltaje pero la capacidad aumenta de 1 amper-hora a 3 amper-hora. Por otro lado, al conectarlas en serie el voltaje aumentaría a 36 Voltios con la misma capacidad de 1 amper-hora.

**IMPORTANTE**: Nunca conecten baterías en serie que tengan capacidades diferentes porque unas se van a descargar primero que otras y pueden provocar problemas. Nunca conecten en paralelo baterías que tengan voltajes diferentes porque van a provocar un cortocircuito.

## Implementación

Apliquemos este conocimiento a un proyecto de electrónica. En particular, yo tengo una placa desarrollo Feather Huzzah EPS8266. Según sus especificaciones este tiene un consumo de 67 mA con picos de 250 mA, aunque realmente depende de lo que se este usando. Usaremos un [valor promedio](https://diyi0t.com/how-to-reduce-the-esp8266-power-consumption/) de 70 mA. Si lo conectamos con una batería de 3.7 Voltios con una capacidad de 500 mAh podemos tener una duración de 7 horas (500 mAh/70mA = 7.14 h) aproximadamente, asumiendo que usamos la batería al 100% de efectividad, lo cual sabemos que no es real. Digamos que tenemos un 70% de efectividad, eso nos reduce la duracion a 5 horas (500 / 70 * 0.7 = 5). Ok, lo admito, estoy obviando ciertos parametros, como por ejemplo el modo de consumo de energía del modulo, pero esto es otro tema.

Otro ejemplo, un teléfono móvil. Tomemos el iPhone X. Segun [estas especificaciones](https://www.gsmarena.com/apple_iphone_x-8858.php) nos dicen que tiene una bateria de 2716 mAh y tiene un _talk time_ de 21 horas, esto es desactivando todas las diferentes formas de conectividad (WiFi, Bluetooth) y configurar los demas parámetros en el modo de menor consumo (pantalla en bajo brillo, por ejemplo). Veamos cual es el consumo del teléfono. Si dividimos la capacidad para el tiempo de uso (mAh / h = mA) obtendremos el consumo, esto es 129 mA. Pero en el caso de los teléfonos móviles, en realidad, no es un valor fijo ya que el consumo depende de los módulos activos y uso del propietario, aunque existen tecnologías que ayudan a que el teléfono sea más eficiente, en materia de consumo. [Fuente](https://www.electronicdesign.com/technologies/pmics/article/21795444/what-drains-the-smartphone-battery)

## Conclusión

Despues de todo, qué realmente esta sucediendo con las baterias y su duración? La verdad, nada. Todo funciona de manera correcta. El problema es el marketing. Nos dicen que cada nuevo teléfono tiene una batería de mayor capacidad, lo cual es verdad. Pero el consumo tambien aumenta ya que se implementan nuevas tecnologías, lo cual nos lleva a un empate técnico. Aumenta la capacidad, aumenta el consumo, se mantiene o disminuye la duración. Por otra parte, si usaramos una batería de hoy en día en un telefono de bajo consumo (uno de hace 10 años, con menos tecnologías) entonces veríamos mayor duración y nos sorprenderían de verdad con el marketing de la capacidad de la batería.

