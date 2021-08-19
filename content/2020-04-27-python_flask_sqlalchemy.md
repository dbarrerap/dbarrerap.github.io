---
title: Getting started with MariaDB using Docker, Python and Flask
date: 2020/04/27
category: programming
tags: python, flask, sqlalchemy, mariadb, mysql, tutorial
---
I've written this short walkthrough to provide a boilerplate for greater applications using MariaDB (a drop-in replacement of MySQL) with Docker and Python (with the help of Flask). This is my first English post, so pardon my French.

## Docker? What and why?

Sometimes we want to install a specific version of MariaDB on a certain system, but no packages are available. Or maybe, we simply want to isolate MariaDB from the rest of the system, to be sure that we won't cause any damage.

A virtual machine would certainly serve the scope. However, this means installing a system on the top of another system. It requires a lot of resources.

In many cases, the best solution is Docker. Docker is a framework that runs containers. A container is meant to run a specific daemon, and the software that is needed for that daemon to properly work. Docker does not virtualize a whole system; a container only includes the packages that are not included in the underlying system.

Docker requires a very small amount of resources. It can run on a virtualized system. It is used both in development and in production environments. [Source](https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/)

## Flask

Flask is a micro web framework written in Python. It is classified as a microframework because it does not require particular tools or libraries. 

## Installing necessary software

1. Install [Docker](https://www.docker.com/products/docker-desktop) following their instructions according to your system.
2. Download and install [Python](https://www.python.org/downloads/).

## Prepare environment

### Docker + MySQL

Open a terminal window and run the following command:

```bash
$ docker run -p 3306:3306 --name mariadb -e MARIADB_ROOT_PASSWORD=S0m3P4ssw0rd! mariadb/server:10.4
```

The previous command will spin up a MariaDB Server container that you can connect to and communicate with using a MariaDB client. If you're running macOS, like me, you can install ```mysql-client``` from Homebrew.

Connect to your MariaDB instance by executing the following command in a terminal window.

```bash
$ mysql -h 127.0.0.1 -u root -p
```

Enter the password set up with the Docker. You should see something like this.

<div class="row"><div class="col-md-6 offset-md-3">
<img src="/assets/ScreenShot20200426_1458.png" alt="Screenshot - MySQL" class="img-fluid">
</div></div>

Now, create the database.

```sql
CREATE DATABASE mydb;
```

Create a new table.

```sql
CREATE TABLE mydb.user (name VARCHAR(32));
```

Now, insert new records.

```sql
INSERT INTO mydb.user VALUES ('David'), ('Paula'), ('Gabriela'), ('Andres');
```

We are done with the databse for now.

### Python + Flask

For this, I'll create a virtual environment.

```bash
$ python3 -m venv venv
$ source venv/bin/activate
```

And now let's install Flask.

```bash
$ pip install flask 
```

Let's check if everything goes according to plan. Create a file called ```app.py``` and write the following

```python
from flask import Flask

app = Flask(__name__)
app.config['DEBUG'] = True

@app.route('/')
def index():
    return 'Hello world'

if __name__ == '__main__':
    app.run()
```

You're ready to test your ```Hello world``` web application.

```bash
$ python3 app.py
```

Now open your browser and go to ```http://localhost:5000```. It should say **Hello world**. If it doesn't, leave a comment below with your error and I'll do my best to help you out.

Now let's continue by installing the dependencies.

```bash
$ pip install flask-sqlachemy pymysql
```

**Flask-SQLAlchemy** is an extension for Flask that adds support for SQLAlchemy to your application. It aims to simplify using SQLAlchemy with Flask by providing useful defaults and extra helpers that make it easier to accomplish common tasks.

**PyMySQL** is a pure-Python MySQL client library, based on PEP 249. We will use this as our connector to the database.

Now let's add to our code.

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['DEBUG'] = True
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://{DB_USERNAME}:{DB_PASSWORD}@localhost:3306/{DB_NAME}'
db = SQLAlchemy(app)

@app.route('/')
def index():
    return 'Hello world'

if __name__ == '__main__':
    app.run()
```

Replace ```{DB_USERNAME}```, ```{DB_PASSWORD}```, and ```{DB_NAME}``` with the respective data. Now, we have two ways to access the data: _Reflect_, which has read-only access, and _Automap_, which allows us to have read-write access. We'll be using the latter.

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.ext.automap import automap_base

app = Flask(__name__)
app.config['DEBUG'] = True
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://{DB_USERNAME}:{DB_PASSWORD}@localhost:3306/{DB_NAME}'
db = SQLAlchemy(app)

Base = automap_base()
Base.prepare(db.engine, reflect=True)

{NAME} = Base.classes.{TABLE_NAME}

@app.route('/')
def index():
    return 'Hello world'

if __name__ == '__main__':
    app.run()
```

Where ```{NAME}``` is a name we give to the database's ```{TABLE_NAME}```. So, for our database that we set up previously, the code would be like this.

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.ext.automap import automap_base

app = Flask(__name__)
app.config['DEBUG'] = True
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:S0m3P4ssw0rd!@localhost:3306/mydb'
db = SQLAlchemy(app)

Base = automap_base()
Base.prepare(db.engine, reflect=True)

Users = Base.classes.user

@app.route('/')
def index():
    users = db.session.query(Users).all()
    for user in users:
        print(user.name)
    return 'Hello world'

if __name__ == '__main__':
    app.run()
```

The new code does the following:

* **users = db.session.query(Users).all()** queries the table and fetches all the records.
* **print(user.name)**, as we iterate over every record in the table, we can access the columns as properties of the object ```User```.

In order to return all the properties from the object we'll use the ```vars()``` method. Let's modify the ```index()``` method like this.

```python
@app.route('/')
def index():
    users = db.session.query(Users).all()
    data = dict()
    for user in users:
        data = {prop: value for (prop, value) in vars(user).items()}
    return str(data)
```

We create a dictionary object called ```data``` which we will populate with all the properties of the ```User``` object and now our web application shows this data on the browser, as a string. Let's convert this into a [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) object, by importing the JSON module. Then we'll use the ```dumps()``` method to show it as a response. The final code is the following.

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.ext.automap import automap_base
import json

app = Flask(__name__)
app.config['DEBUG'] = True
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:S0m3P4ssw0rd!@localhost:3306/mydb'
db = SQLAlchemy(app)

Base = automap_base()
Base.prepare(db.engine, reflect=True)

Users = Base.classes.user

@app.route('/')
def index():
    users = db.session.query(Users).all()
    data = list()
    for user in users:
        row = {prop: value for (prop, value) in vars(user).items()}
        data.append(row)
    return json.dumps(str(data))

if __name__ == '__main__':
    app.run()
```

## This is (NOT) the end.

Hopefully this short walkthrough has helped you get started using MariaDB with Docker, Python and Flask. And, yes, this was a very simple example, but it only gets more exciting from here!
