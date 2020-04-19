---
title: Python - Ambientes Virtuales
layout: post
date: 2019-10-03 00:00:00 -0500
comments: true
---
Creo que podemos estar todos de acuerdo que hoy en dia Python es el mejor lenguaje
de programacion. Altamente modular, puede ser usado para proyectos basicos de calculo
hasta para [tomar la foto de un agujero negro].

Con el pasar del tiempo suelen suceder dos cosas:

1. Las librerias son actualizadas. A veces estas actualizaciones hacen que nuestros programas
sean ejecutados de manera distinta al esperado, y
2. La computadora usada para desarrollo se llena de librerias. Instalamos una libreria para
un solo proyecto y nunca mas la volvemos a usar. Y ahi esta ocupando espacio.

## Que es un ambiente virtual?

Como logramos resolver estos problemas? __Ambientes virtuales__. Segun Jose Domingo Mu&ntilde;oz,
un [ambiente virtual] es:

<blockquote class="blockquote px-5">
un mecanismo que me permite gestionar programas y paquetes python 
sin tener permisos de administración, es decir, cualquier usuario sin privilegios puede tener uno 
o más "espacios aislados" donde poder instalar distintas versiones de programas y paquetes python.
</blockquote>

Esto que quiere decir en espa&ntilde;ol? Que de manera aislada a una instalacion base de Python,
podemos crear carpetas donde se almacenaran las librerias necesarias para ciertos casos particulares
sin afectar la instalacion base, y cuando ya no sean necesarias simplemente eliminamos la carpeta
y todo vuelve a la normalidad. Tambien esto nos permite hacer pruebas de actualizacion sin romper
el funcionamiento de la aplicacion, ya que podemos crear un nuevo ambiente de trabajo, copiar el codigo
hacia este nuevo ambiente y probar si todo funciona de manera correcta, de lo contrario podemos
resolver los problemas que aparezcan.

## Como crear un ambiente virtual?

Para poder crear un ambiente virtual, inicialmente debemos tener `Python` instalado y 
su administrador de paquetes `pip`. Seguros de tener esto, necesitamos una sola libreria
llamada `virtualenv`.

{% highlight bash %}
$ pip install virtualenv
{% endhighlight %}

Si estan usando Python 3, este ya viene con una libreria llamada `venv`, que es mas actualizado.

Una vez instalada la libreria, podemos crear ambientes virtuales. En lo personal me gusta
tener todo en un solo lugar por lo cual vamos a crear una sola carpeta donde vamos a crear nuestros
ambientes virtuales:

{% highlight Bash %}
$ mkdir PythonVirtualEnvs
$ cd PythonVirtualEnvs
{% endhighlight %}

Y aqui vamos a crear nuestro ambiente virtual:

__Python 2__
{% highlight Bash %}
$ virtualenv _env_
{% endhighlight %}
__Python 3__
{% highlight Bash %}
$ python3 -m venv _env_
{% endhighlight %}

Donde _env_ es el nombre del ambiente virtual deseado. Por defecto, un ambiente virtual no importa
ninguna libreria instalada en el sistema. El ambiente virtual creado tiene una estructura similar
a la siguiente:

<pre>
├── bin
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── easy_install
│   ├── easy_install-3.7
│   ├── pip
│   ├── pip3
│   ├── pip3.7
│   ├── python -> python3
│   └── python3 -> /usr/local/bin/python3
├── include
├── lib
│   └── python3.7
│       └── site-packages
└── pyvenv.cfg
</pre>

* `bin` es la carpeta donde se almacenan los ejecutables que interactuan con el ambiente virtual.
* `include` es la carpeta que almacena las cabeceras C para compilar los paquetes de Python
* `lib` una copia de la version actual de Python con la carpeta `site-packages` donde se almacenan
las librerias para el ambiente virtual.

Para trabajar con nuestro ambiente virtual, este debe ser activado:

{% highlight Bash %}
$ source env/bin/activate
(env) $
{% endhighlight %}

Como podran ver, ahora aparece el nombre del ambiente virtual junto a nuestro cursor, esto quiere
decir que nuestro ambiente virtual esta activado correctamente y que no usara las librerias del
sistema. Hagamos una prueba.

{% highlight Bash %}
(env) $ pip install bcrypt
...
(env) $ python -c "import bcrypt; print(bcrypt.hashpw('password'.encode('utf-8'), bcrypt.gensalt()))"
b'$2b$12$429Kxn73i2cZfDX4ct9otO1q4UeRYFJsJsPfVDVPohCU.jSigSRMa'
{% endhighlight %}

Instalamos una libreria en el ambiente virtual, `bcrypt` e hicimos una prueba. Ahora vamos a
desactivar el ambiente virtual y volver a hacer la prueba, para ver que lo que hagamos en el ambiente
virtual no afecta el sistema.

{% highlight Bash %}
(env) $ deactivate
$ python -c "import bcrypt; print(bcrypt.hashpw('password'.encode('utf-8'), bcrypt.gensalt()))"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ImportError: No module named bcrypt
{% endhighlight %}

Aqui vemos que al desactivar el ambiente virtual, la linea de comando cambia, ya no muestra el nombre
del ambiente virtual y al ejecutar el mismo comando nos responde con un error, que no existe el 
modulo `bcrypt`, pues este esta instalado en el ambiente virtual y no en el sistema.

## Conclusion

En este tutorial hemos aprendido que es un ambiente virtual. Tambien hemos visto como instalar 
`virtualenv`, crear un ambiente virtual, activarlo y finalmente desactivarlo.


[tomar la foto de un agujero negro]: http://www.blog.pythonlibrary.org/2019/04/11/python-used-to-take-photo-of-black-hole/
[ambiente virtual]: https://openwebinars.net/blog/entornos-de-desarrollo-virtuales-con-python3/