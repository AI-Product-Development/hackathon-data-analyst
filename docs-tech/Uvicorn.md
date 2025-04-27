TITLE: Running Uvicorn from Command Line
DESCRIPTION: Example of running Uvicorn from the command line with auto-reloading enabled and a custom port. The ASGI application is specified in the format 'path.to.module:instance.path'.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
$ uvicorn main:app --reload --port 5000
```

----------------------------------------

TITLE: Programmatic Uvicorn Server Execution
DESCRIPTION: Different methods to run Uvicorn programmatically using uvicorn.run() or Server/Config instances.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_2

LANGUAGE: python
CODE:
```
import uvicorn

async def app(scope, receive, send):
    ...

if __name__ == "__main__":
    uvicorn.run("main:app", port=5000, log_level="info")
```

----------------------------------------

TITLE: Creating a Basic ASGI Application in Python
DESCRIPTION: An example of a simple ASGI application that responds with 'Hello, world!' to HTTP requests. This demonstrates the basic structure of an ASGI app with scope, receive, and send parameters.
SOURCE: https://github.com/encode/uvicorn/blob/master/README.md#2025-04-21_snippet_1

LANGUAGE: python
CODE:
```
async def app(scope, receive, send):
    assert scope['type'] == 'http'

    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            (b'content-type', b'text/plain'),
        ],
    })
    await send({
        'type': 'http.response.body',
        'body': b'Hello, world!',
    })
```

----------------------------------------

TITLE: Running Uvicorn Programmatically in Python
DESCRIPTION: Demonstrates how to run Uvicorn directly from within a Python program using uvicorn.run(). It includes configuration options such as host, port, and log level.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_1

LANGUAGE: python
CODE:
```
import uvicorn

class App:
    ...

app = App()

if __name__ == "__main__":
    uvicorn.run("main:app", host="127.0.0.1", port=5000, log_level="info")
```

----------------------------------------

TITLE: Programmatic Uvicorn Configuration
DESCRIPTION: Example of programmatically starting Uvicorn server using the run() function with configuration parameters.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/settings.md#2025-04-21_snippet_1

LANGUAGE: python
CODE:
```
uvicorn.run("main:app", host="0.0.0.0", port=8000)
```

----------------------------------------

TITLE: Running Uvicorn with Gunicorn Worker Class
DESCRIPTION: Demonstrates how to use Gunicorn as a process manager for Uvicorn, utilizing the UvicornWorker class for better production deployment.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_3

LANGUAGE: bash
CODE:
```
gunicorn -w 4 -k uvicorn.workers.UvicornWorker
```

----------------------------------------

TITLE: Creating Simple ASGI Application in Python
DESCRIPTION: Defines a basic ASGI application that responds with 'Hello, world!' to HTTP requests.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_1

LANGUAGE: python
CODE:
```
async def app(scope, receive, send):
    body = "Hello, world!"
    await send(
        {
            "type": "http.response.start",
            "status": 200,
            "headers": [
                [b"content-type", b"text/plain"],
                [b"content-length", len(body)],
            ],
        }
    )
    await send(
        {
            "type": "http.response.body",
            "body": body.encode("utf-8"),
        }
    )
```

----------------------------------------

TITLE: Basic ASGI Application Implementation
DESCRIPTION: Simple ASGI application that returns 'Hello, world!' response with proper HTTP headers.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_1

LANGUAGE: python
CODE:
```
async def app(scope, receive, send):
    assert scope['type'] == 'http'

    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            (b'content-type', b'text/plain'),
            (b'content-length', b'13'),
        ],
    })
    await send({
        'type': 'http.response.body',
        'body': b'Hello, world!',
    })
```

----------------------------------------

TITLE: Running Uvicorn with Multiple Worker Processes
DESCRIPTION: Shows how to run Uvicorn with multiple worker processes using the built-in --workers option for improved performance and resilience.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_2

LANGUAGE: bash
CODE:
```
$ uvicorn main:app --workers 4
```

----------------------------------------

TITLE: Advanced Server Configuration and Async Execution
DESCRIPTION: Example showing how to configure and run Uvicorn server using Config and Server classes in an async context.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_3

LANGUAGE: python
CODE:
```
import asyncio
import uvicorn

async def app(scope, receive, send):
    ...

async def main():
    config = uvicorn.Config("main:app", port=5000, log_level="info")
    server = uvicorn.Server(config)
    await server.serve()

if __name__ == "__main__":
    asyncio.run(main())
```

----------------------------------------

TITLE: Configuring Uvicorn via Command Line
DESCRIPTION: Example of starting Uvicorn server using command line options to specify host and port settings.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/settings.md#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
uvicorn main:app --host 0.0.0.0 --port 8000
```

----------------------------------------

TITLE: Running Uvicorn with HTTPS
DESCRIPTION: Example of running Uvicorn with HTTPS enabled, specifying the paths to the SSL key file and certificate file for secure connections.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_6

LANGUAGE: bash
CODE:
```
$ uvicorn main:app --port 5000 --ssl-keyfile=./key.pem --ssl-certfile=./cert.pem
```

