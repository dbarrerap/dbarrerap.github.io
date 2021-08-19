---
title: Dart: Una introduccion
date: 2020/06/07
category: programming
tags: dart, development, interpreted language
summary: Una pequeña introduccion al lenguaje Dart antes de usar su framework Flutter para el desarrollo de aplicaciones moviles.
---
Mucho se viene hablando de un framework multiplataforma para dispositivos moviles, Flutter. Pero todo framework esta basado en algun lenguaje de programacion. Laravel con PHP, Django con Python, Rails para Ruby. Asimismo, el lenguaje de programacion de FLutter es **Dart**.

Dart es un lenguaje de programacion desarollado por Google y presentado al mundo en el 2011. El objetivo de Dart, al ser multiplataforma, es dar una alternativa mas moderna al desarrollo de aplicaciones web. Actualmente esta en la version estable es ```2.8.4```. Esta influenciado por otros lenguajes como C, C#, Java, JavaScript, Kotlin, Ruby, Strongtalk, TypeScript y Erlang.

## Instalacion

Dependiendo del sistema operativo, existen diferentes metodos de instalacion. Yo tengo una Mac. Sigue [este enlace](https://dart.dev/get-dart) para consultar el metodo de instalacion de tu sistema operativo.

Para instalar Dart, se debe tener [Homebrew](https://brew.sh/) instalado previamente.

```bash
$ brew tap dart-lang/dart
==> Tapping dart-lang/dart
Cloning into '/usr/local/Homebrew/Library/Taps/dart-lang/homebrew-dart'...

$ brew install dart
```

## Hola, mundo!

Como es usual en una guia de uso de lenguajes de programacion, hagamos un ```Hola, mundo!```:

```dart
// Este es un comentario
void main() {
    print('Hola, mundo!');
}
```

Guardamos esto en un archivo, yo lo llamare ```hello.dart```. Para ejecutarlo, abrimos una terminal (Simbolo de Sistema en Windows) y corremos ```dart hello.dart -c```

```bash
$ dart hello.dart -c
Hola, mundo!
$
```

Como podemos ver, su estructura es similar a la del lenguaje C, ya que requiere una funcion de entrada llamada ```main()``` la cual indica donde la aplicacion debe comenzar. Para mayor informacion, consultar la [documentacion](https://dart.dev/guides/language/language-tour#the-main-function).

## Variables y Funciones

Muy bien, ya sabemos como mostrar informacion al mundo. Ahora pidamos informacion y respondamos acorde. En nuestro siguiente ejemplo pediremos el nombre del usuario y mostraremos un saludo.

```dart
import 'dart:io';

void main() {
    print('Cual es tu nombre?');
    String nombre = stdin.readLineSync();
    saludar(nombre);
}

void saludar(String usuario) {
    print('Hola, $usuario!');
}
```

Revisemos este ejemplo:

* Para poder interactuar con el usuario, necesitamos una libreria [dart:io](https://dart.dev/articles/libraries/dart-io), la cual importamos en la parte superior de nuestra aplicacion.
* Dentro del metodo ```main()```, declaramos una nueva variable del tipo ```String``` llamado ```nombre```. ```stdin.readLineSync()``` nos permite leer la entrada o respuesta del usuario desde la linea de comandos.
* A continuacion, llamamos a una nueva funcion llamada ```saludar()``` enviandole como argumento el nombre que recibimos en el paso anterior.
* La funcion ```saludar()``` acepta como parametro un objeto del tipo String, el cual llamamos ```usuario``` y lo que hace es imprimir un saludo, usando la tecnica de *[interpolacion]*(https://dart.dev/guides/language/language-tour#strings).

Al ejecutar la aplicacion, deberiamos ver algo como esto:

```bash
$ dart hello.dart -c
Cual es tu nombre?
David
Hola, David!
$
```

## Otros datos basicos

Dart tiene 7 [tipos de datos](https://dart.dev/guides/language/language-tour#built-in-types). Veamos un ejemplo con un programa que calcula la cantidad minima de monedas a usar para cierta cantidad a devolver:

```dart
import 'dart:io';


void main() {
    double monto;
    do {
      print('Cual es el valor a devolver?');
      monto = double.tryParse(stdin.readLineSync());  // Convertir de string a double
    } while (monto == null || monto < 0);  // Repetir el bloque mientras una de estas condiciones sea cierta

    // Eliminar posibles valores inherentes de los double
    int monto_int = (monto * 100).round();
    
    int monedas = 0;

    // 25 centavos
    monedas += monto_int ~/ 25;  // Division entera
    monto_int = monto_int % 25;  // Calculamos el residuo

    // 10 centavos
    monedas += monto_int ~/ 10;
    monto_int = monto_int % 10;

    // 5 centavos
    monedas += monto_int ~/ 5;
    monto_int = monto_int % 5;

    // 1
    monedas += monto_int;

    print('Se devuelven $monedas monedas.');
}
```
En este ejemplo tenemos muchas cosas. Hemos usado dos tipos de datos numericos, ```int``` y ```double```, para numeros enteros y decimales respectivamente. Tambien hemos usado un nuevo metodo, ```double.tryParse()```, lo que hace es intentar convertir en un double el argumento, de no poderlo lograr devuelve ```null```. 

Tambien tenemos una [estructura de control](https://dart.dev/guides/language/language-tour#control-flow-statements), la que chequea si monto es null o un valor menor a 0. De ser verdadera una de estas condiciones, repite el bloque de codigo interna, de lo contrario sigue con la ejecucion normal del programa.

Continuando con el codigo, convertimos el valor ingresado en un numero entero. Usamos el metodo ```round()``` para descartar la parte decimal. Tambien usamos operadores matematicos, ```~/``` para que nos devuelva la parte entera de la division y ```%``` para que nos devuelva el residuo de esa division.

## Clases

Podemos crear nuestra propia estructura de datos mediante el uso de clases. Vamos con el ejemplo:

```dart
import 'dart:math';

class Point {
  int _x, _y;

  Point() {
    this._x = 0;
    this._y = 0;
  }

  Point.withCoords(int x, int y) {
    this._x = x;
    this._y = y;
  }

  int getX() => this._x;
  int getY() => this._y;

  void setX(x) {
    this._x = x;
  }

  double distance(Point other) {
    int x_diff = this._x - other._x;
    int y_diff = this._y - other._y;
    return sqrt(x_diff * x_diff + y_diff * y_diff);
  }

  @override
  String toString() {
    return "($_x, $_y)";
  }
}

void main() {
    Point p1 = Point();
    Point p2 = Point.withCoords(5, 3);

    print(p1);
    print(p2);

    print(p1.distance(p2));
}
```

En este ejemplo he creado una clase llamada ```Point``` que representa un punto en el espacio y tiene coordenadas en el eje ```x``` y ```y```. Como buena practica, las propiedades de una clase deben ser privadas, en Dart eso se logra usando el guion bajo, ```_``` en el nombre de la propiedad. Esto puede ser usado tambien en los metodos. 

Dart, a diferencia de otros lenguajes de programacion, no soporta la sobre carga de metodos, pero nos ofrece otras facilidades. Por ejemplo, no podemos tener varios constructores _sin nombre_, pero nos permite crear constructores que tengan un nombre distinto, por asi decirlo. En el ejemplo tenemos el constructor predeterminado, ```Point()``` y otro constructor con nombre, ```Point.withCoords()```, el cual acepta valores para los ejes.

A continuacion creamos un metodo, ```distance()``` el cual usando el [Teorema de Pitagoras](https://www.montereyinstitute.org/courses/DevelopmentalMath/TEXTGROUP-1-8_RESOURCE/U07_L1_T4_text_final_es.html), nos devuelve la distancia entre un punto y otro. Para usar ciertas funciones matematicas, como el calculo de la raiz cuadrada, hacemos uso de la libreria ```dart:math```;

Finalmente, sobreescribimos el metodo ```toString()```, cuyo objetivo es devolver una representacion del objeto creado. De no hacer esto, al imprimir los puntos, ```print(p1);```, nos devuelve que es una instancia de Point, ```Instance of 'Point'```. No es muy explicativo. Usando nuestra version de ```toString()``` es mas clara, pues nos devuelve el par ordenado que representa su posicion en el espacio.

## Conclusion

Este es un primer acercamiento al lenguaje Dart. Como pueden ver, no es muy diferente a otros lenguajes de programacion como ```Java``` o ```C```. Tambien tiene ciertas facilidades como ```closure```, tecnica tambien llamada expresion ```lambda```, inspirado de JavaScript. Pero como siempre menciono, si manejas un lenguaje orientado a objetos, los manejas todos, solo es de aprender sus reglas de sintaxis, los cuales la mayoria son iguales. 

Es Dart el lenguaje del futuro? Solo puedo decir lo siguiente: es un lenguaje avanzado para su época y sin duda es un lenguaje muy agradable.
