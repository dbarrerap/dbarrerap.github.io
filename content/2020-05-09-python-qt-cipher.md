---
title: Cifrado Cesar con Python + PyQt5
date: 2020/05/09
tags: python, qt5, gui, development, seguridad, criptografia
category: programming
---
En criptografía, el cifrado César, también conocido como cifrado por desplazamiento, código de César o desplazamiento de César, es una de las técnicas decodificación más simples y más usadas. Es un tipo de cifrado por sustitución en el que una letra en el texto original es reemplazada por otra letra que se encuentra un número fijo de posiciones más adelante en el alfabeto.

El cifrado César muchas veces puede formar parte de sistemas más complejos de codificación, como el cifrado Vigenère, e incluso tiene aplicación en el sistema ROT13. Como todos los cifrados de sustitución alfabética simple, el cifrado César se descifra con facilidad y en la práctica no ofrece mucha seguridad en la comunicación, pero si eres el primero en implementarlo en el mundo, es bastante seguro!

Para estar claros, a continuación mostraré como encriptar el texto ```CODING``` con un desplazamiento de ```1```.

<table class="table">
    <thead>
        <tr><th>texto</th><th>C</th><th>O</th><th>D</th><th>I</th><th>N</th><th>G</th></tr>
    </thead>
    <tbody>
        <tr><th>desplazamiento</th><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr>
        <tr><th>cifrado</th><td>D</td><td>P</td><td>E</td><td>J</td><td>O</td><td>H</td></tr>
    </tbody>
</table>

De manera formal, si tenemos un texto _p_, _p<sub>i</sub>_ es el _i_ caracter en _p_, y _k_ siendo la clave (un numero no negativo), entonces cada letra _c<sub>i</sub>_ en el texto cifrado, _c_, se lo calcula de la siguiente manera:

> **c<sub>i</sub> = (p<sub>i</sub> + k) % 26**

Donde ```% 26``` es el residuo de la división para 26. Quizá lo estoy complicando un poco más de lo necesario, pero es una manera precisa de plasmar el algoritmo.

## Implementación

Vamos a escribir un programa llamado ```cesar-qt.py``` que nos permite encriptar un texto usando el cifrado César. Iniciemos entonces creando la interfaz del programa. Inicialmente necesitamos ingresar el texto plano, seleccionar un valor numérico y mostrar el texto cifrado.

