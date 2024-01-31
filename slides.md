---
theme: seriph
themeConfig:
  primary: '#709fdc'
background: img/title.png
class: text-center
highlighter: shikiji
presenter: true
exportFilename: slidev-exported
export:
  format: pdf
  timeout: 30000
  dark: false
  withClicks: false
  withToc: false
lineNumbers: true
drawings:
  persist: false
transition: slide-left
mdc: true
aspectRatio: 16/9
infoLine: true
author: Daniel Schwarz
title: Login API
date: 01.02.2024
fonts:
  sans: Helvetica Neue, Roboto
  serif: Roboto Slab
  mono: Fira Code
---

# Login-API

Entwicklung einer RESTful API zur Registrierung und <br>
dem Login von Benutzern mit OAuth2 als Container-Image

<!--
Schönen Guten Morgen ...

Im Rahmen der Umschulung ...

Soll in der Praktukumszeit ...

... ein Projekt dokumantiert und Präsentiert ...

In meinem Fall geht es um ...
-->

---
layout: image-right
image: img/inhalt.png
---

# Inhalt

- Vorstellung
- Unternehmen
- Projektbegin
- Planung
- Entwurf
- Implemetierung
- Qualitätsmanagement
- Übergabe
- Fazit

<!--
Ich werde mit einer...

... persönlichen Vorstellung beginnen
-->

---
layout: image-right
image: img/gh_account.png
---

# Vorstellung

- Daniel Schwarz
- 06.02.1985
- Berlin
- Bürokaufmann
- Angeln, Gaming, IT

<br>
<br>

Github: @Axklen

<!--
Ich heisse Daniel ...
-->

---
layout: image-right
image: img/zancor.png
---

# Praktikumsunternehmen

ZANCOR Consulting & Development UG <br>
Rigaer Straße 49<br>
10247 Berlin<br><br>
www.zancor.de<br><br><br>

- IT-Softwareprojekte
- Consulting
- Schulungen

---
layout: section
---

# Projektbeginn
Eine immer wiederkehrende Aufgabe 

---
layout: image-right
image: img/problem.png
---

# Das Problem

- Login Modul in fast jedem Projekt
- Monolitische Projekte
- keine Versionsverwaltung
- wechselnde Entwickler

---
layout: image
image: img/micros.png
---

# Microservice Architektur

---
layout: image
image: img/git.png
---

# Versionsverwaltung

<style>
h1 {
  color: white
}
</style>
---
layout: center
---

# Ziele

- RESTful API
- Geltende Sicherheitsstandards
- Email-Verifizierung
- Daten in Datenbank Speichern
- Containerimage
- Codequalität
- Versionskontrolle
- auf Cloudinstanz bereitstellen

---
layout: section
---

# Planung
Das Wann und Womit


---
layout: two-cols
---

#  Planung

- 06.11. - 24.11.2024
- max. 80h
- Homeoffice
- keine Bereitstellungen

::right::

#  Phasen

| Projektphasen                | Zeit (in h) |
| --------------- |-----------:|
| Analyse/Planung |      8      |
| Entwurf         |      9      |
| Implementierung |     39      |
| Abnahme         |      4      |
| Dokumentation   |     13      |
| **Gesamt**      |   **73**    |


---
layout: center
---

# Kosten

| Stelle                | Kosten
| --------------- |-----------:|
| Entwickler |      1499,42€      |
| Geschäftsleiter         |      354,84€      |
| Cloud-Server |     2,51€      |
| **Gesamt**      |   **1856,77€**    |


---
layout: center
---

# Armotisation

- 10 Projekte im Jahr 2022
- Kosten: 1643,20€
- Zukünftige Kosten: 410,80€
- Ersparnis: 1232,40€/Jahr

## Amortisationsdauer 

18 Monate

---
layout: center
---

# Nutzwert API

<img src="img/nwb.png">

---
layout: section
---

# Entwurf
Die Eckpfeiler


---
layout: center
---

# Datenmodell

<img src="img/rm.png">


---
layout: center
---

# Endpunkte

- /register –> POST ‑ Benutzerregistrierung ‑ offen
- /login –> POST ‑ Benutzerlogin ‑ offen
- /user/change_email –> POST ‑ Benutzer Email ändern ‑ abgesichert
- /users –> GET ‑ Erhalte eine Liste aller Nutzer ‑ abgesichert
- /user/by_mail –> GET ‑ Erhalte einen Benutzer mit EMail Angabe ‑ abgesichert
- /user/change_pwd –> PUT ‑ Benutzerpasswort erneuern ‑ abgesichert
- /user –> DELETE ‑ Lösche einen Benutzer ‑ abgesichert


---
layout: image-right
image: img/qsm.jpg
---

# Qualitätssichernde Maßnahmen

- Unittests
- Linter
- Formatter
- Typechecking
- Dependency-Management
- automatisiert
- CI/CD
- sichere Verschlüsselung

---
layout: section
---

# Implementierung
Hier gehts zur Sache


---
layout: image-right
image: img/ablauf.jpeg
---

# Ablauf

- Github‑Repositories
- CI / automatisiertes Testen
- Cloud‑Instanz
- Container‑Images Datenbank & API Service
- REST‑API
    - Module Installieren
    - FastAPI
    - Endpunkte
    - OAuth2 Authentifizierung
    - ORM
    - Password‑Hashing
    - Unittests


