---
title: Python + PyQt5: Una interfaz mas avanzada
date: 2020/05/07
category: programming
tags: python, qt5, gui, development, requests, API
---
Despues de mi [post]({filename}2020-05-02-python-guizero.md) acerca de usar ```guizero```, me puse a investigar y sacar conclusiones y me di cuenta que ```guizero``` llega hasta cierto punto, luego se mezcla con ```tkinter``` y se vuelve una sola mezcla y un dolor de cabeza. El siguiente paso lo da ```Qt```. Qt es un framework multiplataforma que permite el desarrollo de la interfaz para multiples lenguajes. En nuestro caso lo aplicaremos con Python.

El objetivo de la guia de hoy es crear una aplicacion que permita la conversion de monedas. Los objetivos de aprendizaje son:

- Crear una interfaz con Qt
- Consumir una API via HTTP

## Qt

Como habia mencionado antes, Qt es un framework de desarrollo de aplicaciones multiplataforma para desktop, embebido y móvil. Esto es, desarrollo la interfaz en una plataforma (Linux, por ejemplo) y funcionara de igual manera en todas las otras plataformas (Windows, macOS, iOS, Android, BlackBerry entre otros).

## Qt Designer

Qt Designer es una aplicacion que nos permitira crear una interfaz grafica de manera sencilla y exportarla para ser usada con el lenguaje de programacion de preferencia. Seguir las instrucciones de instalado en [este enlace](https://build-system.fman.io/qt-designer-download).

## Signals + Slots

Qt maneja una nomenclatura diferente a las usuales, pero no es de espantarse. Una señal, ```signal```, es lo que en otros lenguajes se denomina un ```evento```, por ejemplo: ```clicked```, ```valueChanged```.

Por otro lado, las ranuras, ```slots```, se denomina la funcion llamada cuando se emite un evento, tambien llamado un ```callback```.

## Dependencias

Para usar Qt con Python, debemos instalar el paquete ```pyqt5```.

```bash
$ pip3 install pyqt5
```

## Una aplicacion basica

Lo primero a realizar es importar los componentes o ```widgets``` y creamos nuestra ventana.

```python
from PyQt5.QtWidgets import *
import sys

class MiVentana(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Mi Primera App con PyQt')
        self.centralWidget = QWidget()

        # Aqui se agregan los widgets

        self.setCentralWidget(self.centralWidget)
        self.show()

if __name__ == "__main__":
    app = QApplication(sys.argv)
    mainWin = MainWindow()
    mainWin.show()
    sys.exit(app.exec_())
```

Parece mucho, pero esto ayuda a que la ventana mantenga su estado. Lo cual nos beneficia en nuestro ejemplo. Si ejeutamos el codigo, nos debe aparecer una ventana vacia.

Muy bien, ahora haremos algo mas interactivo, un contador! Para esto necesitaremos 2 widgets principalmente, un ```QLabel``` y un ```QPushButton```. Yo incluire una segunda etiqueta y un ```QVBoxLayout``` por cuestiones cosmeticas. Un VBox lo que hace es ubicar los widgets de manera vertical y con cierto espacio entre los componentes y el filo de la ventana.

```python
# counter.py
from PyQt5.QtWidgets import *
import sys

class MiVentana(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Mi Primera App con PyQt')
        self.centralWidget = QWidget()

        self.verticalLayout = QVBoxLayout(self.centralWidget)
        self.lbl_text = QLabel('Has presionado el boton esta cantidad de veces:')
        self.verticalLayout.addWidget(self.lbl_text)
        self.lbl_count = QLabel('0')
        self.verticalLayout.addWidget(self.lbl_count)
        self.btn_button = QPushButton('Presioname!')
        self.verticalLayout.addWidget(self.btn_button)

        self.setCentralWidget(self.centralWidget)
        self.show()

if __name__ == "__main__":
    app = QApplication(sys.argv)
    mainWin = MainWindow()
    mainWin.show()
    sys.exit(app.exec_())
```

Con esto ya tenemos nuestra interfaz, la cual deberia verse de la siguiente manera.

<div class="row"><div class="col-md-4 offset-md-4">
<img src="{static}/images/ScreenShot2020-05-07_1203.png" class="img-fluid">
</div></div>

Al presionar el boton no sucede nada. Por ahora. Arreglemos eso!

```python
# counter.py
from PyQt5.QtWidgets import *
import sys

class MiVentana(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Mi Primera App con PyQt')
        self.centralWidget = QWidget()

        self.contador = 0

        self.verticalLayout = QVBoxLayout(self.centralWidget)
        self.lbl_text = QLabel('Has presionado el boton esta cantidad de veces:')
        self.verticalLayout.addWidget(self.lbl_text)
        self.lbl_count = QLabel('0')
        self.verticalLayout.addWidget(self.lbl_count)
        self.btn_button = QPushButton('Presioname!')
        self.btn_button.clicked.connect(self.contar)
        self.verticalLayout.addWidget(self.btn_button)

        self.setCentralWidget(self.centralWidget)
        self.show()

    def contar(self):
        self.contador += 1
        self.lbl_count.setText(str(self.contador))

if __name__ == "__main__":
    app = QApplication(sys.argv)
    mainWin = MainWindow()
    mainWin.show()
    sys.exit(app.exec_())
```

Perfecto! Ahora tenemos una pequeña app que cuenta cuantas veces hemos dado un clic en el boton. 

<div class="row"><div class="col-md-4 offset-md-4">
<img src="{static}/images/ScreenShot2020-05-07_1420.png" class="img-fluid">
</div></div>

Repasemos lo que hemos visto.

Hemos visto como usar un ```QVBoxLayout``` para organizar nuestros widgets de manera vertical en la aplicacion. Tambien existe un ```QHBoxLayout``` que nos permite organizar de manera horizontal.

Tambien hemos visto como establecer y actualizar el texto en un ```QLabel```, mediante el uso del metodo ```setText()```. Algo muy particular es que solo recibe elementos del tipo String. Por lo cual si es de escribir un numero, hay que convertirlo a texto.

Al crear una clase para manejar el contenido de la ventana, nos da la flexibilidad de manejar su estado, esto es, nos permite actualizar los elementos internos; en este ejemplo en particular, nos permite actualiza el contenido de la etiqueta ```lbl_count```, como lo hemos hecho en el metodo ```contar()```.

Muy bien, ahora a lo que vinimos. Vamos a crear una aplicacion que nos permitira convertir una moneda en otra, por ejemplo de Dolares Americanos a Euros.

```python
from PyQt5.QtWidgets import *
import requests
import sys

class MiVentana(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Convertidor de Moneda')
        self.centralWidget = QWidget()

        self.column = QVBoxLayout(self.centralWidget)
        self.row_1 = QHBoxLayout()
        self.cmb_from = QComboBox()
        self.cmb_from.addItems(['USD', 'EUR', 'GBP', 'CAD'])
        self.row_1.addWidget(self.cmb_from)
        self.txt_from = QLineEdit()
        self.txt_from.setText('1')
        self.row_1.addWidget(self.txt_from)
        self.column.addLayout(self.row_1)
        self.row_2 = QHBoxLayout()
        self.cmb_to = QComboBox()
        self.cmb_to.addItems(['USD', 'EUR', 'GBP', 'CAD', 'JPY', 'NZD', 'HKD', 'MXN'])
        self.cmb_to.setCurrentIndex(1)  # Para escoger un valor diferente
        self.row_2.addWidget(self.cmb_to)
        self.txt_to = QLineEdit()
        self.txt_to.setReadOnly(True)  # Lo usaremos en modo solo lectura
        self.row_2.addWidget(self.txt_to)
        self.column.addLayout(self.row_2)
        self.btn_convert = QPushButton('Convertir')
        self.btn_convert.clicked.connect(lambda: self.convertir(self.txt_from.text(), self.cmb_from.currentText(), self.cmb_to.currentText()))
        self.column.addWidget(self.btn_convert)

        self.setCentralWidget(self.centralWidget)
        self.show()

    def convertir(self, monto, cur_from = None, cur_to = None):
        url = 'https://api.exchangeratesapi.io/latest'
        data = {'base': cur_from, 'symbols': cur_to}
        r = requests.get(url, data)
        if r.status_code == 200:
            json = r.json()
            cambio = float(monto) * json['rates'][cur_to]
            self.txt_to.setText('{:.2f}'.format(cambio))

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MiVentana()
    window.show()
    sys.exit(app.exec_())
```

La interfaz de nuestra aplicacion se ve de la siguiente manera.

<div class="row"><div class="col-md-4 offset-md-4">
<img src="{static}/images/ScreenShot2020=05-07_1421.png" class="img-fluid">
</div></div>

Veamos lo nuevo en este codigo. Aqui hemos usado un nuevo widget llamado ```QComboBox``` que nos permite la seleccion en una lista de items. Para agregar items tenemos dos formas de hacerlo: ```addItem()```, para agregar item por item, y ```addItems()```, para agregar una lista de items. De manera predeterminada se escoge el primer elemento de la lista, pero si deseamos escoger otro elemento usamos el metodo ```setCurrentIndex()``` con un valor valido, esto es menor a la longitud de la lista, ya que el indice en una lista empieza en 0.

Tambien, como debemos proveer un valor monetario para realizar el cambio, usamos un ```QLineEdit``` que nos permite el ingreso de datos via teclado a la aplicacion. De este podemos modificar una propiedad de solo lectura mediante el metodo ```setReadOnly()```, como lo hemos en el segundo campo.

Un detalle adicional es el uso de ```lambda``` al momento de conectar la señal ```clicked``` con el slot ```convertir()```. De manera predeterminada el metodo ```connect()``` solamente acepta la referencia a un metodo, los cual nos bloquea el enviar argumentos al metodo. Como es necesario el envio de argumentos en este caso, ya que debemos enviar las monedas y monto para hacer el cambio, usamos una lambda que nos permite hacer justamente esto [[Fuente](https://eli.thegreenplace.net/2011/04/25/passing-extra-arguments-to-pyqt-slot)].

Finalmente, tenemos el metodo ```convertir()```, cuyos parametros son el monto a convertir, la moneda base y la moneda de conversion. Para realizar la conversion, consultamos con una API, [Exchange Rates](https://exchangeratesapi.io/), el cambio del dia mediante la libreria ```requests```. Usamos el metodo ```get()``` para realizar la consulta pasandole dos argumentos esenciales, la URL a consultar y los parametros de consulta.

```python
url = 'https://api.exchangeratesapi.io/latest'
data = {'base': cur_from, 'symbols': cur_to}
r = requests.get(url, data)
```

Al hacer la consulta, la URL se convierte en ```https://api.exchangeratesapi.io/latest?base=USD&symbols=EUR```, siguiendo el ejemplo de convertir de Dolares Americanos a Euros.

Si existe una respuesta satisfactoria, esto es si el ```status_code``` es igual a 200, toma los datos de la respuesta de la API, realiza el cambio de divisas y la muestra en el segundo campo de texto.

## Conclusion

WOW! Eso fue divertido! Fue mucho por aprender, revisar... Pero como siempre, es de practicar, practicar y practicar...
```guizero``` es un buen sustituto/complemento para ```tkinter```, pero ambos tienen sus complicaciones en cuanto a widgets y ubicacion en pantalla. 

```PyQt```, por otro lado, con ```QVBoxLayout``` y ```QHBoxLayout``` podemos distribuir a manera de columnas y filas los widgets que necesitemos. Adicional, una interfaz Qt es multiplataforma, por lo cual el diseño es consistente entre diferentes sistemas operativos sin perder el estilo. Es verdad, es un poco mas complejo el desarrollo usando este framework, pero hay que pesar tambien sus ventajas.

### Ventajas PyQt

* **Flexibilidad:** Aunque en los ejemplos mostrados no se aplica, se puede separar la logica del programa de la interfaz. El concepto de señales y slots (eventos y callbacks) hace facil esta comunicacion entre objetos.
* **Uso de API:** Qt ofrece una amplia gama de APIs para el uso de redes, bases de datos y mas. Sin necesidad de otras librerias.
* **Componentes nativos:** Como habia mencionado, Qt ofrece muchos widgets diseñados para tener una apariencia nativa en todas las plataformas.
* **Aprendizaje:** PyQt es una de los frameworks mas usados en varios lenguajes, por lo cual documentacion nunca hara falta.

### Desventajas de PyQt

* En cuanto a Python, no existe documentacion especifica para PyQt5.
* Requiere practica y tiempo para entender todos los detalles de Qt, lo cual hace una curva de aprendizaje bastante empinada.