Usaremos el conocido sistema de filas y columnas, proveniente de [Bootstrap](https://getbootstrap.com/docs/4.4/layout/grid/), para ubicar nuestros widgets.

```python
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtCore import Qt


class MainWindow(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Cifrado Cesar')
        center_widget = QWidget()

        # Nuestra columna principal
        container = QVBoxLayout(center_widget)

        # Primera fila
        row_1 = QHBoxLayout()

        # Primera columna de la primera fila
        vbox_1 = QVBoxLayout()
        lbl_plaintext = QLabel('Texto plano')
        vbox_1.addWidget(lbl_plaintext)
        txt_plaintext = QLineEdit()
        txt_plaintext.setPlaceholderText('Escriba un texto...')
        vbox_1.addWidget(txt_plaintext)
        vbox_1.setAlignment(Qt.AlignVCenter)
        row_1.addLayout(vbox_1)
       
        # Segunda columna de la primera fila
        vbox_2 = QVBoxLayout()
        lbl_rotation = QLabel('Rotacion')
        vbox_2.addWidget(lbl_rotation)
        cmb_values = QComboBox()
        cmb_values.addItems([str(x) for x in range(1, 26)])
        vbox_2.addWidget(cmb_values)
        vbox_2.setAlignment(Qt.AlignVCenter)
        row_1.addLayout(vbox_2)

        # Segunda fila
        row_2 = QHBoxLayout()  

        # Primera columna de la segunda fila
        vbox_3 = QVBoxLayout()
        lbl_ciphertext = QLabel('Texto cifrado')
        vbox_3.addWidget(lbl_ciphertext)
        self.txt_ciphertext = QLineEdit()
        self.txt_ciphertext.setReadOnly(True)
        vbox_3.addWidget(self.txt_ciphertext)
        row_2.addLayout(vbox_3)
        
        # Agregamos las filas a nuesta columna principal
        container.addLayout(row_1)
        container.addLayout(row_2)

        # Ubicamos el contenido en nuestra ventana
        self.setCentralWidget(center_widget)
        self.show()


if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow()
    sys.exit(app.exec_())
```

Donde obtendremos una ventana como la siguiente:

<div class="row"><div class="col-md-4 offset-md-4">
<img src="{static}/images/ScreenShot2020-05-09_1353.png" class="img-fluid">
</div></div>

Es interesante ver esta línea ```cmb_values.addItems([str(x) for x in range(1, 26)])```, aquí hacemos que Python cree para nosotros una lista de numeros del 1 al 25 usando una técnica llamada ```list comprehension```. Por qué? En un abecedario internacional, tenemos 26 letras. Si usamos el número 26, le damos la vuelta al abecedario y es lo mismo que ingresar el numero 0, por lo cual no habría desplazamiento.

Vamos a usar una señal de ```QLineEdit``` llamado ```textChanged```, que detecta cada cambio en el campo de texto. De esta manera aplicaremos el cifrado cada vez que ingresemos una letra. Asimismo, usaremos la señal ```currentIndexChanged``` de ```QComboBox``` para realizar el mismo efecto cuando seleccionemos un nuevo valor de Rotacion. Pero primero crearemos el slot ```caesar()``` quien se encargará de realizar la encriptación.

```python
class MainWindow(QMainWindow):
    def __init__(self):
        ...

    def caesar(self, texto, rotacion):
        cifrado = ''
        for p in texto:
            if p.isalpha():
                c = ord(p) + int(rotacion)
                if (p.isupper() and (c < 65 or 90 < c)) or (p.islower() and (c < 97 or 122 < c)):
                        c -= 26
                    cifrado += chr(c)
                else:
                    cifrado += p
        self.txt_ciphertext.setText(cifrado)
```

Repasemos lo que sucede. Por cada _p_ del texto plano, chequeamos si es letra, pues solo aplicaremos el cifrado a letras del abecedario, no caracteres especiales (números o signos de puntuacion). Convertimos el caracter _p_ en su valor ordenado (```ord()```), segun la tabla ascii, y le sumamos la clave para obtener el caracter _c_ del cifrado. Luego chequeamos si el caracter cifrado esta dentro del rango del abecedario segun sus valores ascii (esto es ```a``` = 65, ..., ```z``` = 90, ```A``` = 97, ..., ```Z``` = 122), si esta fuera del rango, le restamos 26 (le damos la vuelta al diccionario, por asi decirlo) y lo agregamos al texto cifrado, conviertiendolo en letra (```chr()```).

Si es un caracter especial, simplemente lo agregamos al texto cifrado sin convertirlo. Finalmente, mostramos el texto cifrado en el segundo campo de texto ```txt_ciphertext```.

A continuación, asociamos este método con las señales previamente mencionadas y tenemos nuestro programa de encriptación con cifrado César.

##  Código completo

```python
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtCore import Qt


class MainWindow(QMainWindow):
    def __init__(self):
        QMainWindow.__init__(self)
        self.setWindowTitle('Cifrado Cesar')
        center_widget = QWidget()

        container = QVBoxLayout(center_widget)

        row_1 = QHBoxLayout()  

        vbox_1 = QVBoxLayout()
        lbl_plaintext = QLabel('Texto plano')
        vbox_1.addWidget(lbl_plaintext)
        txt_plaintext = QLineEdit()
        txt_plaintext.setPlaceholderText('Escriba un texto...')
        txt_plaintext.textChanged.connect(lambda: self.caesar(txt_plaintext.text(), cmb_values.currentText()))
        vbox_1.addWidget(txt_plaintext)
        vbox_1.setAlignment(Qt.AlignVCenter)
        row_1.addLayout(vbox_1)

        vbox_2 = QVBoxLayout()
        lbl_rotation = QLabel('Rotacion')
        vbox_2.addWidget(lbl_rotation)
        cmb_values = QComboBox()
        cmb_values.addItems([str(x) for x in range(1, 26)])
        cmb_values.currentIndexChanged.connect(lambda: self.caesar(txt_plaintext.text(), cmb_values.currentText()))
        vbox_2.addWidget(cmb_values)
        vbox_2.setAlignment(Qt.AlignVCenter)
        row_1.addLayout(vbox_2)

        row_2 = QHBoxLayout()  
        
        vbox_3 = QVBoxLayout()  
        lbl_ciphertext = QLabel('Texto cifrado')
        vbox_3.addWidget(lbl_ciphertext)
        self.txt_ciphertext = QLineEdit()
        self.txt_ciphertext.setReadOnly(True)
        vbox_3.addWidget(self.txt_ciphertext)
        row_2.addLayout(vbox_3)
        
        container.addLayout(row_1)
        container.addLayout(row_2)

        self.setCentralWidget(center_widget)
        self.show()

    def caesar(self, texto, rotacion):
        cifrado = ''
        for p in texto:
            if p.isalpha():
                c = ord(p) + int(rotacion)
                if (p.isupper() and (c < 65 or 90 < c)) or (p.islower() and (c < 97 or 122 < c)):
                        c -= 26
                    cifrado += chr(c)
            else:
                cifrado += p
        self.txt_ciphertext.setText(cifrado)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow()
    sys.exit(app.exec_())
```

## Conclusión

Si, es posible que me esté obsesionando con PyQt5, pero después de años de haber usado Python en linea de comandos y ahora poder darle una interfaz gráfica es espectacular.

Este programa de cifrado es uno de mis favoritos cuando estoy probando nuevas caracteristicas de un lenguaje de programación, ya que permite varias optimizaciones, bien sea por la implementacion del algoritmo o por la implementacion visual del programa.
