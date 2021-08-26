---
title: Que son las operaciones CRUD usando Django
slug: django-crud
date: 2021-08-25
category: programming
tags: python, django, crud, framework, web development
summary: En este articulo usaremos un framework de Python llamado Django y realizaremos operaciones CRUD para realizar un catalogo de libros.
---
## Que es Django?

Django es un framework web de alto nivel que permite el desarrollo rápido de sitios web seguros y mantenibles. Desarrollado por programadores experimentados, Django se encarga de gran parte de las complicaciones del desarrollo web, por lo que puedes concentrarte en escribir tu aplicación sin necesidad de reinventar la rueda. Es gratuito y de código abierto, tiene una comunidad próspera y activa, una gran documentación y muchas opciones de soporte gratuito y de pago.

Django fue desarrollado inicialmente entre 2003 y 2005 por un equipo que era responsable de crear y mantener sitios web de periódicos. Después de crear varios sitios, el equipo empezó a tener en cuenta y reutilizar muchos códigos y patrones de diseño comunes. Este código común se convirtió en un framework web genérico, que fue de código abierto, conocido como proyecto "Django" en julio de 2005.

Django ha continuado creciendo y mejorando desde su primer hito, el lanzamiento de la versión (1.0) en septiembre de 2008, hasta el reciente lanzamiento de la versión 1.11 (2017). Cada lanzamiento ha añadido nuevas funcionalidades y solucionado errores, que van desde soporte de nuevos tipos de bases de datos, motores de plantillas, caching, hasta la adición de funciones genéricas y clases de visualización (que reducen la cantidad de código que los desarrolladores tiene que escribir para numerosas tareas de programación).

