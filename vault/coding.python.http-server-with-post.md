---
id: uL98NfM0tJc74mHvKGPd2
title: HTTP Server with POST
desc: ''
updated: 1644551050932
created: 1644549239324
---

## Dependency
- [Flask](https://flask.palletsprojects.com/en/2.0.x/)

## Project hierarchy
```
templates/
    index.html
app.py
```

## Implementation
> app.py

```
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/')
def upload_photo():
    return render_template('index.html', ...)

@app.route('/actionName', methods=["POST"])
def my_action():
    files = request.files.getlist('fileName')

    for file in files:
        # do something

    return render_template('index.html', ...)

app.run(host="0.0.0.0", port=5000, debug=True)
```

> index.html

```
    <form enctype="multipart/form-data" method="POST" action="/actionName">
        <input type="file" name="fileName">
    </form>
```