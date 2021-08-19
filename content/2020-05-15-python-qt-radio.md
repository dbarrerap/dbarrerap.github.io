---
title: Python + PyQt5: Radio buttons
date: 2020/05/15
tags: python, qt5, gui, development
category: programming
---
En los últimos dias me encontré con un problema. Yo quería seguir con mi sistema de layout usando ```QHBoxLayout``` y ```QVBoxLayout``` a manera de filas y columnas, pero al intentar hacer una aplicación con ```QRadioButton```, esos que te permiten escoger una sola opcion, me encontre con un problema: Agrupacion.

## Agrupación

Un QRadioButton permite escoger una sola opción de una cantidad _n_ de opciones. Pero qué sucede cuando en la aplicación debo escoger de varios grupos? Por ejemplo un grupo es para Tallas y otro de Pagos, haciendo de ejemplo una aplicación que nos permita comprar ropa. Si estos Radio Buttons no son agrupados, puedo escoger una talla, pero ninguna forma de pago o una forma de pago pero ninguna talla. Esto no es correcto.

Por qué sucede esto? Porque para agrupar los Radio Buttons, estos deben ser hijos de un ```QWidget``` padre, y este Widget padre no puede ser un Layout o la ventana como tal. Ok, comprensible. Por lo cual mi estructura de widgets en la aplicación debería ser de la siguiente manera:

```
QMainWindow  <- Esta ventana de por si es un Widget
|
|- QHBoxLayout  <- Este se convierte en el Layout del Widget padre, no es un Widget
    |
    |- QWidget  <- Tecnicamente, este es el widget hijo de QMainWindow
    |    |
    |    |- QVBoxLayout
    |        |
    |        |- QRadioButton  <- Ellos son hijos del QWidget anterior
    |        |- QRadioButton
    |        |- QRadioButton
    |        |- QRadioButton
    |
    |- QWidget
        |
        |- QVBoxLayout
            |
            |- QRadioButton
            |- QRadioButton
            |- QRadioButton
```

Aquí usamos un QWidget base como agrupador de varios QRadioButton. Esto nos permitirá hacer una selección única de dos grupos de Radio Buttons.

## Implementación

Pues veamos como se escribe la estructura de nuestra Vista.

```python
# view.py
from PyQt5 import QtCore, QtWidgets


class TwoGroups(object):
    def __init__(self, parent):
        self.containerWidget = QtWidgets.QWidget()
        self.container = QtWidgets.QVBoxLayout(self.containerWidget)

        self.horizontalWidget = QtWidgets.QWidget()
        self.row = QtWidgets.QHBoxLayout(self.horizontalWidget)

        self.columnWidget = QtWidgets.QWidget(self.horizontalWidget)
        self.column = QtWidgets.QVBoxLayout(self.columnWidget)

        self.radio_s = QtWidgets.QRadioButton(self.columnWidget)
        self.radio_s.setText('Small')
        self.radio_s.setChecked(True)
        self.column.addWidget(self.radio_s)

        self.radio_m = QtWidgets.QRadioButton(self.columnWidget)
        self.radio_m.setText('Medium')
        self.column.addWidget(self.radio_m)

        self.radio_l = QtWidgets.QRadioButton(self.columnWidget)
        self.radio_l.setText('Large')
        self.column.addWidget(self.radio_l)

        self.row.addWidget(self.columnWidget)

        self.columnWidget_2 = QtWidgets.QWidget(self.horizontalWidget)
        self.column_2 = QtWidgets.QVBoxLayout(self.columnWidget_2)

        self.radio_card = QtWidgets.QRadioButton(self.columnWidget_2)
        self.radio_card.setText('Debit/Credit Card')
        self.radio_card.setChecked(True)
        self.column_2.addWidget(self.radio_card)

        self.radio_transfer = QtWidgets.QRadioButton(self.columnWidget_2)
        self.radio_transfer.setText('Bank Transfer')
        self.column_2.addWidget(self.radio_transfer)

        self.row.addWidget(self.columnWidget_2)
        self.container.addWidget(self.horizontalWidget)

        self.lbl_selected = QtWidgets.QLabel()
        self.container.addWidget(self.lbl_selected)

        parent.setCentralWidget(self.containerWidget)
```

Y el Controlador se mantiene simple.

```python
# controller.py 
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow
from view import *


class Window(QMainWindow):
    def __init__(self):
        super().__init__()
        self.ui = TwoGroups(self)
        self.ui.radio_s.toggled.connect(self.selections)
        self.ui.radio_m.toggled.connect(self.selections)
        self.ui.radio_l.toggled.connect(self.selections)
        self.ui.radio_card.toggled.connect(self.selections)
        self.ui.radio_transfer.toggled.connect(self.selections)
        self.show()

    def selections(self):
        if self.ui.radio_s.isChecked():
            size = 'Small'
        elif self.ui.radio_m.isChecked():
            size = 'Medium'
        elif self.ui.radio_l.isChecked():
            size = 'Large'

        if self.ui.radio_card.isChecked():
            payment = 'Debit/Credit Card'
        elif self.ui.radio_transfer.isChecked():
            payment = 'Bank Transfer'

        text = 'Selected size is {} and payment method is {}'.format(size, payment)
        self.ui.lbl_selected.setText(text)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    w = Window()
    w.show()
    sys.exit(app.exec_())
```

Cuando cambia el estado de un QRadioButton, ```toggled```, el programa chequea el estado, ```isChecked()```, de cada grupo de Radio Buttons y muestra un mensaje de acuerdo a la talla y forma de pago seleccionada.

Eso es todo por hoy. En la proxima entrega les contare acerca de ```slots``` predefinidos, métodos que vienen con los widgets y que nos permitirán escribir aplicaciones de manera más sencilla.
