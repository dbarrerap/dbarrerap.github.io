---
title: Crear una aplicacion de chat en tiempo real usando Flask y SocketIO.
summary: Siempre que leo o escucho de Python y servicios web todos hablan de Django. Mostremos que Flask tambien tiene lo suyo creando una aplicacion de chat usando SocketIO.
date: 2020-08-20
slug: python-flask-socketio
category: programming
tags: python, flask, socketio, websockets, web development
---
## Flask

Flask es un **micro** Framework escrito en Python y desarrollado para simplificar y hacer más fácil la creación de Aplicaciones Web bajo el patron MVC.

La palabra _micro_ no quiere decir que se trate de un proyecto pequeño o que nos sirva para hacer páginas web pequeñas, al instalar Flask disponemos de las herramientas necesarias para crear una aplicacion web completamente funcional. Es probable que en algun momento se necesite una nueva funcionalidad que no se tiene de primeras con la instalacion, para eso encontraras un gran conjunto de plugins que se pueden instalar e integrar facilmente con Flask y que te permitiran añadirle todas las funcionalidades que necesites.

En cuanto al patrón MVC, este es una forma de trabajar que permite diferenciar y separar lo que es la vista (la pagina HTML, lo que el usuario ve), el modelo de datos (los datos que va a manejar la aplicacion), y el controlador (donde se gestionan las peticiones de la app web y se lleva a cabo la logica).

## Flask_SocketIO

### WebSocket

Websocket es un protocolo de comunicaciones que proporciona comunicación bidireccional full-duplex a través de una única conexión TCP.

Para comprender Websockets, primero, debemos tener una comprensión clara del protocolo HTTP porque ambos van de la mano.

HTTP es un protocolo que permite la obtención de recursos, como documentos HTML. Es la base de cualquier intercambio de datos en la Web y es un protocolo cliente-servidor, lo que significa que las solicitudes son iniciadas por el destinatario, generalmente el navegador Web.

Para cada solicitud del navegador, el protocolo HTTP abrirá una conexión, se realiza el intercambio de datos y luego la conexión se cerrará.

Si el requisito es obtener los datos continuamente del servidor, por ejemplo, obtener los datos en tiempo real para el intercambio de criptomonedas, la aplicación de juegos o una aplicación de chat, entonces habrá múltiples llamadas HTTP al servidor.

En qué se diferencia Websocket de los protocolos HTTP tradicionales? Websockets usa comunicación bidireccional y la conexión no se interrumpirá hasta que el cliente o servidor decida terminar la conexión.

Muy bien, ya sacado los tecnicismos del camino, comencemos con nuestra aplicacion web, una sala de chat.

### Instalacion

Primero instalemos todas nuestras dependencias para ejecutar el proyecto. Yo estoy usando una Mac al momento de escribir esta guia, ajustar a su sistema operativo de acuerdo al caso.

    :::bash
    $ python3 -m venv venv
    $ source venv/bin/activate
    $ pip install flask flask-socketio flask-migrate flask-script flask-sqlalchemy python-dotenv

* **Flask**: el framework
* **Flask-Migrate**: Lo utilizaremos para hacer migraciones
* **Flask-Script**: Junto a Flask-Migrate, nos ayudará a realizar migraciones por terminal
* **Flask-SocketIO**: Con esta biblioteca conseguiremos realizar nuestro chat con WebSockets
* **Flask-SQLAlchemy**: ORM que utilizaremos para guardar en nuestra base de datos el historial y poder cargarlo
* **python-dotenv**: Manera sencilla de configurar nuestra aplicación

Ahora debemos crear un archivo de configuracion **.env**. Esta separacion es una buena practica de seguridad.

    :::env
    SECRET_KEY=MiSecreto
    DEBUG=True
    DATABASE="sqlite:///MiAplicacion.db"
    DOMAIN=127.0.0.1:5000

### Base de Datos (Modelo)

El objetivo de instalar SQLAlchemy y Migrate es lograr crear una base de datos donde se guardara un historial de chat.

Creamos un archivo llamado **models.py**

    :::python
    from flask import Flask
    from datetime import datetime
    from flask_sqlalchemy import SQLAlchemy
    from flask_script import Manager
    from flask_migrate import Migrate, MigrateCommand
    from os import environ
    from dotenv import load_dotenv, find_dotenv

    load_dotenv(find_dotenv())
    app = Flask(__name__)

    # Settings
    app.config['SQLALCHEMY_DATABASE_URI'] = environ.get('DATABASE')
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

    # Variables
    db = SQLAlchemy(app)
    migrate = Migrate(app, db)
    manager = Manager(app)
    manager.add_command('db', MigrateCommand)


    class Chat(db.Model):
        '''
        Table chat
        '''
        __tablename__ = 'chat'

        id = db.Column(db.Integer, primary_key=True)
        username = db.Column(db.String(128))
        text = db.Column(db.Text)
        created_at = db.Column(
            db.DateTime, nullable=False, default=datetime.utcnow)

        def __repr__(self):
            return '<Chat {0}>'.format(self.username)


    if __name__ == "__main__":
        manager.run()

