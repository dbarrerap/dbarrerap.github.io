---
title: Dart: Herencia y Polimorfismo
date: 2020/06/08
category: programming
tags: dart, development, interpreted language
summary: Continuando con el lenguaje Dart, esta vez vemos conceptos mas profundos del paradigma de Programacion Orientada a objetos.
---
En la entrega anterior habia mencionado que Dart toma inspiracion en ciertas facilidades de otros lenguajes, asi tambien su estructura y que la diferencia radica en la sintaxis del lenguaje. Siendo asi, sería bueno revisar la sintaxis de Dart para tener claridad al leer y escribir codigo.

## Clases

Algo hay que tener en claro: de manera implicita, toda clase hereda de una clase base llamada ```Object```. Pero, que sucede si deseo heredar de una clase que uno mismo haya escrito? Declaremos una clase base, ```Circle```.

    :::dart
    import 'dart:math';

    class Circle {
      double _radius;
      String _color;

      Circle() {
        this._radius = 1.0;
        this._color = 'red';
      }

      Circle.withRadius(double r) {
        this._radius = r;
        this.color = 'red';
      }

      Circle.withRadiusColor(double r, String c) {
        this._radius = r;
        this._color = c;
      }

      double get radius => this._radius;
      String get color => this._color;
      set radius(double r) => this._radius = r;
      set color(String c) => this._color = c;

      String toString() => 'Circle[radius=$_radius, color=$_color]';
      double area() => double.tryParse((pi * pow(_radius, 2)).toStringAsFixed(2));
    }

Como habia mencionado antes, no podemos sobrecargar el constructor o un método, pero podemos asignarle un nombre y esto lo hace diferente.

Tambien podemos ver que los métodos accesores (getters y setters), los podemos resumir un poco mas y podemos usarlos como si fueran las mismas propiedades del objeto instanciado.

Finalmente sobreescribimos el comportamiento del método ```toString()```, el cual nos responde con una representacion escrita del objeto, y un método ```area()``` el cual nos devuelve el area del circulo. Aqui vemos que usamos la funcion ```pow()``` que viene de la libreria ```dart:math```, el cual nos eleva cierto valor a cierta potencia, en nuestro caso el radio al exponente 2, al cuadrado. Tambien encadenamos el método ```.toStringAsFixed()``` el cual nos ayuda a _truncar_ el valor decimal a 2 cifras significativas.

## Herencia

Muy bien, ya tenemos nuestra clase base. Ahora crearemos la subclase.

    :::dart
    class Cylinder extends Circle {
      double _height;

      Cylinder() : super() {
        this._height = 1.0;
      }

      Cylinder.withHeight(this._height) : super();
      
      Cylinder.withHeightRadius(this._height, double r) : super.withRadius(r);

      Cylinder.withHeightRadiusColor(this._height, double r, String c) : super.withRadiusColor(r, c);

      double get height => this._height;
      set height(double h) => this._height = h;

      // String toString() => 'Cylinder[base=${super.toString()}, height=$_height]';
      double volume() => double.tryParse((area() * _height).toStringAsFixed(2));
    }

Para crear una subclase, _extendemos_ de la clase base mediante el uso de la palabra clave ```extends```. De esta manera tenemos acceso a todas las caracteristicas ya implementadas en la clase base.

Cada constructor llama a su constructor base mediante ```: super()``` llamado al constructor correspondiente y enviando los argumentos de ser necesarios.

He dejado comentado el método ```toString()``` para hacer una prueba rapida, la cual veremos a continuación.

```dart
void main() {
  Cylinder cyl1 = Cylinder();
  print(cyl1);
}
```

Al mostrar la información del cilindro, nos muestra la información del circulo base. Obviamente es incompleto o erroneo, depende de como lo quieran ver. Si descomentamos esta línea y volvemos a ejecutar nuestro programa, nos devuelve una información mas detallada como la que presento a continuación:

```bash
$ dart cylinder.dart -c
Cylinder[base=Circle[radius=1.0, color=red], height=1.0]
$
```

Algo muy particular es que, aunque Dart es un lenguaje bastante declarativo, le cuesta averiguar ciertas cosas. Por ejemplo el uso de ```super``` para obtener el resultado del método del mismo nombre de la clase base, es por esto que debemos llamar al método del cual queremos que nos responda, por eso se llama a ```super.toString()```, para que nos responda el método ```toString()``` de la clase base.

Usemos uno de los otros constructores para tener claro su funcionamiento:

```dart
void main() {
  Cylinder cyl1 = Cylinder();
  print(cyl1);
  Cylinder cyl2 = Cylinder.withHeightRadius(5.0, 2.0);
  print(cyl2);
  print('area: ${cyl2.area()}, volumen: ${cyl2.volumen()}');
}
```

El cual nos responde:

```bash
$ dart cylinder.dart -c
Cylinder[base=Circle[radius=1.0, color=red], height=1.0]
Cylinder[base=Circle[radius=2.0, color=red], height=5.0]
area: 12.57, volumen: 62.85
$
```

Aqui podemos ver que nos devuelve la informacion del segundo cilindro y el valor del _area base_ y el _volumen_.

## Conclusion

Como dije, cada lenguaje tiene sus detalles en cuanto a la sintaxis (como se escribe) y la semántica (significado de lo escrito). Hoy revisamos el como llamar el constructor y un método de la clase base desde una subclase.
