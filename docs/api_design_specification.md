# API Design Specification: Data Analyst / Query Bot Prototype

## 1. API Overview

This document specifies the API design for the backend **prototype** of the Data Analyst / Query Bot. The API is built using **Python with FastAPI, SQLModel, and SQLite**. Its primary purpose is to receive natural language queries, process them, interact with the database, and return results.

Link to OpenAPI/Swagger documentation: (Will be available when the FastAPI application is running, typically at `/docs` or `/redoc`)

## 2. Project Structure

A possible project structure for the FastAPI application:

```
.
├── main.py             # Main FastAPI application instance
├── database.py         # Database connection and session management
├── models.py           # SQLModel definitions (based on database_design.md)
├── crud.py             # Database interaction functions (Create, Read, Update, Delete)
├── services.py         # Business logic (e.g., query translation)
├── api/
│   ├── __init__.py
│   └── endpoints/
│       └── query.py    # Endpoints related to query processing
└── schemas.py          # Pydantic models for request/response validation
```

## 3. Core Dependencies

-   `fastapi`: The web framework.
-   `uvicorn`: ASGI server to run the FastAPI application.
-   `sqlmodel`: ORM for database interaction and data modeling.
-   `sqlite3` (built-in) or `aiosqlite`: Database driver for SQLite. `aiosqlite` would be needed for asynchronous database operations.
-   `pydantic`: For data validation and serialization (integrated with FastAPI and SQLModel).
-   (Potentially) Libraries for natural language processing and SQL generation (e.g., spaCy, custom logic).

## 4. Authentication & Authorization

Authentication and authorization are **not implemented** in this initial prototype based on the provided prompts. All API endpoints are assumed to be publicly accessible for the prototype phase.

## 5. Pydantic & SQLModel Models

**SQLModel Models:**

Refer to `docs/database_design.md` for the `Query` SQLModel definition. This model will be used for interacting with the SQLite database.

**Pydantic Models:**

These models will be used for validating incoming request bodies and serializing outgoing responses. They will likely mirror the structure of the `Query` SQLModel, potentially excluding certain fields for requests or adding extra fields for responses.

```python
# schemas.py

from typing import Optional
from pydantic import BaseModel
from datetime import datetime

class QueryCreate(BaseModel):
    natural_language_query: str

class QueryResponse(BaseModel):
    id: int
    natural_language_query: str
    generated_sql_query: Optional[str] = None
    query_results: Optional[str] = None
    status: str
    created_at: datetime

    class Config:
        orm_mode = True # Enable ORM mode for easy conversion from SQLModel
```

## 6. API Endpoints

### Query API

-   `Method & Path`: `POST /query/`
    *   `Description`: Submits a natural language query for processing.
    *   `Request Body`: `QueryCreate` (Pydantic model)
    *   `Response(s)`:
        *   `200 OK`: `QueryResponse` (Pydantic model) - Returns the created query object with initial status.
        *   `422 Unprocessable Entity`: Standard FastAPI validation error for invalid input.
    *   `Auth`: No

-   `Method & Path`: `GET /query/{query_id}`
    *   `Description`: Retrieves the status and results of a specific query.
    *   `Path Parameters`: `query_id` (integer)
    *   `Response(s)`:
        *   `200 OK`: `QueryResponse` (Pydantic model) - Returns the query object.
        *   `404 Not Found`: If the query_id does not exist.
    *   `Auth`: No

## 7. Error Handling Strategy

FastAPI's built-in error handling will be used for validation errors (422). Custom exception handlers can be implemented for specific application errors, such as database connection issues or query translation failures, returning appropriate HTTP status codes (e.g., 500 Internal Server Error, 400 Bad Request).

## 8. Key Asynchronous Operations

While FastAPI supports `async`/`await`, standard `sqlite3` is blocking. For the prototype, database operations might need to be run in a separate thread using FastAPI's `run_in_threadpool` or by using an asynchronous SQLite driver like `aiosqlite`. Query translation (natural language to SQL) is a prime candidate for an asynchronous operation as it might be time-consuming.
