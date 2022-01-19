---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Flask

siriuskoan

---

# Outline

- Introduction
- Preparation
- Basic Examples
- Variables
- Functions
- Jinja
- Factory Pattern
- Flask Extensions
  - Flask-Login
  - Flask-WTF
  - Flask-SQLAlchemy
  - Flask-Migrate

---

# Introduction

Flask is a lightweight WSGI (Web Server Gateway Interface in Python) web application framework.

It is easy to get started, and it is scalable as well.

[ithelp 鐵人賽](https://ithelp.ithome.com.tw/users/20120263/ironman/4032)

<!--

HSNU repair system is written in Flask.

prerequisites

- Basic Python programming
- Concept of database
- Basic HTML
- Virtual environment (optional)

-->

---

# Preparation

We need a `requirements.txt` file with the following content

```systemd
Flask
Flask-SQLAlchemy
Flask-Login
Flask-WTF
Flask-Migrate
```

Then `$ pip install -r requirements.txt`.

<!--

You can setup venv if you want, and it is highly recommended.

We will discuss the packages later.

-->

---

# Basic Examples

```python
from flask import Flask

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def index():
    return "Index Page"

app.run(host="127.0.0.1", port=8080, debug=True)
```

<!--

It is very easy, and by running this, you have your first flask application.

1. `@` is decorator.

2. `app = Flask(__name__)` creates a flask app.

3. Route should start with `/`.

4. `app.run()` can run this app, and use `flask run` in command line does the same thing.

-->

---

# Basic Examples

```python
from flask import Flask

app = Flask(__name__)

@app.route("/dynamic/<int:id>", methods=["GET"])
def dynamic_url_page(id):
    return str(id)

app.run(host="127.0.0.1", port=8080, debug=True)
```

<!--

Dynamic page.

Before returning `id`, you have to convert it to string since response can only be `str`, `dict`, `Response` object.

-->

---

# Variables

There are many built-in variables in Flask.

- `current_app` - The current application object.
- `request` - The current request object.

---
layout: two-cols
---

# Variables

::left::

`app.py`
```python
from flask import Flask
from utils import get_url_map

app = Flask(__name__)

@app.route("/")
def url_map():
    get_url_map()
    return ""

app.run(host="127.0.0.1", port=8080)
```

::right::

`utils.py`
```python
from flask import current_app

def get_url_map():
    print(current_app.url_map)
```

<!--

The first is `current_app`.

If we `from app import app`, it will cause circular import, so `current_app` come in handy.

`get_url_map` function will get the url_map (the routes we defined above) of the application.

-->

---

# Variables

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def index_page():
    cookies = request.cookies
	if request.method == "GET":
        print(cookies["user"])
        return "GET /"
    if request.method == "POST":
        return "POST /"

app.run(host="127.0.0.1", port=8080)
```

<!--

Cookies are important in websites.

We can get cookies by access the `request` variable.

Another one that is often used is `request.method`.

There is `request.form` containing the form data, but we don't talk about that today since we'll not use it.

`request` has `get_data` and `get_json` functions, you need them when client sends you payload (in `POST` method).

[Request](https://tedboy.github.io/flask/generated/generated/flask.Request.html)

-->

---

# Functions

There are many useful functions in Flask.

- `make_response`
- `redirect`
- `url_for`
- `abort`
- `render_template`

<!--

We don't talk about `render_template` now, it's for jinja.

-->

---

# Functions

```python
from flask import Flask, make_response

app = Flask(__name__)

@app.route("/set")
def set_cookie():
    response = make_response("")
    response.set_cookie("user", "cat")
    return response

@app.route("/delete")
def delete_cookie():
    response = make_response("")
    response.delete_cookie("user")
    return response

app.run(host="127.0.0.1", port=8080)
```

<!--

We have talked about reading cookies, now let's add and remove cookies.

Here we have `make_response` to make response since sometimes we need to return something more than pure text.

For example, now we need to make a response with cookies.

-->

---

# Functions

```python
from flask import Flask, redirect, url_for

app = Flask(__name__)

@app.route("/google")
def redirect_to_google():
    return redirect("https://google.com")

@app.route("/google2")
def redirect_to_google_2():
    return redirect(url_for("redirect_to_google"))

app.run(host="127.0.0.1", port=8080)
```

<!--

In this example, we have three routes.

Route `/google2` redirects client to `url_for("redirect_to_goole")`.

`redirect_to_google` is function name, and the function is bind to route `/google`, so `url_for("redirect_to_goole")` will be evaluated as `/google`, and thus `/google2` will redirect client to `/google`.

-->

---

# Functions

```python
from flask import abort

@app.route("/dont_see")
def dont_see():
    abort(403)

app.run(host="127.0.0.1", port=8080)
```

<!--

When client tries to access `/dont_see`, Flask app will return status code `403` to him or her.

It is useful when client does something bad, and you can abort the bad action immediately, it has the same effect as `return`.

-->

---

# Jinja

Jinja is a templating engine, and Flask use it.

We can view it as running program in HTML templates.

![](/jinja-logo.png)

<!--

You'll understand better after see the examples.

-->

---

# Jinja

Before getting started, we have to create a folder named `templates` where Flask get templates from.

Besides, we need to create `index.html` where we will put HTML template and Jinja code.

<!--

There are two files in examples, one is `app.py` and the other one is `index.html`.

-->

---
layout: two-cols
---

# Jinja


```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index_page():
    name = "cat"
    return render_template("index.html", username=name)

app.run(host="127.0.0.1", port=8080)
```

```html
<body>
  {{ username }}
</body>
```

<!--

The head part of HTML is been omitted in this example because of lack of space.

You can see the `render_template` function, it is used to let Jinja render HTML.

It will run the code in specified HTML template and return the HTML.

Finally, `return` will give the HTML to client.

The text in `{{ }}` is variable.

Jinja will not touch the other parts without `{}`.

-->

---

# Jinja

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index_page():
    return render_template("index.html", state="running")

app.run(host="127.0.0.1", port=8080)
```

```html
<body>
  {% if state == "running" %}
  <p>running</p>
  {% else %}
  <p>terminated</p>
  {% endif %}
</body>
```

<!--

Next part is `if` statement.

The condition of `if` is in `{% %}`.

-->

---
layout: two-cols
---

# Jinja

::left::

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index_page():
    users = [
        "Mary",
        "Cat",
        "Meow",
        "Harry"
    ]
    return render_template(
      "index.html",
      users=users
    )
app.run(host="127.0.0.1", port=8080)
```

::right::

```html
<body>
  <h1>User list</h1>
  <ul>
    {% for user in users %}
    <li>
      {{ user | upper }}
      {{ loop.index }}
    </li>
    {% endfor %}
  </ul>
</body>
```

<!--

This part is `for`.

`| upper` is filter, the `upper` filter is built-in, it will convert lower letters to upper letters.

You can also create your own filter by using `@app.template_filter()` decorator.

`loop.index` is Jinja built-in variable, it stores the index in loop, starting from 1.

-->

---

# Jinja

```python
from flask import Flask, render_template, flash

app = Flask(__name__)

@app.route("/")
def index_page():
    flash("Wrong!", category="warning")
    return render_template("index.html")

app.config["SECRET_KEY"] = "rc498mt6848"
app.run(host="127.0.0.1", port=8080)
```

```html
<body>
  {% for category, message in get_flashed_messages(with_categories=True) %}
  <div class="{{ category }}">{{ message }}</div>
  {% endfor %}
</body>
```

<!--

`flash` function sends messages to frontend, and let it show the messages.

In Jinja, we can use `get_flashed_messages` to get the messages.

`app.config["SECRET_KEY"]` is a very important line here.

`flash` function stores the message in `session`, and if you use `session`, you have to set `SECRET_KEY` for your application.

-->

---

# Jinja

A website has many pages, like main page, login page, register page, dashboard, etc.

If we create an HTML template for every page, there will be too many same part. For example, `head` tag is almost the same for every page.

Therefore, we can do template inheritance to reduce duplicate parts.

In Jinja, we can create `base.html` as the base template, and all the other HTML templates can inherit from it.

---
layout: two-cols
---

# Jinja

::left::

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>
    {% block title %}{% endblock %}
  </title>
</head>
<body>
  <nav>nav</nav>
  <main>
    {% block content %}
    {% endblock %}
  </main>
  <footer>footer</footer>
</body>
```

::right::

```html
{% extends "base.html" %}

{% block title %}Index Page{% endblock %}

{% block content %}
<p>
    {{ user }}
</p>
{% endblock %}
```

<!--

The file on the left is `base.html`, and every HTML template can inherit from it.

It has full HTML structure.

The `block` statement is a "placeholder", the template inheriting from it can replace the part.

The file on the right is `index.html`.

By using `extends` keyword, it can inherit from `base.html`.

The `block` statement is used to replace the placeholder in `base.html`.

-->

---

# Factory Pattern

Factory Pattern is design pattern.

By following the pattern, we can obtain a new application by calling the same function.

For example, we will create a function called `create_app` later, and we can create different with different arguments.

---

# Factory Pattern

![](/directory.png)

<!--

In Flask, we design an application in factory pattern.

To do this, we have to follow a certain directory structure.

`app/` is the core of the application, it contains three folders, and they are blueprints, which basically consists of many routes.

`static/` is the folder containing CSS, JS files, images, etc.

`templates/` is the folder containing HTML template and jinja files.

`tests/` is the folder containing tests, but we will not talk about this.

`manage.py` contains some Flask commands, but we will not talk about this.

`__init__.py` means the directory is a package and is importable.

-->

---

# Factory Pattern

`app/configs.py`

```python
class Config:
    SECRET_KEY = "cyn54g544mxng"
    DEBUG = True
```

<!--

`config.py` contains all config of the application.

It is under `app/`.

We add different config for different env.

-->

---

# Factory Pattern

`app/__init__.py`

```python
def create_app(env):
    app = Flask(__name__, template_folder="../templates", static_folder="../static")
    app.config.from_object(configs.Config)

    from .main import main_bp
    app.register_blueprint(main_bp)

    from .user import user_bp
    app.register_blueprint(user_bp)

    return app
```

<!--

`create_app` is the core of factory pattern.

It is used to create apps with different config.

`main_bp` and `user_bp` are blueprints, we will see them in next page.

We implement the blueprints in `main/` and `user/`, and then register them to the `app` object.

-->

---

# Factory Pattern

`app/main/__init__.py`

```python
from flask import Blueprint

main_bp = Blueprint("main", __name__)

from . import views
```

`app/main/views.py`

```python
from . import main_bp

@main_bp.route("/", methods=["GET"])
def index_page():
    return "index"
```

<!--

Making a blueprint to bind a route is the same as making `app` to do it.

In this example, if we want to get the URL for `index_page`, we should use `url_for("main.index_page")` instead of `url_for("index_page")`.

-->

---

# Flask Extensions

Flask itself is a simple and lightweight framework, so there are many important functions it does not have.

However, there are many Flask extensions that add functionality to a Flask application.

For example,

- Flask-Login
- Flask-WTF
- Flask-SQLAlchemy
- Flask-Migrate
- Flask-Mail
- Flask-RESTful
- ...

<!--

We'll talk about the first four.

-->

---

# Flask Extensions - Flask-Login

Flask-Login is a Flask extension that provides user session management.

It handles the common tasks of logging in, logging out, and remembering your users’ sessions over extended periods of time.

To user this extension, we must set `SECRET_KEY`.

---

# Flask Extensions - Flask-Login

To use this extension, we have to modify `create_app` function.

`app/__init__.py`

```python
from flask_login import LoginManager
login_manager = LoginManager()
def create_app(env):
    app = Flask(__name__, template_folder="../templates", static_folder="../static")
    app.config.from_object(configs[env])
    login_manager.init_app(app)

    from .main import main_bp
    app.register_blueprint(main_bp)
    from .user import user_bp
    app.register_blueprint(user_bp)
    return app
```

<!--

I think it is the most complex one.

`login_manager` is the core of Flask-Login.

`login_manager.init_app(app)` means init `login_manager` with `app` context.

-->

---

# Flask Extensions - Flask-Login

We need user loader.

`app/__init__.py`

```python
from flask_login import LoginManager, UserMixin
class User(UserMixin):
    pass

@login_manager.user_loader
def load(user_id):
    user_id = int(user_id)
    sessionUser = User()
    sessionUser.id = user.id
    return sessionUser
```

<!--

`user_loader` is the most important function when setting `Flask-Login`.

When user has logined, Flask app may want to get some data of the user, then it needs this function to tell it.

We should get username and other information by `user_id` in database or somewhere else.

-->

---

# Flask Extensions - Flask-Login

```python
from flask import Flask
from flask_login import login_user

@app.route("/login")
def login_page():
    user = User()
    user.id = 1
    login_user(user)
    return "Success"

@app.route("/logout")
def logout_page():
    logout_user()
    return "Success"
```

<!--

Now we have set Flask-Login.

We can use some functions.

`login_user` function is used to login a user.

`logout_user` function is used to logout current user.

-->

---

# Flask Extensions - Flask-WTF

Flask-WTF is a Flask extension that provides a way to create HTML forms.

You can create HTML forms using Python class.

---

# Flask Extensions - Flask-WTF

There is no need to initialize Flask-WTF with Flask application.

We only need to do is create a file called `forms.py` and put some forms into it.

`SECRET_KEY` is needed as well.

<!--

We will explain why we need `SECRET_KEY` later.

-->

---

# Flask Extensions - Flask-WTF

To create a form.

`app/forms.py`

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField
from wtforms.validators import DataRequired

class LoginForm(FlaskForm):
    username = StringField(
        "Username", validators=[DataRequired()], render_kw={"placeholder": "Username"}
    )
    password = PasswordField(
        "Password", validators=[DataRequired()], render_kw={"placeholder": "Password"}
    )
    submit = SubmitField("Login")
```

<!--

Flask-WTF allows developers to use Python class to define an HTML form.

Every variable in the class is a field.

Different field types map to different HTML form types.

`validators` are a series of checkers for client's inputs, and you can create your own.

`render_kw` will render the dict to HTML tag attributes.

-->

---

# Flask Extensions - Flask-WTF

To use the form, we need to have a route.

```python
@app.route("/login", methods=["GET", "POST"])
def login_page():
    form = LoginForm()
    if request.method == "GET":
        return render_template("login.html", form=form)
    else
        if form.validate_on_submit():
            username = form.username.data
            password = form.password.data
            if user := login_auth(username, password):
                login_user(user)
        else:
            print(form.errors)
        return redirect(url_for("login_page"))
```

<!--

First we have to create the `form` instance and pass it to HTML template. We will see that later.

After the form is submitted, we can use `form.validate_on_submit` function to check whether the inputs are valid.

If not valid, we `print` the errors here, but we can also `flash` the error messages.

If valid, we can get the inputs by accessing `form.[field name].data`.

-->

---

# Flask Extensions - Flask-WTF

Then in HTML template.

```html
{% extends "base.html" %}

{% block title %}Login{% endblock %}

{% block content %}
<form action="/login" method="post">
    {{ form.csrf_token }}
    {{ form.username }}
    {{ form.password }}
    {{ form.submit }}
</form>
{% endblock %}
```

<!--

In HTML template, we can use `form.[field name]` to place the field in HTML.

`form.csrf_token` is a token that can prevent csrf, if you are interested, you can search for it. The token is the reason why we need to set `SECRET_KEY`.

-->

---

# Flask Extensions - Flask-SQLAlchemy

Flask-SQLAlchemy is a Flask extension that gives developers full power and flexibility of SQL.

It uses ORM framework.

Flask-SQLAlchemy can support many database systems, like MySQL, SQLite, PostgreSQL, etc.

<!--

We'll talk about ORM first.

-->

---

# Flask Extensions - Flask-SQLAlchemy

ORM, standing for Object Relational Mapper, is a technique that maps database to object in programming language.

Pros
- Preventing SQL injection
- Easy to manipulate SQL
- Better readability

Cons
- Slow
- Some functionalities of SQL cannot be used

---

# Flask Extensions - Flask-SQLAlchemy

![](/orm.png)

<!--

From https://tecky.io/en/blog/SQL%E4%B8%89%E9%83%A8%E6%9B%B2:%E4%BD%A0%E4%B8%8D%E9%9C%80%E8%A6%81ORM/

-->

---

# Flask Extensions - Flask-SQLAlchemy

Before getting started, we have to initialize it.

However, we don't create `db` object in `app/__init__.py`, instead, we put the instance in `app/database/models.py`.

1.  Create `app/database/models.py` and add the following lines.
    ```python
    from flask_sqlalchemy import SQLAlchemy
    db = SQLAlchemy()
    ```

2.  And then we can import `db` in  `app/__init__.py`.
    ```python
    from .models import db
    ```

3.  Add `SQLALCHEMY_DATABASE_URI = "sqlite:///data.db"` to `Config` object.

4.  Finally, we initialize Flask-SQLAlchemy with `app` by adding `db.init_app(app)` in `create_app` function.

---

# Flask Extensions - Flask-SQLAlchemy

We first define a table model. The model is put in `app/database/models.py`.

```python
class Users(db.Model):
    __tablename__ = "users"
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String, unique=True, nullable=False)
    password = db.Column(db.String, nullable=False)
    email = db.Column(db.String, unique=True, nullable=False)
    register_time = db.Column(
        db.DateTime, default=datetime.datetime.now, nullable=False
    )

    def __init__(self, username, password, email):
        self.username = username
        self.password = generate_password_hash(password)
        self.email = email
