# Lab5 Chat CI

An isolated copy of the Lab2 Flask chat application. Backend: `app.py`;
static UI: `frontend/`; API tests: `tests/`.

## Install and launch

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt -r requirements-dev.txt
cp .env.example .env
python app.py
```

Configure a real model API key in `.env` for interactive chat, then open
http://127.0.0.1:5001. The development server runs with debug mode enabled.
Never commit `.env`.

Records default to `data/messages.json`, starting empty when the file is absent.
Set `LAB5_DATA_DIR` to a directory to use a different `messages.json` location:

```sh
LAB5_DATA_DIR=/tmp/lab5-chat python app.py
```

The API provides GET `/`, GET `/api/hello`, POST/GET `/api/messages`, and
PATCH/DELETE `/api/messages/<id>`.

## Tests

```sh
.venv/bin/python -m pytest -q --cov=app --cov-report=term-missing --cov-fail-under=70
```

Tests require no real API key. The fixture supplies a placeholder key, removes
the alternative key, and mocks the model HTTP request with a fixed reply.
Each test sets `LAB5_DATA_DIR` to pytest's temporary directory before importing
or reloading the app, so writes never reach the default chat history. Coverage
measures the full `app.py`; the command fails below 70%.
