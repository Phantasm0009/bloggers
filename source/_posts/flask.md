---
title: How to Build a Todo Application With Python Flask, Heroku and Tailwind CSS
date: 2022-12-20 19:04:42
tags:
    - Python Flask
    - Heroku
    - Tailwind 
    - Application
---
![$cover](images/flask.webp)

[](#introduction)Introduction
-----------------------------

Flask is called a **MicroFramework** because it gives you the basic tools you need in order to build a web application in **Python**.  
With Flask you can build any kind of Web service or backend application.  
If you're beginning using Python for web development i suggest you to start with **Django**.

In this guide We will build a Todo application from Scratch with **Tailwind** A utility-first CSS framework for rapidly building custom designs.

[](#requirements)Requirements
-----------------------------

To follow along with me make sure you've :

*   python3
*   pip3
*   pipenv
*   Tailwind CDN

[](#application-setup)Application Setup
---------------------------------------

Create new folder and install the necessary tools.

Create a new folder  

    $ mkdir flask_tailwind_todo_app
    $ cd flask_tailwind_todo_app/
    ...
    

Enter fullscreen mode Exit fullscreen mode

Create a Virtual environment with **pipenv**  

    $ pipenv shell
    ...
    

Enter fullscreen mode Exit fullscreen mode

    Creating a virtualenv for this project…
    Using /usr/bin/python3 (3.7.5) to create virtualenv…
    ⠋Already using interpreter /usr/bin/python3
    Using base prefix '/usr'
    New python executable in /home/username/.local/share/virtualenvs/flask_tailwind_todo_app-4wPj_lFD/bin/python3
    Also creating executable in /home/username/.local/share/virtualenvs/flask_tailwind_todo_app-4wPj_lFD/bin/python
    Installing setuptools, pip, wheel...
    done.
    
    Virtualenv location: /home/username/.local/share/virtualenvs/flask_tailwind_todo_app-4wPj_lFD
    Creating a Pipfile for this project…
    Spawning environment shell (/usr/bin/zsh). Use 'exit' to leave.
    . /home/username/.local/share/virtualenvs/flask_tailwind_todo_app-4wPj_lFD/bin/activate
    username@username-Latitude-7480 ~/projects/xarala/source-code/flask_tailwind_todo_app
    ╰─$ . /home/username/.local/share/virtualenvs/flask_tailwind_todo_app-4wPj_lFD/bin/activate
    (flask_tailwind_todo_app-4wPj_lFD) username@username-Latitude-7480 ~/projects/xarala/source-code/flask_tailwind_todo_app
    ╰─$
    
    

Enter fullscreen mode Exit fullscreen mode

This command will create a new virtual environment and activate it.

You can learn more about **Pipenv** in this [Post](https://www.ousseynoudiop.com/python-how-to-setup-your-virtualenv-correctly/python-how-to-setup-your-virtualenv-correctly/)

Install **flask**, **sqlalchemy** and **gunicorn**  

    $ pipenv install flask flask-sqlalchemy gunicorn
    

Enter fullscreen mode Exit fullscreen mode

This will install Flask, Sqlalchemy and gunicorn in our virtual environment.

[](#create-our-first-flask-app)Create our first Flask app
---------------------------------------------------------

Create new file inside **flask\_tailwind\_todo\_app** directory  

    $ touch app.py
    

Enter fullscreen mode Exit fullscreen mode

Add  

    from flask import Flask
    
    app = Flask(__name__)
    
    
    @app.route("/")
    def home():
       return "Hello Flask"
    
    
    if __name__ == "__main__":
       app.run(debug=True)
    
    
    

Enter fullscreen mode Exit fullscreen mode

Open your terminal and run  

    $ python app.py
    

Enter fullscreen mode Exit fullscreen mode

You'll see something like this  

    * Serving Flask app "app" (lazy loading)
    * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: on
    * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    * Restarting with stat
    * Debugger is active!
    * Debugger PIN: 278-921-051
    

Enter fullscreen mode Exit fullscreen mode

Go to **[http://127.0.0.1:5000/](http://127.0.0.1:5000/)** what is the result ?

If everything work fine you'll see **Hello Flask**  
Let's make it great by rendering a template

[](#rendering-templates)Rendering Templates
-------------------------------------------

    from flask import Flask, render_template
    
    app = Flask(__name__)
    
    
    @app.route("/")
    def home():
       return render_template("home.html")
    
    
    if __name__ == "__main__":
       app.run(debug=True)
    

Enter fullscreen mode Exit fullscreen mode

If you run reload your browser you'll see an error like this **jinja2.exceptions.TemplateNotFound**

Let's fix it by creating our **templates** folder

Inside **templates** folder create home.html file and add this code.  

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Flask Todo App with Tailwind</title>
      </head>
      <body>
        <h1>Hello Flask</h1>
      </body>
    </html>
    

Enter fullscreen mode Exit fullscreen mode

Reload your browser what you see ?  
Let's add some css with tailwind, in this tutorial i will use the **CDN** version.

Don't use the **CDN** if you want to have all **Tailwind** features.  

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link
          href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css"
          rel="stylesheet"
        />
        <title>Flask Todo App with Tailwind</title>
      </head>
      <body>
        <h1 class="text-center">Hello Flask</h1>
      </body>
    </html>
    

Enter fullscreen mode Exit fullscreen mode

We will use a template from [Tailwind components](https://tailwindcomponents.com/component/todo-list-app) here is the source code

Every website in some point or other will need some custom **css** files, let's add static files.

[](#static-files)Static files
-----------------------------

Create **base.html** file and move all the content of **home.html** into it

base.html  

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link
          href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css"
          rel="stylesheet"
        />
        <link
          rel="stylesheet"
          href="{{ url_for('static', filename='css/style.css') }}"
        />
        <title>
          {% if title %}
          {{ title }}
          {% else %} Flask Todo App with Tailwind
          {% endif %}
        </title>
      </head>
      <body>
        <div
          class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans"
        >
          {% block content %}
    
          {% endblock content %}
        </div>
      </body>
    </html>
    

Enter fullscreen mode Exit fullscreen mode

home.html  

    {% extends "base.html" %} {% block content %}
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
      <div class="mb-4">
        <h1 class="text-grey-darkest">Flask Todo List</h1>
        <div class="flex mt-4">
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
            placeholder="Add Todo"
          />
          <button
            class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal"
          >
            Add
          </button>
        </div>
      </div>
      <div>
        <div class="flex mb-4 items-center">
          <p class="w-full text-grey-darkest">
            Add another component to Tailwind Components
          </p>
          <button
            class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red"
          >
            Remove
          </button>
        </div>
      </div>
    </div>
    {% endblock content %}
    

Enter fullscreen mode Exit fullscreen mode

Everything here is static, let's add dynamic data...

[](#working-with-database-crud)Working with database (CRUD)
-----------------------------------------------------------

We will use **SQlite** as the database adapter  

    from flask import Flask, render_template
    from flask_sqlalchemy import SQLAlchemy  # add
    from datetime import datetime  # add
    
    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///todo.db'  # add
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # add
    db = SQLAlchemy(app)  # add
    
    # add
    
    
    class Task(db.Model):
       id = db.Column(db.Integer, primary_key=True)
       name = db.Column(db.String(80), nullable=False)
       created_at = db.Column(db.DateTime, nullable=False,
                              default=datetime.now)
    
       def __repr__(self):
           return f'Todo : {self.name}'
    
    
    @app.route("/")
    def home():
       return render_template("home.html")
    
    
    if __name__ == "__main__":
       app.run(debug=True)
    
    
    

Enter fullscreen mode Exit fullscreen mode

Let's add some data from Python interpreter(**REPL**)  

    $ python
    Python 3.7.5 (default, Nov 20 2019, 09:21:52)
    [GCC 9.2.1 20191008] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    
    >>> from app import db, Task
    >>> db.create_all()
    >>> new_task = Task(name="Learn Flask")
    >>> db.session.add(new_task)
    >>> db.session.commit()
    >>> tasks = Task.query.all()
    >>> tasks
    [Todo : Learn Flask]
    >>>
    

Enter fullscreen mode Exit fullscreen mode

[](#getting-data)Getting data
-----------------------------

We added our fist todo successfully from the **REPL** let's display the in our template **home.html**  

    from flask import Flask, render_template
    from flask_sqlalchemy import SQLAlchemy  # add
    from datetime import datetime  # add
    
    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///todo.db'  # add
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # add
    db = SQLAlchemy(app)  # add
    
    # add
    
    
    class Task(db.Model):
       id = db.Column(db.Integer, primary_key=True)
       name = db.Column(db.String(80), nullable=False)
       created_at = db.Column(db.DateTime, nullable=False,
                              default=datetime.now)
    
       def __repr__(self):
           return f'Todo : {self.name}'
    
    
    @app.route("/")
    def home():
       tasks = Task.query.order_by(Task.created_at) # add
       return render_template("home.html", tasks=tasks) # add
    
    
    if __name__ == "__main__":
       app.run(debug=True)
    
    

Enter fullscreen mode Exit fullscreen mode

Change your **home.html** file  

    {% extends "base.html" %}
    {% block content %}
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
      <div class="mb-4">
        <h1 class="text-grey-darkest">Flask Todo List</h1>
        <div class="flex mt-4">
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
            placeholder="Add Todo"
          />
          <button
            class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal bg-green-600"
          >
            Add
          </button>
        </div>
      </div>
      <div>
        {% if tasks %}
        {% for task in tasks %}
        <div class="flex mb-4 items-center">
          <p class="w-full text-grey-darkest">
            <a href="#">{{ task.name }}</a>
          </p>
          <button
            class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red bg-red-600"
          >
            Remove
          </button>
        </div>
        {% endfor %}
        {% else %}
        <p class="text-center">No Task to display</p>
        {% endif %}
      </div>
    </div>
    {% endblock content %}
    

Enter fullscreen mode Exit fullscreen mode

Open your browser, you'll see a list of tasks.

[](#add-new-data)Add new data
-----------------------------

Let's create a new task from html templates

Change the home fonction  
app.py  

    from flask import Flask, render_template, request, redirect  # add
    from flask_sqlalchemy import SQLAlchemy  # add
    from datetime import datetime  # add
    
    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///todo.db'  # add
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # add
    db = SQLAlchemy(app)  # add
    
    # add
    
    
    class Task(db.Model):
       id = db.Column(db.Integer, primary_key=True)
       name = db.Column(db.String(80), nullable=False)
       created_at = db.Column(db.DateTime, nullable=False,
                              default=datetime.now)
    
       def __repr__(self):
           return f'Todo : {self.name}'
    
    
    @app.route("/", methods=['POST', 'GET'])
    def home():
       if request.method == "POST": # add
           name = request.form['name']
           new_task = Task(name=name)
           db.session.add(new_task)
           db.session.commit()
           return redirect('/')
       else:
           tasks = Task.query.order_by(Task.created_at).all()  # add
       return render_template("home.html", tasks=tasks)  # add
    
    
    if __name__ == "__main__":
       app.run(debug=True)
    
    

Enter fullscreen mode Exit fullscreen mode

home.html  

    {% extends "base.html" %}
    {% block content %}
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
      <div class="mb-4">
        <h1 class="text-grey-darkest">Flask Todo List</h1>
        <div class="flex mt-4">
          {% include "partials/_form.html" %}
        </div>
      </div>
      <div>
        {% if tasks %}
        {% for task in tasks %}
        <div class="flex mb-4 items-center">
          <p class="w-full text-grey-darkest">
            <a href="#">{{ task.name }}</a>
          </p>
          <button
            class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red bg-red-600"
          >
            Remove
          </button>
        </div>
        {% endfor %}
        {% else %}
    
        <p class="text-center">No Task to display</p>
    
        {% endif %}
      </div>
    </div>
    {% endblock content %}
    

Enter fullscreen mode Exit fullscreen mode

Create a new folder inside **templates**, rename it **partials** and put the form into it.

partials/\_form.html  

    <form action="/" method="post" class="inline-flex">
      <input
        class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
        placeholder="Add Todo"
        type="text"
        name="name"
      />
      <button
        class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal bg-green-600"
        type="submit"
      >
        Add
      </button>
    </form>
    

Enter fullscreen mode Exit fullscreen mode

Now you can add new task, retrieve all tasks, what about remove a task ?  
Let's do it

[](#remove-data)Remove data
---------------------------

In app.py  

    # remove a task
    ...
    @app.route('/delete/<int:id>')
    def delete(id):
       task = Task.query.get_or_404(id)
    
       try:
           db.session.delete(task)
           db.session.commit()
           return redirect('/')
       except Exception:
           return "There was a problem deleting data."
    ...
    

Enter fullscreen mode Exit fullscreen mode

In home.html  

    ...
    <a
      class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red bg-red-600"
      href="/delete/{{ task.id }}"
    >
      Remove
    </a>
    ...
    

Enter fullscreen mode Exit fullscreen mode

It's very simple and intuitive, the last part before moving to production is the update task

[](#update-data)Update data
---------------------------

In app.py  

    ...
    
    # update task
    @app.route('/update/<int:id>', methods=['GET', 'POST'])
    def update(id):
       task = Task.query.get_or_404(id)
    
       if request.method == 'POST':
           task.name = request.form['name']
    
           try:
               db.session.commit()
               return redirect('/')
           except:
               return "There was a problem updating data."
    
       else:
           title = "Update Task"
           return render_template('update.html', title=title, task=task)
    ...
    

Enter fullscreen mode Exit fullscreen mode

In update.html  

    {% extends 'base.html' %}
    {% block content %}
    <div class="mb-4">
      <h1 class="text-grey-darkest">{{ title }}</h1>
      <div class="flex mt-4">
        <form action="/update/{{ task.id }}" method="post" class="inline-flex">
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
            placeholder="Add Todo"
            type="text"
            name="name"
            value="{{ task.name }}"
          />
          <button
            class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal bg-green-600"
            type="submit"
          >
            Update
          </button>
        </form>
      </div>
    </div>
    
    {% endblock content %}
    

Enter fullscreen mode Exit fullscreen mode

Now we have fully functional **Flask** app, but if we wanna show the world our new app ?

Let's deploy the application using **Heroku**

[](#deploy-to-heroku)Deploy to Heroku
-------------------------------------

Initialize a new **git** repository  

    $ git init
    $ touch .gitignore
    ...
    

Enter fullscreen mode Exit fullscreen mode

In **.gitignore** file add :  

    # Created by https://www.gitignore.io/api/flask
    # Edit at https://www.gitignore.io/?templates=flask
    
    ### Flask ###
    instance/*
    !instance/.gitignore
    .webassets-cache
    
    ### Flask.Python Stack ###
    # Byte-compiled / optimized / DLL files
    __pycache__/
    *.py[cod]
    *$py.class
    
    # C extensions
    *.so
    
    # Distribution / packaging
    .Python
    build/
    develop-eggs/
    dist/
    downloads/
    eggs/
    .eggs/
    lib/
    lib64/
    parts/
    sdist/
    var/
    wheels/
    pip-wheel-metadata/
    share/python-wheels/
    *.egg-info/
    .installed.cfg
    *.egg
    MANIFEST
    
    # PyInstaller
    #  Usually these files are written by a python script from a template
    #  before PyInstaller builds the exe, so as to inject date/other infos into it.
    *.manifest
    *.spec
    
    # Installer logs
    pip-log.txt
    pip-delete-this-directory.txt
    
    # Unit test / coverage reports
    htmlcov/
    .tox/
    .nox/
    .coverage
    .coverage.*
    .cache
    nosetests.xml
    coverage.xml
    *.cover
    .hypothesis/
    .pytest_cache/
    
    # Translations
    *.mo
    *.pot
    
    # Scrapy stuff:
    .scrapy
    
    # Sphinx documentation
    docs/_build/
    
    # PyBuilder
    target/
    
    # pyenv
    .python-version
    
    # pipenv
    #   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
    #   However, in case of collaboration, if having platform-specific dependencies or dependencies
    #   having no cross-platform support, pipenv may install dependencies that don't work, or not
    #   install all needed dependencies.
    #Pipfile.lock
    
    # celery beat schedule file
    celerybeat-schedule
    
    # SageMath parsed files
    *.sage.py
    
    # Spyder project settings
    .spyderproject
    .spyproject
    
    # Rope project settings
    .ropeproject
    
    # Mr Developer
    .mr.developer.cfg
    .project
    .pydevproject
    
    # mkdocs documentation
    /site
    
    # mypy
    .mypy_cache/
    .dmypy.json
    dmypy.json
    
    # Pyre type checker
    .pyre/
    
    # End of https://www.gitignore.io/api/flask
    

Enter fullscreen mode Exit fullscreen mode

Create **Procfile** and add :  

    $ touch Procfile
    ...
    

Enter fullscreen mode Exit fullscreen mode

    web: gunicorn app:app
    

Enter fullscreen mode Exit fullscreen mode

Login to **Heroku** with your account  

    $ heroku login
    ...
    

Enter fullscreen mode Exit fullscreen mode

Commit your code to the repository  

    $ git add .
    $ git commit -m "Initial commit"
    ...
    

Enter fullscreen mode Exit fullscreen mode

Create a new app on Heroku  

    heroku create flasktailwindtodo
    

Enter fullscreen mode Exit fullscreen mode

Deploy your app to Heroku  

    $ git push heroku master
    #...
    

Enter fullscreen mode Exit fullscreen mode

Open your browser from the terminal using Heroku  

    $ heroku open
    #...
    

Enter fullscreen mode Exit fullscreen mode

Congrats 😍 See you in next Tutorial