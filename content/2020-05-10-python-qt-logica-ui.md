---
title: Python + PyQt: Separando la logica de la interfaz
date: 2020/05/10
tags: python, qt5, gui, development
category: programming
---
Bienvenidos a una nueva guía de creación de programas usando Python y PyQt5. Hasta ahora, hemos realizado programas sencillos que realizan una o dos operaciones. Hemos revisado varios componentes y sus señales (eventos), usando estos últimos para asociarlos con un comportamiento usando slots (callbacks) generado por nosotros mismos. El problema radica en que todo (la lógica y la interfaz) esta metido en un solo archivo. Esto, en el mundo de la programación, no es muy bien visto. Hoy vamos a conocer como resolver ese problema.

## MVC

El MVC, Model-View-Controller o Modelo-Vista-Controlador, es un patrón de diseño de software que propone separar el código de un programa de acuerdo a sus responsabilidades. Este patrón de diseño se urge ser utilizada en sistemas donde se requiere el uso de interfaces de usuario. Además, permite la creación de "software más robusto con un ciclo de vida más adecuado, donde se potencie la facilidad de mantenimiento, reutilización del código y la separación de conceptos". Para más información, sigue [este enlace](https://desarrolloweb.com/articulos/que-es-mvc.html).

## Implementación

Realicemos un programa sencillo para explicar como funciona.

```python
# demo_ui.py
# Generado por Qt Designer

from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_Dialog(object):
    def setupUi(self, Dialog):
        Dialog.setObjectName("Dialog")
        Dialog.resize(283, 166)
        self.btn_clickme = QtWidgets.QPushButton(Dialog)
        self.btn_clickme.setGeometry(QtCore.QRect(130, 120, 113, 32))
        self.btn_clickme.setObjectName("btn_clickme")
        self.lbl_label = QtWidgets.QLabel(Dialog)
        self.lbl_label.setGeometry(QtCore.QRect(40, 20, 120, 16))
        self.lbl_label.setObjectName("lbl_label")
        self.txt_name = QtWidgets.QLineEdit(Dialog)
        self.txt_name.setGeometry(QtCore.QRect(40, 40, 201, 21))
        self.txt_name.setObjectName("txt_name")
        self.lbl_name = QtWidgets.QLabel(Dialog)
        self.lbl_name.setGeometry(QtCore.QRect(40, 90, 60, 16))
        self.lbl_name.setObjectName("lbl_name")

        self.retranslateUi(Dialog)
        QtCore.QMetaObject.connectSlotsByName(Dialog)

    def retranslateUi(self, Dialog):
        _translate = QtCore.QCoreApplication.translate
        Dialog.setWindowTitle(_translate("Dialog", "Dialog"))
        self.btn_clickme.setText(_translate("Dialog", "Haz clic!"))
        self.lbl_label.setText(_translate("Dialog", "Escriba su nombre"))
        self.lbl_name.setText(_translate("Dialog", "TextLabel"))
```

Esta interfaz fue realizada en Qt Designer, del cual hablaré en otra entrada. Por ahora analicemos lo que tenemos, que no es muy diferente a lo que ya hemos hecho en entradas anteriores. Tenemos la clase ```Ui_Dialog``` que representa una ventana de dialogo. Existen dos métodos, ```setupUi``` el cual se encarga de crear y ubicar los widgets de la interfaz. Podemos ver que este recibe un argumento, el cual es un widget de alto nivel (```Dialog``` en este caso) y todos los widgets creados son hijos de este. El método ```retranslateUi``` permite la traduccion de la interfaz en diferentes idiomas.

Junto a este archivo, crearemos otro llamado ```demo_controller.py``` el cual brindará funcionalidad a la interfaz.

```python
# demo_controller.py

import sys
from PyQt5.QtWidgets import QDialog, QApplication
from demo_ui import *


class MiFormulario(QDialog):
    def __init__(self):
        super().__init__()
        self.ui = Ui_Dialog()
        self.ui.setupUi(self)
        self.ui.btn_clickme.clicked.connect(self.mostrar_mensaje)
        self.show()

    def mostrar_mensaje(self):
        self.ui.lbl_name.setText('Hola, {}'.format(self.ui.txt_name.text()))


if __name__ == '__main__':
    app = QApplication(sys.argv)
    w = MiFormulario()
    w.show()
    sys.exit(app.exec_())

```

Veamos este codigo a profundidad. 

* Hago la importación de módulos necesarios. ```QtWidgets``` es la base de todos los objetos para la interfaz de usuario.
* Creo una nueva clase, ```MiFormulario``` que hereda de la clase base ```QDialog```.
* Genero el método constructor predeterminado (```__init__```) para ```QDialog```.
* El manejo de eventos en PyQt5 usa signals (eventos) y slots (callbacks), como lo he mencionado antes. Cuando se presiona un boton, el evento ```clicked``` se genera. El método ```connect``` conecta un evento con un método, un slot, un callback, en nuestro caso ```mostrar_mensaje```. El bucle de manejo de eventos espera a que un evento ocurra para realizar una tarea. Este sigue funcionando hasta que se llame a ```exit()``` o el widget principal sea destruido (se cierre la ventana).
* Se crea un objeto del tipo ```QApplication```, que es la aplicacion como tal. Este objeto recibe como parametro ```sys.argv``` el cual contiene parametros o atributos enviados via linea de comandos.
* Se crea una instancia de ```MiFormulario``` con el nombre ```w```.
* Usamos el método ```show()``` para mostrar la ventana en pantalla.
* ```mostrar_mensaje()``` es la tarea a realizar cuando el usuario hace clic en el boton. En este caso, muestra el texto 'Hola' junto al nombre que haya escrito el usuario en el campo ```txt_name```.
* ```sys.exit()``` asegura que, al cerrar la ventana, se liberen los recursos de la memoria.

## Conclusión
Segun Gamma et al. "**MVC** consta de tres tipos de objetos. El _Modelo_ es el objeto de la aplicación, la _Vista_ es su presentación en pantalla y el _Controlador_ define la forma en que la interfaz de usuario reacciona a la entrada del usuario. Antes de MVC, los diseños de interfaz de usuario tendían a agrupar estos objetos. MVC los desacopla para aumentar la flexibilidad y la reutilización."

En otras palabras, el componente modelo se encarga de mantener la persistencia de los datos, guardando o recuperando la información independiente de medio utilizado. La vista representa los componentes que muestran la interfaz de la aplicación, mostrando la información obtenida a partir del modelo, de manera que el usuario pueda visualizarla. Finalmente, el controlador representa los componentes que se encargan de la interacción del usuario, actuando de intermediario entre el usuario, el modelo y la vista. El controlador recoge las peticiones del usuario, interacciona con el modelo y finalmente selecciona que vista es la adecuada para mostrar los datos en cuestión.