----------------------------------------

TITLE: Running Gunicorn with Uvicorn Worker and HTTPS
DESCRIPTION: Demonstrates how to run Gunicorn with the Uvicorn worker class while enabling HTTPS support by specifying SSL certificate and key files.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_7

LANGUAGE: bash
CODE:
```
$ gunicorn --keyfile=./key.pem --certfile=./cert.pem -k uvicorn.workers.UvicornWorker main:app
```

----------------------------------------

TITLE: Creating Cache-Aware Dockerfile for Python Application
DESCRIPTION: Defines a Dockerfile that uses caching strategies to optimize build times for a Python application using UV and Uvicorn.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_4

LANGUAGE: dockerfile
CODE:
```
FROM python:3.12-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Change the working directory to the `app` directory
WORKDIR /app

# Install dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project

# Copy the project into the image
ADD . /app

# Sync the project
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen

# Run with uvicorn
CMD ["uv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

----------------------------------------

TITLE: Running Uvicorn Server with UV
DESCRIPTION: Starts the Uvicorn server to run the ASGI application using UV.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_3

LANGUAGE: bash
CODE:
```
uv run uvicorn main:app
```

----------------------------------------

TITLE: Running Uvicorn Server from Command Line
DESCRIPTION: Command to start the Uvicorn server with the example application. It specifies the module and application object to run.
SOURCE: https://github.com/encode/uvicorn/blob/master/README.md#2025-04-21_snippet_2

LANGUAGE: shell
CODE:
```
$ uvicorn example:app
```

----------------------------------------

TITLE: Nginx Configuration for Proxying to Uvicorn
DESCRIPTION: Sample Nginx configuration for proxying requests to Uvicorn, including settings for websocket support, static file serving, and proper header forwarding.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_5

LANGUAGE: nginx
CODE:
```
http {
  server {
    listen 80;
    client_max_body_size 4G;

    server_name example.com;

    location / {
      proxy_set_header Host $http_host;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_redirect off;
      proxy_buffering off;
      proxy_pass http://uvicorn;
    }

    location /static {
      # path for static files
      root /path/to/app/static;
    }
  }

  map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
  }

  upstream uvicorn {
    server unix:/tmp/uvicorn.sock;
  }

}
```

----------------------------------------

TITLE: Installing Uvicorn Using pip
DESCRIPTION: Commands for installing Uvicorn with minimal dependencies or with standard features including Cython-based components.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_0

LANGUAGE: shell
CODE:
```
pip install uvicorn

pip install 'uvicorn[standard]'
```

----------------------------------------

TITLE: Installing Uvicorn with pip
DESCRIPTION: Commands to install Uvicorn using pip. The first command installs Uvicorn with minimal dependencies, while the second installs it with Cython-based dependencies and optional extras.
SOURCE: https://github.com/encode/uvicorn/blob/master/README.md#2025-04-21_snippet_0

LANGUAGE: shell
CODE:
```
$ pip install uvicorn
```

LANGUAGE: shell
CODE:
```
$ pip install 'uvicorn[standard]'
```

----------------------------------------

TITLE: Environment Variable Configuration for Uvicorn
DESCRIPTION: Example of configuring Uvicorn using environment variables with the UVICORN_ prefix.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/settings.md#2025-04-21_snippet_2

LANGUAGE: bash
CODE:
```
export UVICORN_HOST="0.0.0.0"
export UVICORN_PORT="8000"
uvicorn main:app
```

----------------------------------------

TITLE: Initializing Python Project with UV
DESCRIPTION: Creates a new Python project using the UV package manager, setting up a basic project structure.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
uv init app
```

----------------------------------------

TITLE: Adding Uvicorn Dependency with UV
DESCRIPTION: Adds Uvicorn as a dependency to the project using the UV package manager.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_2

LANGUAGE: bash
CODE:
```
uv add uvicorn
```

----------------------------------------

TITLE: HTTP Request Handler Example
DESCRIPTION: ASGI application example showing how to handle HTTP requests and return method and path information.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_4

LANGUAGE: python
CODE:
```
async def app(scope, receive, send):
    """
    Echo the method and path back in an HTTP response.
    """
    assert scope['type'] == 'http'

    body = f'Received {scope["method"]} request to {scope["path"]}'
    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            [b'content-type', b'text/plain'],
        ]
    })
    await send({
        'type': 'http.response.body',
        'body': body.encode('utf-8'),
    })
```

----------------------------------------

TITLE: Building and Running Docker Container
DESCRIPTION: Commands to build the Docker image and run the container for the Python application.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_5

LANGUAGE: bash
CODE:
```
docker build -t my-app .
docker run -p 8000:8000 my-app
```

----------------------------------------

TITLE: Streaming Response Implementation
DESCRIPTION: Example demonstrating how to implement streaming HTTP responses with delays between chunks.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_5

