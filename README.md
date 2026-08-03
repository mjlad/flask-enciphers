# flask-enciphers

Encrypted session interface for Flask using [enciphers](https://pypi.org/project/enciphers/).

Replaces Flask's default signed cookie session with a fully encrypted one.

> **Version 2.0 note**: updated to match `enciphers` 2.0's AEAD rewrite.
> `ENCIPHERS_STEP` no longer exists — see [CHANGELOG.md](CHANGELOG.md)
> for exactly what changed and why.

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
| `ENCIPHERS_BACKEND` | `str` | `"AES256_GCM"` | `"AES256_GCM"` or `"XCHACHA20_POLY1305"` |
| `ENCIPHERS_KEY` | `int` | random | Secret key, a 128-bit value |
| `ENCIPHERS_KEY_ENV` | `str` | None | Environment variable for key |

> If no `ENCIPHERS_KEY`/`ENCIPHERS_KEY_ENV` is provided, a random key is
> generated at startup — fine for local development, but every process
> in a real deployment needs to share the same key, or sessions won't
> be portable between them.

## Session expiry

If `session.permanent` is set (giving the cookie a real `Max-Age`), the
same expiry is also bound inside the encrypted token itself — a copy of
the cookie can't be replayed past that point even if a client ignores
the cookie's own expiration. A non-permanent session cookie (the
default) carries no `expires_at` either, unchanged from before.

## License

Apache-2.0 — Copyright 2026 Mejlad Alsubaie