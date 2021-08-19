---
title: .NET Core - Aplicacion MVC Basica
date: 2021-08-18
slug: dotnetcore-mvc-1
category: programming
tags: dotnet core, asp.net, mvc, web development, web application, c sharp, c#
summary: Una guia para la creacion de una aplicacion web usando .NET Core/EF y SQLite.
---

En esta pequeña guia, vamos a crear una pequeña aplicacion web usando la arquitectura MVC en ASP.NET Core,
en particular un blog. Tutorial basico. Esta guia puede ser seguida en todos los sistemas operativos, 
con ciertas variaciones, asi que ajustar de acuerdo a la necesidad. 

Las herramientas a utilizar en este tutorial son:

* .NET 5 / Core 3.1
* Visual Studio Code
* SQLite Explorer

## Configuracion de Estacion de Trabajo

### Instalacion .NET Core

Lo primero que debemos hacer es instalar el SDK de .NET Core para el desarrollo de aplicaciones. Para esto,
vamos a [este enlace](https://dotnet.microsoft.com/download) y descargamos el apropiado para nuestro equipo.
Puedes descargar .NET 5 o .Net Core 3.1, cualquiera de los dos funciona.

Para confirmar que se haya instalado correctamente, abrimos una ventana de PowerShell o Simbolo de Sistema y
escribimos:

    :::ps1
    > dotnet

Deberiamos ver algo similar a esto:

<div class="row"><div class="col-md-4 offset-md-4">
<img src="https://www.tutorialsteacher.com/Content/images/core/dotnet-cli.png" class="img-fluid">
</div></div>

### Instalacion Visual Studio Code

Seguimos con nuestro editor de codigo, para poder desarrollar aplicaciones y lo vamos a configurar para
que funcione con el lenguaje por defecto, que es C#. Vamos a [este enlace](https://code.visualstudio.com/download)
y descargamos el apropiado para nuestro equipo. Para instalar [la extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp), revisa la [documentacion](https://code.visualstudio.com/docs/editor/extension-marketplace).

### Instalacion SQLite Explorer

Finalmente instalamos esta herramienta que nos permitira visualizar los datos que guardemos en la base de prueba.
La podemos descargar de [aqui](https://sqlitebrowser.org/dl/).

## Nuestra primera aplicacion ASP.NET MVC

.NET Core viene con plantillas para cada una de las posibles aplicaciones que deseemos realizar, desde aplicaciones
de consola hasta aplicaciones web. Para esto, en una ventana de comandos escribimos:

    :::ps1
    > dotnet new mvc -o MiAplicacionWeb

Esto creara una carpeta _MiAplicacionWeb_ donde se encontraran todos los archivos de nuestro proyecto,
ya iremos viendolos a medida que avancemos. Probemos que todo este bien, escribimos:

    :::ps1
    > cd MiAplicacionWeb
    > dotnet run

Este ultimo comando construira toda la aplicacion y ejecutara un servidor de pruebas para poder visualizar
nuestra aplicacion web en la direccion https://localhost:5000. Presionamos Ctrl+C para terminarlo.

## Crear el Modelo

El modelo es un archivo clase que modela los datos a ser almacenados. Tambien puede ser visto como el maquetado
de las tablas que se almacenaran en la base de datos. Vamos a crear 3 tablas/modelos: _Post_, _Comment_ y _User_.
Para esto, creamos los archivos en la carpeta __Models__ con los nombres _Post.cs_, _Comment.cs_ y _User.cs_.

    ::::cs
    // Post.cs
    using System;
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;

    namespace MiAplicacionWeb.Models
    {
        public class Post
        {
            // Properties
            public int ID { get; set; }
            public string Titulo { get; set; }
            public string Contenido { get; set; }
            [DataType(DataType.Date)]
            [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
            public DateTime Fecha { get; set; }

            // Navigation Properties
            public User User { get; set; }
            public ICollection<Comment> Comments { get; set; }
        }
    }

    // Comment.cs
    namespace MiAplicacionWeb.Models
    {
        public class Comment
        {
            // Properties
            public int ID { get; set; }
            public string Lector { get; set; }
            public string Contenido { get; set; }

            // Navigation Properties
            public Post Post { get; set; }
        }
    }

    // User.cs
    using System.Collections.Generic;

    namespace MiAplicacionWeb.Models
    {
        public class User
        {
            // Properties
            public int ID { get; set; }
            public string Nombre { get; set; }
            public string Apellido { get; set; }
            public string Email { get; set; }

            // Navigation Properties
            public ICollection<Post> Posts { get; set; }
        }
    }

## Agregar herramientas y paquetes NuGet necesarios

Los siguientes comandos nos podran dar funcionalidad con SQLite y CodeGeneration.

    :::ps1
    > dotnet tool install --global dotnet-ef
    > dotnet tool install --global dotnet-aspnet-codegenerator
    > dotnet add package Microsoft.EntityFrameworkCore.Design
    > dotnet add package Microsoft.EntityFrameworkCore.SQLite
    > dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
    > dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    > dotnet add package Microsoft.EntityFrameworkCore.Tools

## Generar codigo para controlador y vista

Aplicaremos un scaffolding al modelo Post y actualizaremos los archivos creados.

    :::ps1
    > dotnet aspnet-codegenerator controller -name PostsController -m Post -dc BlogContext --relativeFolderPath Controllers -udl --referenceScriptLibraries -sqlite

Este comando creara el controlador del modelo Post en la carpeta _Controllers_, las vistas para Crear, Leer, Editar y Eliminar posts, los cuales encontraremos en _Views/Posts_ y finalmente el adminitrador de sesion con la base de datos __BlogContext__ en la carpeta _Data_. Asimismo, actualiza los archivos _Startup.cs_ y _appsettings.json_.

## Actualizar estilo de paginas

En este paso actualizaremos las plantillas para agregar navegacion hacia el modulo de Posts.

    :::html
    Views/Shared/_Layout.cshtml
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - MiAplicacionWeb</title>
        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <header>
            <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
                <div class="container">
                    <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MiAplicacionWeb</a>
                    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                            aria-expanded="false" aria-label="Toggle navigation">
                        <span class="navbar-toggler-icon"></span>
                    </button>
                    <div class="navbar-collapse collapse d-sm-inline-flex justify-content-between">
                        <ul class="navbar-nav flex-grow-1">
                            <li class="nav-item">
                                <a class="nav-link text-dark" asp-area="" asp-controller="Posts" asp-action="Index">Blog</a>
                            </li>
                        </ul>
                    </div>
                </div>
            </nav>
        </header>
        <div class="container">
            <main role="main" class="pb-3">
                @RenderBody()
            </main>
        </div>

        <footer class="border-top footer text-muted">
            <div class="container">
                &copy; 2021 - MiAplicacionWeb - <a asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
            </div>
        </footer>
        <script src="~/lib/jquery/dist/jquery.min.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>
        @await RenderSectionAsync("Scripts", required: false)
    </body>
    </html>

Lo que hemos hecho es eliminar los links de navegacion que se generaron y agregamos uno que nos lleve a nuestro blog.

Ahora debemos actualizar _BlogContext.cs_ para agregar los otros modelos.

    :::cs
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.EntityFrameworkCore;
    using MiAplicacionWeb.Models;

        public class BlogContext : DbContext
        {
            public BlogContext (DbContextOptions<BlogContext> options)
                : base(options)
            {
            }

            public DbSet<MiAplicacionWeb.Models.Post> Post { get; set; }
            public DbSet<MiAplicacionWeb.Models.Comment> Comment { get; set; }
            public DbSet<MiAplicacionWeb.Models.User> User { get; set; }
        }

# Migracion

Una vez hecho esto, vamos a crear nuestra primera migracion. Las migraciones son una forma practica de manejar los
cambios que se realicen a lo largo del tiempo en la base de datos. Vamos a crear la primera migracion, la cual
creara la base y las tablas iniciales, de acuerdo a lo que hemos detallado en los modelos.

    :::ps1
    > dotnet ef migrations add InitialCreate
    > dotnet ef database update

Podemos ejecutar [nuestra aplicacion](https://localhost:5001) para ver que todo vaya viento en popa, 
esto es que podemos acceder a la seccion Blog, la cual estara vacia al momento.

Hasta aqui hemos avanzado bastante, hemos visto la creacion de un nuevo proyecto MVC, 
hemos creado modelos de datos relacionados, hemos modificado las vistas y 
hemos creado migraciones, para mejor control de versiones de cambios en la estructura 
de las tablas de la base de datos.

En el siguiente post modificaremos la apariencia del listado de posts y haremos que se muestre 
la data relacionada en las vistas (como comentarios y datos del autor).