LANGUAGE: python
CODE:
```
import asyncio

async def app(scope, receive, send):
    """
    Send a slowly streaming HTTP response back to the client.
    """
    await send({
        'type': 'http.response.start',
        'status': 200,
        'headers': [
            [b'content-type', b'text/plain'],
        ]
    })
    for chunk in [b'Hello', b', ', b'world!']:
        await send({
            'type': 'http.response.body',
            'body': chunk,
            'more_body': True
        })
        await asyncio.sleep(1)
    await send({
        'type': 'http.response.body',
        'body': b'',
    })
```

----------------------------------------

TITLE: Creating Docker Compose Configuration for Development
DESCRIPTION: Defines a Docker Compose configuration for running the application in development mode with hot-reloading.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/docker.md#2025-04-21_snippet_6

LANGUAGE: yaml
CODE:
```
services:
  backend:
    build: .
    ports:
      - "8000:8000"
    environment:
      - UVICORN_RELOAD=true
    volumes:
      - .:/app
    tty: true
```

----------------------------------------

TITLE: Configuring Supervisor for Uvicorn Deployment
DESCRIPTION: Example configuration for using Supervisor as a process manager for Uvicorn, showing how to set up multiple processes and handle logging.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/deployment/index.md#2025-04-21_snippet_4

LANGUAGE: ini
CODE:
```
[supervisord]

[fcgi-program:uvicorn]
socket=tcp://localhost:8000
command=venv/bin/uvicorn --fd 0 main:App
numprocs=4
process_name=uvicorn-%(process_num)d
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
```

----------------------------------------

TITLE: Installing Uvicorn Dependencies with pip
DESCRIPTION: This snippet shows the content of a requirements file for installing Uvicorn and its dependencies using pip. It includes core dependencies, optional packages, testing tools, and documentation generators.
SOURCE: https://github.com/encode/uvicorn/blob/master/requirements.txt#2025-04-21_snippet_0

LANGUAGE: plaintext
CODE:
```
-e .[standard]

# TODO: Remove this after h11 makes a release. By this writing, h11 was on 0.14.0.
# Core dependencies
h11 @ git+https://github.com/python-hyper/h11.git@master

# Explicit optionals
a2wsgi==1.10.8
wsproto==1.2.0
websockets==13.1

# Packaging
build==1.2.2.post1
twine==6.1.0

# Testing
ruff==0.11.2
pytest==8.3.5
pytest-mock==3.14.0
pytest-xdist[psutil]==3.6.1
mypy==1.15.0
types-click==7.1.8
types-pyyaml==6.0.12.20250326
trustme==1.2.1
cryptography==44.0.2
coverage==7.8.0
coverage-conditional-plugin==0.9.0
coverage-enable-subprocess==1.0
httpx==0.28.1

# Documentation
mkdocs==1.6.1
mkdocs-material==9.6.10
```

----------------------------------------

TITLE: Running Daphne ASGI Server
DESCRIPTION: Commands to install and run the Daphne ASGI server as an alternative to Uvicorn. Daphne supports HTTP/1.1, HTTP/2, and WebSockets.
SOURCE: https://github.com/encode/uvicorn/blob/master/README.md#2025-04-21_snippet_3

LANGUAGE: shell
CODE:
```
$ pip install daphne
$ daphne app:App
```

----------------------------------------

TITLE: Running Hypercorn ASGI Server
DESCRIPTION: Commands to install and run the Hypercorn ASGI server, which supports HTTP/1.1, HTTP/2, WebSockets, and the trio async framework.
SOURCE: https://github.com/encode/uvicorn/blob/master/README.md#2025-04-21_snippet_4

LANGUAGE: shell
CODE:
```
$ pip install hypercorn
$ hypercorn app:App
```

----------------------------------------

TITLE: Installing and Running Daphne ASGI Server
DESCRIPTION: Commands to install Daphne using pip and run an ASGI application with it. Daphne is the first ASGI server implementation, originally developed for Django Channels.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_6

LANGUAGE: shell
CODE:
```
pip install daphne
daphne app:App
```

----------------------------------------

TITLE: Installing and Running Hypercorn ASGI Server
DESCRIPTION: Commands to install Hypercorn using pip and run an ASGI application with it. Hypercorn is an ASGI server that supports HTTP/1.1, HTTP/2, HTTP/3, and WebSockets.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/index.md#2025-04-21_snippet_7

LANGUAGE: shell
CODE:
```
pip install hypercorn
hypercorn app:App
```

----------------------------------------

TITLE: Cloning Uvicorn Repository
DESCRIPTION: Instructions for cloning the forked Uvicorn repository to begin development.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/contributing.md#2025-04-21_snippet_0

LANGUAGE: shell
CODE:
```
$ git clone https://github.com/YOUR-USERNAME/uvicorn
```

----------------------------------------

TITLE: Installing Uvicorn Development Dependencies
DESCRIPTION: Commands to install Uvicorn and its development dependencies after cloning.
SOURCE: https://github.com/encode/uvicorn/blob/master/docs/contributing.md#2025-04-21_snippet_1

LANGUAGE: shell
CODE:
```
$ cd uvicorn
$ scripts/install
