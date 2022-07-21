
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

@app.route('/file', methods=["GET"])
def get_glb():
    # Note: The full url is f"{request.host_url}tmp/file"
    return send_file("tmp/file")

@app.route('/actionName', methods=["POST"])
def my_action():
    files = request.files.getlist('fileName')

    for file in files:
        # do something (i.e. file.save(f"tmp/{file.filename}"))

    return render_template('index.html', ...)

app.run(host="0.0.0.0", port=5000, debug=True)
```

> index.html

```
    <form enctype="multipart/form-data" method="POST" action="/actionName">
        <input type="file" name="fileName">
    </form>

    <a href="/file">Download file</a>
```
