---
title: Laravel 6 - CRUDamente simple!
layout: post
date: 2019-10-01 00:00:00 -0500
tags: [php, laravel, tutorial, crud]
comments: true
---
<p class="lead">En esta ocasion crearemos una simple aplicacion CRUD.</p>

Como es normal, empezaremos con definiciones:

<dl><dt>CRUD</dt>
<dd><code>C</code>reate, <code>R</code>ead, <code>U</code>pdate, <code>D</code>elete. Son las operaciones
basicas en una aplicacion web con base de datos donde permite crear un registro, leer el registro,
actualizar los datos del registro y finalmente eliminar un registro.</dd></dl>

Muy bien, con eso fuera del camino, aqui van los pasos que vamos a seguir:

* Instalar Laravel
* Configurar las credenciales para la base de datos
* Generar un Modelo y Migracion
* Crear un Controlador y Ruta
* Iniciar servidor de prueba
* Crear las Vistas

## Instalar Laravel

Muy bien, para instalar Laravel primeramente debemos tener instalado 
[Composer](https://getcomposer.org/download/). Una vez resuelto esto, instalamos Laravel
con el siguiente comando:

{% highlight console %}composer create-project --prefer-dist laravel/laravel crudapp{% endhighlight %}

Esto creara una carpeta llamada <code>crudapp</code> donde se instalar&aacute; Laravel y
toda la estructura del proyecto. Fin del paso 1.

## Configurar las credenciales para la base de datos

Abrir el archivo <code>.env</code> que se encuentra en la raiz del directorio
del proyecto, ubicar la seccion de la base de datos y modificar los valores
que les muestro a continuacion:

{% highlight console %}
DB_CONNECTION=mysql 
DB_HOST=127.0.0.1 
DB_PORT=3306 
DB_DATABASE=nombre_base_de_datos
DB_USERNAME=usuario_base_de_datos
DB_PASSWORD=clave_base_de_datos
{% endhighlight %}

Previamente debe existir la base de datos mencionada. De no existir, hacerlo en este momento.
[Clic aqui](http://www.oscarabadfolgueira.com/crear-una-base-datos-mysql-desde-consola/) si no 
sabes como hacerlo. Fin del paso 2.

## Generar un Modelo y Migracion

En el [post anterior]({% link _posts/2019-09-21-laravel-pt-1.md %}) habiamos hablado muy por
encima lo que es el Modelo, el esquema de como es la estructura de una entidad o tabla
en la base de datos, aqui tambien podremos ver sus relaciones con otros Modelos (mas de esto
en otro proyecto). Iniciemos creando el Modelo:

{% highlight console %}php artisan make:model Book -m{% endhighlight %}

Este comando creara un archivo llamado Book.php en el directorio <code>app/</code>, este es
nuestro Modelo. La opcion <code>-m</code> crea el archivo migracion, el cual podemos encontrar en
<code>database/migrations/</code>, con el nombre <code>&lt;timestamp&gt;-create_books_table.php</code>.
Vamos a editar estos archivos.

Primeramente modifiquemos el archivo de migracion de la siguiente manera:

{% highlight php %}
<?php 
  
use Illuminate\Support\Facades\Schema; 
use Illuminate\Database\Schema\Blueprint; 
use Illuminate\Database\Migrations\Migration; 
   
class CreateBooksTable extends Migration 
{ 
    /** 
     * Run the migrations. 
     * 
     * @return void 
     */ 
    public function up() 
    { 
        Schema::create('books', function (Blueprint $table) { 
            $table->increments('id'); 
            $table->string('titulo'); 
            $table->string('autor'); 
            $table->text('resumen'); 
            $table->timestamps(); 
        }); 
    } 
   
    /** 
     * Reverse the migrations. 
     * 
     * @return void 
     */ 
    public function down() 
    { 
        Schema::dropIfExists('books'); 
    } 
}
{% endhighlight %}

Una vez realizado esto, crearemos la tabla <code>books</code> en la base de datos con las
columnas <code>titulo, autor, resumen</code>. Las columnas <code>id, timestamps</code> son
parte del control de cambios de la tabla, ya hablaremos de eso mas adelante. Para crear la
tabla en la base de datos, ejecutamos el siguiente comando:

{% highlight console %}php artisan migrate{% endhighlight %}

Ahora vamos al archivo del Modelo y lo editamos asi:

{% highlight php %}
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    protected $fillable = [
     'titulo',
     'autor',
     'resumen',
    ];
}
{% endhighlight %}

Con esto le decimos al modelo que los campos <code>titulo, autor, resumen</code> seran llenados
por el usuario de la aplicacion.

## Crear un Controlador y Ruta

El controlador maneja la logica de nuestra aplicacion, es quien se encarga de obtener los
datos del Modelo y pasarselos a la Vista para presentarselo al usuario y a su vez de obtener
los datos del usuario para pasarlos al Modelo para ser guardados en la base de datos.
Para crear el controlador ejecutamos:

{% highlight console %}php artisan make:controller BookController --resource{% endhighlight %}

Este comando creara un archivo en <code>app/Http/Controllers</code> llamado <Code>BookController.php</code>.
Si abrimos este archivo podemos ver que tiene muchos metodos ya escritos para nosotros. Cada uno
de estos metodos seran llamados para cada accion que realicemos con nuestra aplicacion, estos
metodos son: <code>index, store, create, update, destroy, show, edit</code>.

Pero para acceder a estos metodos, debemos crear las rutas, la direccion web que invoca estos
metodos. Para esto vamos a <code>routes/</code> y abrimos el archivo <code>web.php</code> y 
le agregamos la siguiente linea:

{% highlight php %}
<?php 

 Route::get('/', function () {
     return view('welcome');
 });
 
Route::resource('books', 'BookController');
{% endhighlight %}

En el controlador, <code>BookController</code> agregar la siguiente linea al metodo <code>create</code>:

{% highlight php %}
// En la parte superior debemos tener las siguientes importaciones:
// use App\Book;
// use Illuminate\Http\Request;

public function create()
{
    return 'Create a Book object!';
}
{% endhighlight %}

## Iniciar servidor de prueba

Para probar que todo va bien, Laravel viene con un servidor de prueba, el cual podemos ejecutar
con el siguiente comando:

{% highlight console %}php artisan serve{% endhighlight %}

Esto ejecuta un servidor web en el puerto 8000, al cual vamos a acceder con la siguiente direccion:
<code>http://localhost:8000/books/create</code>

Si ven la siguiente imagen:

<div class="row">
<div class="col-md-8 offset-md-2">
<img src="/assets/ScreenShot2019-09-22.png" alt="Screenshot 1" class="img-fluid img-thumbnail">
</div>
</div>

Perfecto, hasta ahora todo bien! Ahora debemos editar el controlador de la siguiente manera:

{% highlight php %}
<?php

namespace App\Http\Controllers;

use App\Book;
use Illuminate\Http\Request;

class BookController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $books = Book::all();
        return view('books.index', compact('books'));
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
        return view('books.create');
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $request->validate([
            'titulo'=>'required',
            'autor'=>'required',
            'resumen'=>'required'
        ]);

        $book = new Book([
            'titulo' => $request->get('titulo'),
            'autor' => $request->get('autor'),
            'resumen' => $request->get('resumen'),
        ]);
        $book->save();
        return redirect('/books')->with('success', 'Book saved!');
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        // Por ahora este metodo no lo usaremos
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        $book = Book::find($id);
        return view('books.edit', compact('book'));
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $request->validate([
            'titulo'=>'required',
            'autor'=>'required',
            'resumen'=>'required'
        ]);

        $book = Book::find($id);
        $book->titulo = $request->get('titulo');
        $book->autor = $request->get('autor');
        $book->resumen = $request->get('resumen');
        $book->save();

        return redirect('/books')->with('success', 'Book updated!');
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        $book = Book::find($id);
        $book->delete();

        return redirect('/books')->with('success', 'Book deleted!');
    }
}
{% endhighlight %}

