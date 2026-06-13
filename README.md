# flask-enciphers

Encrypted session interface for Flask using [enciphers](https://pypi.org/project/enciphers/).

Replaces Flask's default signed cookie session with a fully encrypted one.

## Installation

```bash
pip install flask-enciphers
```

## Usage

```python
from flask import Flask, session
from flask_enciphers import EnciphersSession

app = Flask(__name__)
EnciphersSession(app)

@app.route("/login")
def login():
    session["user_id"] = 1
    return "logged in"
```

### Application Factory Pattern

```python
from flask_enciphers import EnciphersSession

es = EnciphersSession()

def create_app():
    app = Flask(__name__)
    es.init_app(app)
    return app
```

## Configuration

| Key | Type | Default | Description |
|---|---|---|---|
| `ENCIPHERS_STEP` | `int` | random | Encryption step |
| `ENCIPHERS_KEY` | `int` | random | Secret key |
| `ENCIPHERS_KEY_ENV` | `str` | None | Environment variable for key |

> If no configuration is provided, random values are generated at startup.

## License

Apache-2.0 — Copyright 2026 Mejlad Alsubaie