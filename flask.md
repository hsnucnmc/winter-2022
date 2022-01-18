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
- Flask Family

---

# Introduction

Flask is a lightweight WSGI (Web Server Gateway Interface in Python) web application framework.

It is easy to get started, and it is scalable as well.

[ithelp 鐵人賽](https://ithelp.ithome.com.tw/users/20120263/ironman/4032)

<!--

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