Y con esto ya estamos listos para el siguiente y ultimo paso...

## Crear las Vistas

Cuando se probo que las rutas y metodos esten funcionando correctamente, lo unico que hicimos fue
devolver texto. Obviamente no es lo que esperamos que suceda cuando queremos crear un objeto
dentro de nuestra aplicacion, queremos ingresar datos para que sean almacenados en la base de datos!
Es por eso que en el paso anterior hemos cambiado ese comportamiento y vemos que ahora ciertos
metodos, <code>index, show, create, edit</code>, devuelven algo llamado <code>view</code>, una vista,
una pagina web para que el usuario interactue con nuestra aplicacion. Para esto, abramos el
directorio <code>resources/views/</code>, creamos una carpeta llamado <code>books</code> y
dentro de esta carpeta crearemos los siguientes archivos:

* layout.blade.php
* index.blade.php
* create.blade.php
* edit.blade.php

Los cuales editaremos de la siguiente manera:

__layout.blade.php__
{% highlight php %}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Laravel 6 + MySQL CRUD Tutorial</title>
    <meta name="csrf-token" content="{% raw %}{{ csrf_token() }}{% endraw %}">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        @yield('content')
    </div>
    <script src="{% raw %}{{ asset('js/app.js') }}{% endraw %}" type="text/js"></script>
