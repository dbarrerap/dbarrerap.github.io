---
title: Creando una aplicaci&oacute;n en Python con guizero
date: 2020/05/03
category: programming
tags: python, gui, development
---
En este tutorial van a aprender a hacer una interfaz sencilla usando Python.

A quien haya escrito aplicaciones en Python anteriormente, es probable que en su mayor parte las entradas y salidas hayan sido de manera textual en una pantalla negra llamada ```consola```. Añadir una interfaz grafica al programa ayuda al usuario a interactuar con ella mediante botones, menus, cuadros de texto y otros controles con los que el usuario final ya esta acostumbrado.

## Libreria

Para este tutorial usaremos una nueva libreria llamada [```guizero```](https://lawsie.github.io/guizero/). Esta libreria se puede instalar de varias maneras, mediante la descarga desde la pagina de ellos o mediante el administrador de paquetesde Python, ```pip```.

```bash
$ pip3 install guizero
```

Python viene con ```tkinter``` instalado de manera predeterminada, pero a veces suele ser muy complejo su uso. El objetivo de ```guizero``` es justamente simplificar su uso.

## Nuestra primera aplicación

Como es costumbre, iniciemos con un ```Hola mundo!```:

```python
from guizero import App

app = App(title='Hola mundo!')
app.display()
```

Guardamos y ejecutamos. Deberia aparecer una pantalla en blanco con el titulo ```Hola mundo!```

<div class="row"><div class="col-md-4 offset-md-4">
<img src="/assets/ScreenShot2020-05-02_1837.png">
</div></div>

## Añadir widgets

Ahora, añadamos contenido a nuestra ventana. Los elementos, tales como texto, botones, cuadros de texto, se denominan ```widgets```. Para hacer uso de ellos, debemos importarlos. Para esto, modificamos la linea de importación. Haremos esto a lo largo del presente tutorial.

```python
from guizero import App, Text
```

Todo el codigo que crea widgets debe ser escrito antes de la instrucción ```app.display()```.

```python
from guizero import App, Text
# Escribir el codigo que agrega widgets aqui
app.display()
```

La linea ```app.display()``` inicia un bucle en el cual constantemente está esperando un clic en un boton o que algun cambio se haya dado, llamado ```evento```, y automaticamente actualizar la pantalla respectivamente. Este bucle bloquea cualquier codigo que se escriba despues, pensar como un loop infinito.

### Text

Probablemente es el widget mas comun y simple, lo que hace es mostrar un texto en la ventana.

Agregar ```Text``` a la sentencia de importación y agregar el widget a la interfaz.

```python
mensaje = Text(app, text='Mi primera app')
```

Aqui hemos creado un widget Text con el nombre ```mensaje```. El primer argumento le dice al widget quien es el elemento padre, en este caso la ventana principal, la cual habíamos creado anteriormente.

<div class="row"><div class="col-md-4 offset-md-4">
<img src="/assets/ScreenShot2020-05-02_2158.png">
</div></div>

Debería verse como en la imagen arriba. Notas que le dijimos al widget ```Text``` el texto a mostrar mediante ```text='Mi primera app'```? Esto es denominado un _argumento clave_, porque se ha especificado la clave ```text``` y el valor, o mas bien texto, a mostrar. Existen otros argumentos clave, los cuales se pueden agregar separándolos por comas.

```python
mensaje = Text(app, text='Mi primera app', size=36, font="Times New Roman", color="#003b6f")
```

Leyendo [la documentación](https://lawsie.github.io/guizero/text/), podemos ver toda la lista de argumentos que pueden usarse, asi como sus posibles valores. En el ejemplo he usado ```size``` para el tamaño, ```font``` para la tipografia, la cual depende de la disponibilidad en el sistema, y por ultimo ```color```, pero no todos los colores pueden ser definidos por su nombre por lo cual tambien acepta un color usando su valor en hexadecimal, como lo he hecho en el ejemplo.

### TextBox

El widget TextBox es usado para que el usuario pueda ingresar datos (como el metodo ```input()```). Agreguemos uno a nuestra aplicación.

Agreguemos el widget a la sentencia de importación.

```python
from guizero import App, Text, TextBox
```

Y ahora agregar el widget a la pantalla.

```python
nombre = TextBox(app)
```

Hay un argumento clave llamado ```width``` el cual permite hacer mas ancho en campo de entrada. Consultar la [documentación](https://lawsie.github.io/guizero/textbox/) para ver los demas argumentos claves.

### PushButton

```PushButton``` nos permite crear un boton. Cuando se presiona el boton, este llama a una función.

Para su correcto funcionamiento, la función a llamar debe estar creada previo a la creación de los widgets.

```python
def recibir_nombre():
    mensaje.value = nombre.value
```

La propiedad ```.value``` nos permite obtener y establecer el valor del objeto.

A continuación, agregamos ```PushButton``` a nuestra sentencia de importación. Ahora, agrega el siguiente codigo para agregar el boton a nuestra interfaz.

```python
actualizar_texto = PushButton(app, command=recibir_nombre, text='Escribir mi nombre')
```

El primer argumento le dice al boton quien es su elemento padre, esto es común entre todos los widgets. A continuación usamos argumentos clave: ```command``` le dice al boton cual función ejecutar cuando se presiona y ```text``` es el texto a mostrar en el boton. Guardamos y ejecutamos. Escribimos nuestro nombre en el campo de texto y presionamos el boton.

<div class="row"><div class="col-md-4 offset-md-4">
<img src="/assets/ScreenShot2020-05-03_1215.png">
</div></div>

Como ven en el ejemplo, al presionar el boton, se actualiza el texto con el texto escrito.

### Slider

Un ```Slider```, o deslizador, nos permite escoger facilmente un valor dentro de un rango de valores, como un deslizador de volumen.

Asi como el boton, debemos crear la función que es llamada al mover el deslizador previo a la creación del widget, esta función es llamada ```callback```.

```python
def cambiar_tamano(valor):
    mensaje.text_size = valor
```

Esta función tiene un parametro llamado ```valor```. Cuando el deslizador se mueva, el valor seleccionado es transmitido a la función ```cambiar_tamano()``` el cual recibe este valor. El codigo de la función lo que hace es cambiar el tamano del widget ```Text``` de acuerdo al valor del deslizador.

A continuacion, agregamos el ```Slider``` a la sentencia de importación y agregamos el widget a la interfaz.

```python
tamano = Slider(app, command=cambiar_tamano, start=10, end=40)
```

La función ```cambiar_tamano()``` es llamado cada vez que el deslizador se mueve. El rango esta dado entre los valores de ```start``` y ```end```, el valor minimo y maximo, respectivamente. Guardamos y ejecutamos. Como podras ver, el widget ```Text``` ahora tiene el valor seleccionado en el ```Slider```, y si movemos este ultimo, el tamaño del texto se actualiza de acorde al valor en el deslizador.

### Picture

El widget ```Picture``` nos permite incluir imagenes a nuestra aplicacion. Dependiendo del sistema operativo, diferentes formatos son soportados. De manera predeterminada los formatos aceptados son PNG y GIF, excepto en macOS en el cual solo GIF es soportado. Para agregar soporte a imagenes JPG, GIF animados, entre otros, debemos instalar ```guizero[images]```.

```bash
$ pip3 install guizero[images]
```

Muy bien, ahora pondremos una imagen junto a nuestra aplicación. Tambien puede estar en una sub-carpeta, en mi caso creare una carpeta llamada ```imagenes``` y pondre una imagen de un Raspberry Pi dentro.

Ahora, agregamos ```Picture``` a la sentencia de importación y agregamos el widget a la interfaz.

```python
imagen = Picture(app, image='imagenes/raspberry-pi.jpg', width=240, height=120)
```

Tambien se puede modificar el tamaño de la imagen usando los argumentos clave ```width``` y ```height```. Consultar la [documentacion](https://lawsie.github.io/guizero/picture/) para mas información.

Al ejecutar el codigo, nuestra aplicación debe verse de la siguiente manera:

<div class="row"><div class="col-md-4 offset-md-4">
<img src="/assets/ScreenShot2020-05-03_1501.png">
</div></div>

## Codigo Completo

```python
from guizero import App, Text, TextBox, PushButton, Slider, Picture

def recibir_nombre():
    mensaje.value = nombre.value

def cambiar_tamano(valor):
    mensaje.text_size = valor

app = App(title='Hola mundo!')
mensaje = Text(app, text='Mi primera app', size=36, font='Times New Roman', color="#003b6f")
nombre = TextBox(app, width="32")
actualizar_texto = PushButton(app, command=recibir_nombre, text='Escribir mi nombre')
tamano = Slider(app, command=cambiar_tamano, start=10, end=40)
imagen = Picture(app, image='imagenes/raspberry-pi.jpg', width=240, height=120)

app.display()
```

## Conclusion

En este tutorial hemos creado una aplicacion simple con ciertos elementos bastante comunes, como lo son las etiquetas (```Text```), entradas de texto (```TextBox```), botones (```PushButton```), deslizadores (```Slider```) e imagenes (```Picture```). En otra entrada haremos uso de controles un poco mas complejos como un menu, checkbox y radio button. 