Y ahora realizamos la migracion.

    :::bash
    $ python3 models.py db init
    $ python3 models.py db migrate
    $ python3 models.py db upgrade

Se creara la base _MiAplicacion.db_ con la tabla lista para guardar datos.

### Logica de aplicacion (Controlador)

Creamos un archivo llamado **app.py** con el siguiente contenido:

    :::python
    from os import environ
    from dotenv import load_dotenv, find_dotenv
    from flask import Flask, render_template
    from flask_socketio import SocketIO, emit
    from models import db, Chat

    load_dotenv(find_dotenv())
    app = Flask(__name__)

    # Configuracion
    app.config['SECRET_KEY'] = environ.get('SECRET_KEY')
    app.config['DEBUG'] = True if environ.get('DEBUG') == 'True' else False
    app.config['PORT'] = 80

    # Socketio
    DOMAIN = environ.get('DOMAIN')
    socketio = SocketIO(app)

    # Base de datos
    app.config['SQLALCHEMY_DATABASE_URI'] = environ.get('DATABASE')
    db.init_app(app)

    # Cargamos la plantilla HTML con el frontend
    @app.route('/<username>/')
    def open_chat(username):
        my_chat = Chat.query.all()
        return render_template(
            'chat.html',
            domain=DOMAIN,
            chat=my_chat,
            username=username
        )

    # Recibirá los nuevos mensajes y los emitirá por socket
    @socketio.on('new_message')
    def new_message(message):
        # Emitimos el mensaje con el alias y el mensaje del usuario
        emit('new_message', {
            'username': message['username'],
            'text': message['text']
        }, broadcast=True)
        # Salvamos el mensaje en la base de datos
        my_new_chat = Chat(
            username=message['username'],
            text=message['text']
        )
        db.session.add(my_new_chat)
        db.session.commit()


    # Iniciamos
    if __name__ == '__main__':
        socketio.run(app)

## Frontend (Vista)

Creamos la carpeta _templates_ y dentro de ella el archivo **chat.html** con el siguiente contenido

    :::html
    <!DOCTYPE html>
    <html lang="es">

    <head>
        <meta charset="UTF-8">
        <title>Chat</title>
        <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    </head>

    <body>
        <div id="app">
            <!-- Renderizará los nuevos mensajes -->
            <section>
                <div v-for="message in messages">
                    <p><strong>@[[ message.username ]]</strong></p>
                    <p>[[ message.text ]]</p>
                </div>
            </section>
            <!-- Formulario para introducir nuevos mensajes -->
            <section>
                <input v-model="newMessage" @keypress.enter="sendMessage" type="text" placeholder="Escribe un mensaje...">
                <button @click="sendMessage">Enviar</button>
            </section>
        </div>
        <!-- Importamos socket.io -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.0.4/socket.io.slim.js"></script>
        <!-- Importamos VueJS -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.3/vue.min.js"></script>
        <script>
            // Conectamos con nuestro dominio
            var socket = io.connect('{{ domain }}');
            // Instanciamos VueJS
            var app = new Vue({
                el: "#app",
                delimiters: ['[[', ']]'],
                data: {
                    username: '{{ username }}',
                    // Le damos los mensajes del hitorial
                    messages: [
                    {% for message in chat %}
                        {
                            username: '{{ message.username }}',
                            text: '{{ message.text }}'
                        }{% if not loop.last %},{% endif %}
                    {% endfor %}
                    ],
                    newMessage: ''
                },
                methods: {
                    sendMessage: () => {
                        // Enviamos el nuevo mensaje
                        socket.emit('new_message', {
                            channel: app.channel,
                            username: app.username,
                            text: app.newMessage
                        });
                        // Clear text
                        app.$set(app, 'newMessage', '');
                    }
                }
            });

            socket.on('connect', function() {
                console.log('Connect')
            });

            socket.on('new_message', function(msg) {
                // Recibimos los nuevos mensajes y los añadimos a nuestro array
                let my_messages = app.messages;
                my_messages.push({
                    username: msg.username,
                    text: msg.text
                })
                app.$set(app, 'messages', my_messages);
            });
        </script>
    </body>

    </html>

### Resultado final

Ahora abrimos el navegador en la direccion http://127.0.0.1:5000/&lt;usuario&gt; donde &lt;usuario&gt; lo puede reemplazar con su nombre, este es su nombre de usuario en el chat.