</body>
</html>
{% endhighlight %}

__index.blade.php__
{% highlight plain %}
@extends('books.layout')
   
@section('content')
<div class="row">
<h2>Listado de Libros</h2>
</div>

<div class="row">
    <div class="col-md-10 offset-md-1">
        <a href="{% raw %}{{ route('books.create') }}{% endraw %}" class="btn btn-primary mb-2">Crear</a>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>Titulo</th>
                    <th>Autor</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
            @foreach($books as $book)
                <tr>
                    <td>{% raw %}{{ $book->titulo }}{% endraw %}</td>
                    <td>{% raw %}{{ $book->autor }}{% endraw %}</td>
                    <td>
                        <a href="{% raw %}{{ route('books.edit',$book->id)}}{% endraw %}" class="btn btn-secondary">Editar</a>
                        <form action="{% raw %}{{ route('books.destroy', $book->id)}}{% endraw %}" method="post">
                            {% raw %}{{ csrf_field() }}{% endraw %}
                            @method('DELETE')
                            <button class="btn btn-danger" type="submit">Eliminar</button>
                        </form>
                    </td>
                </tr>
            @endforeach
            </tbody>
        </table>
    </div>
</div>
@endsection
{% endhighlight %}

__create.blade.php__
{% highlight plain %}
@extends('books.layout')
   
@section('content')
<div class="row">
<h2>Creacion de Libro</h2>
</div>

<div class="row">
    <div class="col-md-8 offset-md-2">
        <form action="{% raw %}{{ route('books.store') }}{% endraw %}" method="POST" name="nuevo_libro">
            {% raw %}{{ csrf_field() }}{% endraw %}
            <div class="form-row">
                <label for="book_titulo">Titulo</label>
                <input type="text" name="titulo" id="book_titulo" class="form-control" placeholder="Titulo del Libro">
                <span class="text-danger">{% raw %}{{ $errors->first('titulo') }}{% endraw %}</span>
            </div>
            <div class="form-row">
                <label for="book_autor">Autor</label>
                <input type="text" name="autor" id="book_autor" class="form-control" placeholder="Autor del Libro">
                <span class="text-danger">{% raw %}{{ $errors->first('autor') }}{% endraw %}</span>
            </div>
            <div class="form-row">
                <label for="book_resumen">Resumen</label>
                <textarea name="resumen" id="book_resumen" class="form-control" placeholder="Resumen del Libro"></textarea>
                <span class="text-danger">{% raw %}{{ $errors->first('resumen') }}{% endraw %}</span>
            </div>
            <button type="submit" class="btn btn-primary">Guardar</button>
        </form>
    </div>
</div>
@endsection
{% endhighlight %}

__edit.blade.php__
{% highlight plain %}
@extends('books.layout')
   
@section('content')
<div class="row">
<h2>Actualizacion de Libro</h2>
</div>