```

<!--

The table object must inherit from `db.Model`.

As you can see, we can do many things in Python syntax.

-->

---

# Flask Extensions - Flask-SQLAlchemy

Let's see CRUD in Flask-SQLAlchemy.

`C`

```python
def create_user(username, password, email):
    user = Users(username, password, email)
    db.session.add(user)
    db.session.commit()
```

`R`

```python
def get_user_by_name(username):
    user = Users.query.filter_by(username=username).first()
    return user
```


<!--

In create, we create an `Users` object and pass some values to it, and then `user` object is created with reguster_time automatically.
Finally we add the user and commit the change.

In read, we can divide the expression.

- `Users` - The table we want to check
- `query` - `SELECT`
- `filter_by` - `WHERE`
- `first` - The first object in the `SELECT` statement.

-->

---

# Flask Extensions - Flask-SQLAlchemy

`U`

```python
def update_user_email(username, email):
    filter = Users.query.filter_by(id=user_id)
    filter.update({"email": email})
    db.session.commit()
```

`D`

```python
def delete_user(username):
    user = Users.query.filter_by(username=username).first()
    db.session.delete(user)
    db.session.commit()
```

<!--

In update, we do `WHERE` first, and apply the changes to it.

In delete, we do `WHERE` first, and then we take the first object and delete it.

-->

---

# Flask Extensions - Flask-SQLAlchemy

Recall the table we make yesterday.

```sql
CREATE TABLE users (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL UNIQUE,
    level INT NOT NULL
);
```

In Flask-SQLAlchemy, the table is

```python
class users(db.Model):
    __tablename__ = "users"
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String, unique=True, nullable=False)
    level = db.Column(db.Integer, nullable=False)