Para mas informacion, [leer aqui](https://developer.mozilla.org/es/docs/Learn/Server-side/Django/Introduction).

## Configuracion de Ambiente de Desarrollo

El entorno de desarrollo es una instalación de Django en su computadora local que puede utilizar para desarrollar y probar aplicaciones de Django antes de implementarlas en un entorno de producción.

Las principales herramientas que proporciona el propio Django son un conjunto de scripts de Python para crear y trabajar con proyectos de Django, junto con un servidor web de desarrollo simple que puede usar para probar aplicaciones web locales (es decir, en su computadora, no en un servidor web externo). en el navegador web de su computadora.

Hay otras herramientas periféricas, que forman parte del entorno de desarrollo, que no cubriremos aquí. Estos incluyen cosas como un editor de texto o IDE, yo estare usando [Visual Studio Code](https://code.visualstudio.com/download), para editar código y una herramienta de administración de control de fuente como [Git](https://git-scm.com/) para administrar de manera segura diferentes versiones de su código. Suponemos que ya tiene instalado un editor de texto.

### Usando Django en un ambiente virtual de Python

Iniciemos creando un ambiente virtual.

```
python3 -m venv djangovenv
```

Y lo activamos

```
# Linux/macOS
$ source djangovenv/bin/activate
```

```
# Windows
> djangovenv\Scripts\activate
```

A partir de este momento se asume que todos los comandos mostrados son ejecutados en el ambiente virtual.

### Instalando Django

Una vez en el ambiente virtual, instalamos Django.

```
$> pip install django
```

Podemos probar que esta correctamente instalado ejecutando el siguiente comando

```
$> python -m django --version
```

## Creando un esqueleto

Una vez instalado, crearemos un proyecto, el cual luego agregaremos con componentes especificos a la aplicacion web como modelos, vistas, rutas y preferencias.

El flujo de trabajo es usualmente el siguiente:

1. Usar la herramienta `django-admin` para crear la carpeta del proyecto, plantillas basica y `manage.py` que sirve para administrar el proyecto.
1. Usar `manage.py` para crear uno o mas aplicaciones.
1. Registrar las nuevas aplicaciones para incluirlas en el proyecto.
1. Enganchar las rutas de cada aplicacion.

## Crear el proyecto

Crear un nuevo proyecto vacio usando la herramienta `django-admin` y luego entramos en la carpeta

```
django-admin startproject libreria
cd libreria
```

La carpeta `libreria` cuenta con la siguiente estructura

```
libreria
|- manage.py
|- libreria/
   |- __init__.py
   |- asgi.py
   |- settings.py
   |- urls.py
   |- wsgi.py
```

Los archivos importantes aqui son:

* **settings.py** contiene las preferencias del sitio web, incluye las aplicaciones registradas, la ubicacion de archivos estaticos, detalles de la configuracion de base de datos, etc.
* **urls.py** define conexion entre ruta y vista. Si bien este archivo puede registrar todas las rutas del proyecto, lo mejor es delegar las rutas a cada aplicacion, como lo veremos luego.
* **manage.py** es el script usado para crear aplicaciones, trabajar con la base de datos e iniciar el servidor de desarrollo.

## Crear aplicacion

<div class="row"><div class="col-10 offset-1">
<blockquote class="blockquote">¿Cuál es la diferencia entre un proyecto y una aplicación? Una aplicación es una aplicación web que hace algo, por ejemplo, un sistema de registro web, una base de datos de registros públicos o una pequeña aplicación de encuesta. Un proyecto es una colección de configuraciones y aplicaciones para un sitio web en particular. Un proyecto puede contener varias aplicaciones. Una aplicación puede estar en varios proyectos.</blockquote>
</div></div>

A continuacion crearemos una aplicacion llamada `catalogo`.

```
python manage.py startapp catalogo
```

Esto creara una carpeta llamada `catalogo` con diferentes archivos para nuestra aplicacion.

```
libreria
|- manage.py
|- libreria/
|- catalogo/
   |- migrations/
   |- __init__.py
   |- admin.py
   |- apps.py
   |- models.py
   |- tests.py
   |- views.py
```

## Registrar la aplicacion

Ahora que la aplicacion ha sido creada, debemos registrarla con el proyecto para ser incluida cuando ejecutemos alguna herramienta.

Abrimos el archivo `settings.py` en el proyecto y buscamos la lista llamada `INSTALLED_APPS` y agregamos `catalogo` a la lista.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Aqui agregamos nuevas aplicaciones
    'catalogo.apps.CatalogoConfig',
]
```

La nueva linea especifica el objeto de configuracion que fue generado en `/libreria/catalogo/apps.py` al crear la aplicacion.

## Especificando la base de datos

En el mismo archivo tambien se puede configurar la base de datos a usar. Django es compatible con varios [motores de base de datos](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-DATABASE-ENGINE). En lo posible, intentar mantener una misma configuracion entre desarrollo y produccion, para reducir problemas de funcionamiento.

Para este tutorial usaremos [SQLite](https://www.sqlite.org). Esta ya viene configurada por defecto.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

## Enganchar las rutas

Al crear el proyecto, se creo un archivo `urls.py` que se encarga de direccionar al usuario a la aplicacion correcta de acuerdo a lo que haya escrito en la barra de direcciones. Como se dijo mas arriba, aqui podemos manejar las rutas de todo el proyecto, pero lo mas comun es delegar a cada aplicacion manejar sus propias rutas. El mismo archivo nos explica como.

```python
"""locallibrary URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/3.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

Las rutas son manejadas mediante `urlpatterns` que es una lista de funciones `path()`. Cada `path()` asocia un patron de URL con una vista definida en `views.py`. Esta lista inicialmente define la URL `admin/` con el modulo `admin.site.urls` que contiene la definicion de URLs para la aplicacion de Administracion.

La documentacion al inicio del archivo nos indica las diferentes maneras de agregar una ruta en esta lista. Usaremos el tercer patron para agregar las rutas de nuestra aplicacion. Para esto debemos importar la funcion `include` del modulo `django.urls`.

```python
# Documentacion eliminada para brevedad

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('catalogo/', include('catalogo.urls')),
]
```

Adicional a esto, si ejecutamos el servidor de prueba y vamos a http://127.0.0.1:8000 nos mostrara un error de que la ruta `/` no ha sido definida. En nuestro caso, solo tenemos una aplicacion por lo cual redigiremos a nuestra aplicacion.

El archivo quedaria asi:

```python
# Documentacion eliminada para brevedad

from django.contrib import admin
from django.urls import path, include
from django.views.generic import RedirectView

urlpatterns = [
    path('admin/', admin.site.urls),
    path('catalogo/', include('catalogo.urls')),
    path('', RedirectView.as_view(url='catalogo/', permanent=True)),
]
```

Y como ultimo paso, debemos crear el archivo `urls.py` dentro de la aplicacion con el siguiente contenido

```python
from django.urls import path
from . import views

urlpatterns = [

]
```

## Probando el esqueleto

Uf! Eso ha sido bastante! Pero ya tenemos el esqueleto completo y listo para su primera prueba. La aplicacion como tal no hace nada aun, pero es bueno probar para ver si no hemos roto algo.

Para esto ejecutaremos nuestro servidor de desarrollo mediante el comando `runserver`

```
python manage.py runserver
```

Al acceder a la direccion http://127.0.0.1:8000 veremos el siguiente mensaje de error

<div class=row><div class="col-md-10 offset-md-1">
<img src="{static}/images/catalogo_sinrutas.png" class=img-fluid>
</div></div>

Esto es porque aun no hemos definido rutas y vistas en nuestra aplicacion, en el modulo `catalogo.urls` (archivo `catalogo/urls.py`)

Pero en este punto sabemos que funciona!