<form action="{% raw %}{{ route('books.update', $book->id) }}{% endraw %}" method="POST" name="actualizar_libro">
    {% raw %}{{ csrf_field() }}{% endraw %}
    @method('PATCH')
    <div class="form-group">
        <label for="book_titulo">Titulo</label>
        <input type="text" name="titulo" id="book_titulo" class="form-control" value="{% raw %}{{ $book->titulo }}{% endraw %}">
        <span class="text-danger">{% raw %}{{ $errors->first('titulo') }}{% endraw %}</span>
    </div>
    <div class="form-group">
        <label for="book_autor">Autor</label>
        <input type="text" name="autor" id="book_autor" class="form-control" value="{% raw %}{{ $book->autor }}{% endraw %}">
        <span class="text-danger">{% raw %}{{ $errors->first('autor') }}{% endraw %}</span>
    </div>
    <div class="form-group">
        <label for="book_resumen">Resumen</label>
        <textarea name="resumen" id="book_resumen" class="form-control">
            {% raw %}{{ $book->resumen }}{% endraw %}
        </textarea>
        <span class="text-danger">{% raw %}{{ $errors->first('resumen') }}{% endraw %}</span>
    </div>
    <button type="submit" class="btn btn-primary">Guardar</button>
</form>
@endsection
{% endhighlight %}

Con esto, hemos terminado nuestra aplicacion. Dejenme explicar un par de componentes de las vistas.

<dl>
<dt>Blade</dt>
<dd>Blade es un motor de plantillas que permite el reuso de componentes para crear increibles
presentaciones. De esta manera, podemos generar una plantilla base (layout) y que esta sea usada
por las vistas que son mostradas al usuario (index, create, edit), de esta manera toda la
aplicacion tiene un dise&ntilde;o consistente.</dd>
<dt>@extends()</dt>
<dd>Esta es la directiva de Blade que define que este archivo es hijo de un archivo mayor
(layout, en este caso), esto es el contenido de este archivo inyectarlo en el padre para 
la visualizacion.</dd>
<dt>@section(&lt;nombre&gt;)</dt>
<dd>Una plantilla padre puede tener varias secciones que pueden ser llenados por el hijo, 
cada una de estas secciones deben tener un nombre unico. De esta manera, en las vistas hijo
solo se escribe el contenido a ser llenado en el padre, evitando repetir codigo.</dd>
</dl>

Ahora, si no tienen todavia ejecutando el servidor, ejecutenlo con el comando:

{% highlight console %}php artisan serve{% endhighlight %}

Y vayan a la direccion <code>http://localhost:8000/books</code> y deberian ver algo asi:

<div class="row mb-2">
<div class="col-md-8 offset-md-2">
<img src="/assets/ScreenShot2019-09-22_11-40-47.png" alt="Screenshot 2" class="img-fluid img-thumbnail">
</div>
</div>

Al presionar el boton de <span class="btn btn-primary">Crear</span>, deberian ver lo siguiente:

<div class="row mb-2">
<div class="col-md-8 offset-md-2">
<img src="/assets/ScreenShot2019-09-22_12-10-12.png" alt="Screenshot 3" class="img-fluid img-thumbnail">
</div>
</div>

Una vez ingresado los datos del libro, deberian regresar a la pagina de listado y ver
el libro que han ingresado, como el siguiente ejemplo:

<div class="row mb-2">
<div class="col-md-8 offset-md-2">
<img src="/assets/ScreenShot2019-09-22_12-12-39.png" alt="Screenshot 4" class="img-fluid img-thumbnail">
</div>
</div>

El presionar <span class="btn btn-secondary">Editar</span>, podremos editar los datos ingresados:

<div class="row mb-2">
<div class="col-md-8 offset-md-2">
<img src="/assets/ScreenShot2019-09-22_12-23-49.png" alt="Screenshot 5" class="img-fluid img-thumbnail">
</div>
</div>

Y finalmente, al presionar <span class="btn btn-danger">Eliminar</span>, el libro se elimina.

Con esto podemos dar por finalizado por ahora el tutorial de una aplicacion CRUD simple. Aunque
queda un metodo pendiente <code>show</code>. Esto se los dejo de tarea, investiguen, lean, implementen.
Si tienen algun error o problema, me lo dejan en la caja de comentarios abajo que los respondere
lo mas pronto posible.

Hasta la proxima.