```

<!--

`AUTO_INCREMENT` is enabled by default when an integer type is assigned as PK.

`db.String` type is a generic type.

-->

---

# Flask Extensions - Flask-Migrate

Flask-Migrate is an extension that handles SQLAlchemy database migrations for Flask applications.

The database operations are made available through the Flask command-line interface.

---

# Flask Extensions - Flask-Migrate

Simply, database migration is the version control of the schema of database.

Every migration is a file with SQL statements, and they specify how to migrate to the version and how to rollback from the version.

It is useful when there are many developers developing a project. Someone can modify the schema and create a migration file, and then the others can use the migration file to migrate their local testing database to newer version.

---

# Flask Extensions - Flask-Migrate

We have to initialize Flask-Migrate first.

1.  Add the following lines to `manage.py`.
    ```python
    from flask_migrate import Migrate
    migrate = Migrate(app, db)
    ```

2.  And then we can try to run command `flask db init` to initialize the migration.

---

# Flask Extensions - Flask-Migrate

After setting up, we can start using it.

1.   Run `flask db migrate -m "description"` to generate a migration file.

2.   Run `flask db upgrade` to migrate to current Flask-SQLAlchemy version.

3.   Edit the table schema.

4.   Run `flask db migrate` to create a new migration file since it detects the schema has been changed.

5.   Run `flask db upgrade` to migrate to the newer version.

6.   If something goes wrong, run `flask db downgrade` to rollback.
