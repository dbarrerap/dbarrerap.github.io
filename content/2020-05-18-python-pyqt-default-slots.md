---
title: Python + PyQt5: Slots predeterminados
date: 2020/05/18
category: programming
tags: python, qt5, gui, development
summary: En esta entrega vemos un tema mas avanzado para entender mejor como funcionan los widgets y alterar su comportamiento.
---
Hasta ahora hemos visto ciertos widgets y hemos declarado nuestro comportamiento de manera personalizada. Pero Qt no solo provee la apariencia física de los widgets, sino que también provee comportamientos, ```slots```,  predeterminados.

El manejo de eventos es un mecanismo importante en todas las aplicaciones. La aplicación no solo debe reconocer el evento, sino que también debe tomar la acción correspondiente para servir el evento. La acción tomada en cualquier evento determina el curso de la aplicación. Para mostrar esto, desarrollaremos una aplicacion que copie y pegue un texto de un campo de texto en otro.

Esta guía te hará comprender cómo un evento realizado en un widget invoca una acción predefinida en el widget asociado. Debido a que queremos copiar el contenido de un widget ```LineEdit``` al hacer clic en el botón, necesitamos invocar el método ```selectAll()``` cuando se produce el evento ```pressed()``` en el botón. Además, necesitamos invocar el método ```copy()``` cuando se produce el evento ```released()``` en el botón. Para pegar el contenido en el portapapeles en otro widget de ```LineEdit``` al hacer clic en otro botón, necesitamos invocar el método ```paste()``` cuando ocurra el evento ```clicked()``` en otro botón.

## Implementacion

```python
# view.py
from PyQt5 import QtCore, QtWidgets


class CopyPaste(object):
    def __init__(self, parent):
        self.containerWidget = QtWidgets.QWidget()
        self.container = QtWidgets.QVBoxLayout(self.containerWidget)

        self.input_1 = QtWidgets.QLineEdit(self.containerWidget)

        self.container.addWidget(self.input_1)

        self.rowWidget = QtWidgets.QWidget()
        self.row = QtWidgets.QHBoxLayout(self.rowWidget)

        self.copy = QtWidgets.QPushButton(self.rowWidget)
        self.copy.setText('Copy')
        self.row.addWidget(self.copy)

        self.paste = QtWidgets.QPushButton(self.rowWidget)
        self.paste.setText('Paste')
        self.row.addWidget(self.paste)

        self.container.addWidget(self.rowWidget)

        self.input_2 = QtWidgets.QLineEdit(self.containerWidget)

        self.container.addWidget(self.input_2)

        parent.setCentralWidget(self.containerWidget)
```

Hasta aqui, nada nuevo. Una interfaz que contiene dos campos de texto y dos botones, uno para copiar y otro para pegar.

```python
# controller.py
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow
from app import *


class Window(QMainWindow):
    def __init__(self):
        super().__init__()
        self.ui = CopyPaste(self)
        self.ui.copy.pressed.connect(self.ui.input_1.selectAll)
        self.ui.copy.released.connect(self.ui.input_1.copy)
        self.ui.paste.clicked.connect(self.ui.input_2.paste)
        self.show()


if __name__ == '__main__':
    app = QApplication(sys.argv)
    w = Window()
    w.show()
    sys.exit(app.exec_())
```

Aqui podemos ver que conectamos las diferentes señales de los ```QPushButton``` (```pressed```, ```released``` y ```clicked```) con los diferentes slots de los ```QLineEdit``` (```selectAll```, ```copy```, ```paste```). En la [documentación](https://doc.qt.io/qt-5/qlineedit.html#public-slots) podemos ver todos los slots posibles para un QLineEdit. Consultar la documentación nos permite ver que podemos dearrollar nuestras aplicaciones de manera más rápida ya que podemos ver que cada widget tiene funcionalidad predeterminada, solo es de conectarse y hacer uso de ella.

Con esto ya estamos listos para crear nuestro primer CRUD usando PyQt5. Pero eso en la siguiente entrega, hasta entonces!
