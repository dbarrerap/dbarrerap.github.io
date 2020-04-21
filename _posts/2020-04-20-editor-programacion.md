---
title: Editores de Texto para Programacion
author: David E. Barrera
date: 2020/04/20
tags: ['programacion', 'novatos', 'editor de texto']
comments: true
layout: post
---
Una de las mas grandes incógnitas al iniciar a programar es: "Que debo usar para programar?"
Y esto depende mucho del lenguaje de progamación a usar. 

C# usas Visual Studio o Xamarin.Java, usas NetBeans o Eclipse. Pero estos son lenguajes compilados. 
En los interpretados, el paisaje es un poco mas amplio y flexible. JavaScript, Python, Ruby no 
tienen una herramienta predeterminada. Sí, sé que existe [JetBrains](https://www.jetbrains.com/),
pero ya vamos a llegar a ellos.

Como mi lenguaje de preferencia es [Python](https://www.python.org/), me enfocaré en este, aunque
las herramientas que mostraré aqui, en su mayoría, son adaptables a cualquier lenguaje, inclusive
los compilados.

## IDLE

IDLE es el ambiente de desarrollo y aprendizaje integrado que viene con la instalacion del
lenguaje mismo. Los beneficios de usar IDLE es su simplicidad lo cual permite el aprendizaje
del lenguaje de manera sencilla y efectiva. Sus principales caracteristicas son:

* Editor de texto multi-ventana con resaltado de sintaxis, autocompletado, margenes automaticos
* Debugger integrado paso a paso(stepping), puntos de pausa (breakpoints) y visualizacion de stack

## Visual Studio Code

En esencia, VS Code es un editor de codigos y siendo asi adopta caracteristicas comunes, tales como:

* Editor, el area principal para editar los archivos. Se pueden abrir multiples pestañas y organizarlas
de manera horizontal o vertical.
* Barra Lateral, contiene diferentes utilidades como un explorador de archivos, buscador de texto,
administrador de versiones (integracion con Git) y mas.
* Barra de Estado, donde se muestra informacion adicional del archivo siendo editado.
* Paneles, donde se puede mostrar diferentes tipos de informacion de ejecucion o debugging, errores
y advertencias o una terminal integrada.
* Extensiones. La funcionalidad del editor se puede mejorar mediante extensiones, descargables
desde la misma aplicacion.

Estas caracteristicas permiten tener mayor productividad al manejar un proyecto maduro, donde 
se deben manejar varios archivos a la vez. Ademas incluye tecnologias de autocompletado e 
[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), propio de los productos 
Microsoft. Ayuda rapida, detecta errores en el script y muestra posibles correcciones.

Como se puede ver, ya no es un ambiente de aprendizaje, sino de desarrollo rapido. 
**No lo recomiendo para alguien que esta aprendiendo el lenguaje,** pero si a alguien que ya
tiene cierto dominio sobre el lenguaje y trabaje con proyectos pequeños y medianos.

## PyCharm

PyCharm es el sueño de todo programador. De hecho, cualquier IDE desarrollado por JetBrains
es el sueño de cualquier programador. Pero para llegar a usar PyCharm debemos responder la
pregunta: Que caracteristica me obliga a usar un IDE grande y pesado, con una licencia que
se debe pagar mensual o anualmente? Quiza no haya una unica respuesta a esto, pero si una
suma de respuestas. Ademas, todas las caracteristicas se convierten en beneficio cuando se
tome el tiempo de aprenderlas y sacar provecho y no sean una molestia. Veamos.

* Autocompletado. Si, otra vez esta misma caracteristica. Aunque PyCharm no solo hace
autocompletado del lenguaje, sino tambien de modulos integrados y personalizados. El 
valor agregado es que tambien indica los parametros que aceptan las funciones/metodos y
el valor de respuesta.
* Autocompletado para bases de datos. Una de las caracteristicas avanzadas es que podemos
tener acceso a la base de datos del back-end y, durante la generacion de consultas, da sugerencias
de autocompletado (campos, tablas, etc).
* Visualizacion de Git. El editor resalta los cambios realizados, y permite visualizar lo que
estaba escrito antes de los cambios.
* Administracion de paquetes. Si, es verdad, unos sabemos usar <code>pip</code>. Pero una
referencia visual siempre es buena, en especial si muestra si los paquetes instalados estan
al dia, y la habilidad de buscar paquetes nuevos tambien es bienvenida.
* Refactorizacion. La habilidad de hacer cambios de manera consistente y de manera segura
a traves de los cientos de archivos ya que PyChram entiende el arbol sintactico, a diferencia
de solo reemplazo de texto.
* Herramientas para bases de datos. Ya sea que se trabaje con MS SQL, MySQL, Oracle, PostgreSQL,
u otros motores de bases de datos, PyCharm nos permite acceso directo a las bases, editar las 
sentencias, ejecutar consultas, navegar por datos y alterar esquemas.
* Consola interactiva. Se puede ejecutar consolas interactivas Python o Django dentro de PyCharm, 
lo que presenta muchas ventajas sobre las estándar: revisión de sintaxis sobre la marcha con 
inspecciones, emparejado automático de llaves, paréntesis y comillas y, por supuesto, finalización 
de código. Ambas consolas funcionan con intérpretes locales y remotos.
* Terminal Integrado. Esto es lo que hace a PyCharm un IDE Python integral. Ya no es necesario 
salir del IDE mientras desarrolla. El terminal local está disponible para Windows, Linux y Mac OS.
* Interprete Remoto. Usar un intérprete Python remoto en vez de uno local le permite ejecutar, 
depurar y perfilar su aplicación en un entorno similar al de producción o uno de pruebas, 
ya sea el servidor real o uno virtualizado creado con Vagrant o Docker.
* Docker. PyCharm integra Docker, una popular plataforma abierta para aplicaciones distribuidas 
para desarrolladores y administradores de sistemas.

Y hay muchas otras caracteristicas por nombrar.

Muchas veces veo que recomiendan PyCharm, tan solo porque tiene TODAS estas capacidades. Pero,
realmente se necesita tanto poder para un <code>hello, world</code>? En realmente necesario
un IDE con tantas herramientas para sencillos scripts?

Estos no son los unicos IDEs que existen para programar en Python, existen tambien:

* Eclipse + Pydev. IDE con extension de compatibilidad.
* Sublime Text. Editor de texto especializado.
* VIm. Editor de texto en consola.
* Atom. Al igual que Sublime Text, es un editor de texto especializado.
* Spyder. Este IDE integra paquetes de uso cientifico.
* Thonny. Otro IDE sencillo y ligero, recomendado para principiantes.
* Anaconda. IDE especializado para analisis de datos.

Cuando esta pregunta aparece, siempre recomiendo empezar desde lo mas basico. Lo importante
al iniciar en la programacion es dominar el lenguaje. Luego, una vez dominado el lenguaje, 
sacar la artillería pesada. Pero como todo en la vida, es decision del programador las herramientas
a usar.
