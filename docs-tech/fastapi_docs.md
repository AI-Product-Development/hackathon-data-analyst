# FastAPI Documentation Snippets

## Defining a simple FastAPI app - Python
```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/tutorial/first-steps.md#_snippet_0*

## Running FastAPI with Uvicorn Console
```
uvicorn main:app --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
<span style="color: green;">INFO</span>:     Started reloader process [28720]
<span style="color: green;">INFO</span>:     Started server process [28722]
<span style="color: green;">INFO</span>:     Waiting for application startup.
<span style="color: green;">INFO</span>:     Application startup complete.
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/first-steps.md#_snippet_0*

## Declaring Request Body Parameter (Python)
```
async def create_item(item: Item):
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/body.md#_snippet_2*

## Running FastAPI Development Server with CLI
```
$ fastapi dev main.py

 ╭────────── FastAPI CLI - Development mode ───────────╮
 │                                                     │
 │  Serving at: http://127.0.0.1:8000                  │
 │                                                     │
 │  API docs: http://127.0.0.1:8000/docs               │
 │                                                     │
 │  Running in development mode, for production use:   │
 │                                                     │
 │  fastapi run                                        │
 │                                                     │
 ╰─────────────────────────────────────────────────────╯

INFO:     Will watch for changes in these directories: ['/home/user/code/awesomeapp']
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [2248755] using WatchFiles
INFO:     Started server process [2248757]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/index.md#_snippet_2*

## Implementing Token Header Dependency (Python 3.9+) - Python
```
from typing import Annotated

from fastapi import Header, Depends


async def get_token_header(x_token: Annotated[str, Header()]):
    return x_token
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/tutorial/bigger-applications.md#_snippet_1*

## Defining Basic Sync FastAPI Endpoints (Python)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/index.md#_snippet_0*

## Importing Depends Function - FastAPI Python
```
from fastapi import Depends
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/reference/dependencies.md#_snippet_0*

## Using Annotated for Type Hints with Metadata Python 3.9+
```
from typing import Annotated

some_variable: Annotated[str, "some metadata"] = "a string"
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/python-types.md#_snippet_24*

## Running FastAPI Application with Uvicorn (Console)
```
uvicorn main:app --reload

<span style="color: green;">INFO</span>:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
<span style="color: green;">INFO</span>:     Started reloader process [28720]
<span style="color: green;">INFO:     Started server process [28722]
<span style="color: green;">INFO:     Waiting for application startup.
<span style="color: green;">INFO:     Application startup complete.
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/index.md#_snippet_0*

## Defining Function Dependencies in FastAPI
```
def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/dependencies/classes-as-dependencies.md#_snippet_0*

## Initializing Basic FastAPI App and Endpoints (Python)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}

```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/index.md#_snippet_2*

## Installing Uvicorn Server (console)
```
pip install "uvicorn[standard]"

```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/index.md#_snippet_1*

## Declaring Response Model via Return Type Annotation - FastAPI Python
```
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    price: float
    tags: list[str] = []


app = FastAPI()


@app.get("/items/")
async def read_items() -> list[Item]:
    return [
        {"name": "Foo", "price": 42},
        {"name": "Bar", "price": "baz"},
    ]
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/tutorial/response-model.md#_snippet_0*

## Defining Separate Input and Output Pydantic Models - FastAPI Python
```
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr


class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: str | None = None


class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


app = FastAPI()
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/tutorial/response-model.md#_snippet_4*

## Initializing FastAPI Application and Route - Python
```
from fastapi import FastAPI, APIRouter
from fastapi.openapi.utils import get_openapi

app = FastAPI()

@app.get("/items/")
async def read_items():
    return [{"name": "Foo"}]
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/how-to/extending-openapi.md#_snippet_0*

## Creating Basic FastAPI App (Async)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
async def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/README.md#_snippet_2*

## Installing FastAPI Dependencies (console)
```
pip install "fastapi[standard]"
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/index.md#_snippet_0*

## Validating FastAPI Token Data and Scopes with Pydantic (Python)
```
class TokenData(BaseModel):
    username: str | None = None
    scopes: list[str] = []

# ... inside dependency function ...
try:
    payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
    username: str = payload.get("sub")
    scopes: list[str] = payload.get("scopes", [])
    token_data = schemas.TokenData(scopes=scopes, username=username)
except (JWTError, ValidationError):
    raise credentials_exception
user = fake_users_db.get(token_data.username)
if user is None:
    raise credentials_exception
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/advanced/security/oauth2-scopes.md#_snippet_5*

## Defining and Using Basic Dependency in FastAPI
```
from typing import Annotated

from fastapi import Depends, FastAPI

# Define the dependency function. It behaves like a small path operation.
def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

app = FastAPI()

@app.get("/items/")
# Declare the dependency in a path operation function using Annotated and Depends
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    # The result of common_parameters is available in the 'commons' parameter
    return commons
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/tutorial/dependencies/index.md#_snippet_0*

## Defining Basic Async FastAPI Endpoints (Python)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
async def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/index.md#_snippet_1*

## Installing FastAPI with Standard Dependencies
```
pip install "fastapi[standard]"
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/README.md#_snippet_0*

## Creating Virtual Environment with venv Console
```
$ python -m venv .venv
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/virtual-environments.md#_snippet_1*

## Mixing Required, Default, and Optional Query Params (FastAPI, Python)
```
from typing import Optional
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: str, needy: str, skip: int = 0, limit: Optional[int] = None):
    # Example logic - not specified in text, just demonstrate usage
    item = {"item_id": item_id, "needy": needy, "skip": skip}
    if limit is not None:
        item.update({"limit": limit})
    return item
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/fr/docs/tutorial/query-params.md#_snippet_5*

## Declare Basic Path Parameter in FastAPI
```
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/path-params.md#_snippet_0*

## Building FastAPI Docker Image (Annotated) - Dockerfile
```
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

COPY ./app /code/app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/deployment/docker.md#_snippet_3*

## Defining Async Endpoints in FastAPI (Python)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
async def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}

```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/index.md#_snippet_3*

## Installing FastAPI with Standard Dependencies
```
pip install "fastapi[standard]"
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/README.md#_snippet_0*

## Creating Virtual Environment with venv Console
```
$ python -m venv .venv
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/en/docs/virtual-environments.md#_snippet_1*

## Mixing Required, Default, and Optional Query Params (FastAPI, Python)
```
from typing import Optional
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: str, needy: str, skip: int = 0, limit: Optional[int] = None):
    # Example logic - not specified in text, just demonstrate usage
    item = {"item_id": item_id, "needy": needy, "skip": skip}
    if limit is not None:
        item.update({"limit": limit})
    return item
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/fr/docs/tutorial/query-params.md#_snippet_5*

## Declare Basic Path Parameter in FastAPI
```
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/tutorial/path-params.md#_snippet_0*

## Building FastAPI Docker Image (Annotated) - Dockerfile
```
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

COPY ./app /code/app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/deployment/docker.md#_snippet_3*

## Defining Async Endpoints in FastAPI (Python)
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
async def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}

```
*SOURCE: https://github.com/fastapi/fastapi/blob/master/docs/em/docs/index.md#_snippet_3*
