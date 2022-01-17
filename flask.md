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

Dynamic page

-->