---
layout: image-right
image: img/gh-actions.png
---

# Continuous Integration

- pytest ‑ Unittest
- deptry ‑ Coverageauswertung
- mypy ‑ Statischer‑Typen‑Checker
- tox ‑ kompatibiltätstest
- ruff ‑ Linter & Formatter
- git-hooks - automatisierung

---
layout: center
---

# Container-Image

```sh
# syntax=docker/dockerfile:1
FROM python:3.11-slim-buster
ENV POETRY_VERSION=1.7.1 \
POETRY_VIRTUALENVS_CREATE=false \
POSTGRE_USER=auchGeheim \
POSTGRE_PASS=geheim \
POSTGRE_DB=postgres \
SECRET_KEY=sehrGeheimerKeyDerMitPythonSecretsErstelltWurde
ENV URL_DATABASE=postgresql://$POSTGRE_USER:$POSTGRE_PASS@localhost
:5432/$POSTGRE_DB
# Install poetry
RUN pip install "poetry==$POETRY_VERSION"
# Copy only requirements to cache them in docker layer
WORKDIR /code
COPY poetry.lock pyproject.toml /code/
# Project initialization:
RUN poetry install --no-interaction --no-ansi --no-root --no-dev
# Copy Python code to the Docker image
COPY login_api /code/login_api/
CMD [ "uvicorn", "login_api.main:app", "--reload", "--port", "8000", "
--host", "0.0.0.0"]
```

---
layout: center
---

# Endpunkte

```python
...
@app.get("/users", tags=["user"])
async def get_all_users() -> None:
    pass
@app.get("/user/by_mail", tags=["user"])
async def get_user_by_mail() -> None:
    pass
@app.post("/register", tags=["user"])
async def register_user() -> None:
    pass
@app.post("/login", tags=["user"])
async def login_user() -> None:
    pass
@app.put("/user/change_email", tags=["user"])
async def update_email() -> None:
    pass
...

```

---
layout: center
---

# Oauth2 - `auth_handler.py`

```python
...
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")

SECRET_KEY = str(getenv("SECRET_KEY"))
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

# data = {"email: user_email"}
def create_access_token(data: dict, expires_delta: Union[timedelta,None] = None) -> str:
    to_encode_data = data.copy()
    if expires_delta:
        expires = datetime.utcnow() + expires_delta
    else:
        expires = datetime.now() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode_data.update({"exp": expires})
    # Accesstoken erstellen
    encoded_jwt = jwt.encode(to_encode_data, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
...
```

---
layout: center
---

# ORM - `models.py`

```python
from datetime import datetime

from sqlalchemy import Boolean, DateTime, Integer, String
from sqlalchemy.orm import DeclarativeBase, mapped_column

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id = mapped_column(Integer, primary_key=True, index=True)
    email = mapped_column(String, unique=True)
    username = mapped_column(String, unique=True)
    hashed_pwd = mapped_column(String)
    is_activated = mapped_column(Boolean, default=False)
    created_at = mapped_column(DateTime, default=datetime.now())
```


---
layout: center
---

# Unit-Tests - `test_main.py`

```python
def test_change_email_no_auth():
    response = client.put("/user/change_email", json={"new_email": "roland@peter.de"})
    assert response.json() == {"detail": "Not authenticated"}
    assert response.status_code == 401


# test Userendpoints
def test_get_all_users_with_auth():
    response = client.get("/users", headers=access_token_header)
    assert response.status_code == 200
    response_data = response.json()
    assert response_data[-1]["email"] == TEST_EMAIL


def test_get_user_by_mail_with_auth():
    url = f"/user/by_mail?email={TEST_EMAIL}"
    response = client.get(url, headers=access_token_header)
    assert response.status_code == 200
    response_data = response.json()
    assert response_data["email"] == TEST_EMAIL
    assert response_data["username"] == TEST_USERNAME
```

---
layout: center
---

# Projektsruktur

<img width="45%" src="img/fs.png">

---
layout: center
---

# Codecovarage

<img src="img/codecov.png">

---
layout: section
---

# Testen & Bereitstellen
Die Zitter-Phase

---
layout: center
---

# Manualles Testen - `swagger-UI`

<img style="width: 65%;" src="img/swagger2.png">

---
layout: center
---

# Bereitstellung

- Containerimage via Github-Package-Registry
- PostgreSQL Container
- Login_API Container
- System-D Service
- Portweiterleitung Cloudfirewall
- Erreichbarkeit sicherstellen

---
layout: section
---

# Übergabe
Fast geschafft

---
layout: two-cols
---

# Übergabe

- finales Meeting
- funtionalität vorgestellt
- schulung
- dokumentation
- repository ubertragen
- Protokoll unterzeichnet

::right::

<img src="img/up.png">


---
layout: section
---

# Fazit
Was habe ich gelernt


---
layout: image-right
image: img/fazit.jpg
---

# Das Negative

- Keine Email-Verifizierung
- Zeitplanung leicht überzogen
- Kein auto-update auf Server

<br>

# Das Positive

- Im maximalen Zeitrahmen
- Alles funtional
- Viel gelernt
- Auftraggeber überaus zufrieden

---
layout: section
---

# Danksagung
Sind Sie noch Wach?
