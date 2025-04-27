TITLE: Defining a Table Model Class with SQLModel in Python
DESCRIPTION: Creates a Hero class that inherits from SQLModel to represent a database table. The class is configured with table=True and includes fields for id, name, secret_name, and age. The id field is set as the primary key.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/create-db-and-table.md#2025-04-13_snippet_0

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, SQLModel


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
```

----------------------------------------

TITLE: Defining a SQL Table Model in Python with SQLModel
DESCRIPTION: Creates a Hero class that represents a SQL table with id, name, secret_name, and age columns. The class inherits from SQLModel and uses Field for column configurations, including primary key definition and optional values.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/index.md#2025-04-13_snippet_0

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, SQLModel


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
```

----------------------------------------

TITLE: Python Database Model with SQLModel
DESCRIPTION: Complete code example showing how to define a Hero database model class with SQLModel, including field definitions and database setup logic
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs_src/tutorial/create_db_and_table/annotations/en/tutorial003.md#2025-04-13_snippet_0

LANGUAGE: python
CODE:
```
from typing import Optional
from sqlmodel import Field, SQLModel, create_engine

class Hero(SQLModel, table=True):
    id: Optional[int] = Field(primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None

sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"

engine = create_engine(sqlite_url)

def create_db_and_tables():
    SQLModel.metadata.create_all(engine)

if __name__ == "__main__":
    create_db_and_tables()
```

----------------------------------------

TITLE: Defining a SQLModel Table Model in Python
DESCRIPTION: Creates a Hero class that defines both a SQLAlchemy table and a Pydantic model. The model includes id, name, secret_name, and age fields with appropriate types and constraints.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/README.md#2025-04-13_snippet_0

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, SQLModel


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
```

----------------------------------------

TITLE: FastAPI App Initialization with SQLModel
DESCRIPTION: Creates FastAPI application instance and sets up SQLModel database connection with thread-safe configuration.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/simple-hero-api.md#2025-04-13_snippet_1

LANGUAGE: python
CODE:
```
from fastapi import FastAPI
from sqlmodel import Field, Session, SQLModel, create_engine, select

app = FastAPI()
```

----------------------------------------

TITLE: Querying Database Data with SQLModel
DESCRIPTION: Shows how to query data from a database using SQLModel's select function. The example creates a session, builds a simple query with a where clause, executes it to retrieve a Hero by name, and prints the result.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/index.md#2025-04-13_snippet_3

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine, select


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


engine = create_engine("sqlite:///database.db")

with Session(engine) as session:
    statement = select(Hero).where(Hero.name == "Spider-Boy")
    hero = session.exec(statement).first()
    print(hero)
```

----------------------------------------

TITLE: Creating and Writing to a Database with SQLModel in Python
DESCRIPTION: Complete example showing how to define a model, create instances, connect to a SQLite database, create tables, and insert data. Uses Session and engine concepts from SQLAlchemy.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/README.md#2025-04-13_snippet_2

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)


engine = create_engine("sqlite:///database.db")


SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    session.add(hero_1)
    session.add(hero_2)
    session.add(hero_3)
    session.commit()
```

----------------------------------------

TITLE: Complete Database Write Example with SQLModel
DESCRIPTION: A full example showing how to define a model, create instances, initialize a database engine, create tables, and save data to a SQLite database. The code demonstrates the Session usage pattern for database operations.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/index.md#2025-04-13_snippet_2

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)


engine = create_engine("sqlite:///database.db")


SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    session.add(hero_1)
    session.add(hero_2)
    session.add(hero_3)
    session.commit()
```

----------------------------------------

TITLE: Hero API Endpoints Implementation
DESCRIPTION: Implements POST and GET endpoints for creating and reading hero records using SQLModel sessions.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/simple-hero-api.md#2025-04-13_snippet_3

LANGUAGE: python
CODE:
```
app = FastAPI()

@app.on_event("startup")
def on_startup():
    create_tables()

@app.post("/heroes/")
def create_hero(hero: Hero):
    with Session(engine) as session:
        session.add(hero)
        session.commit()
        session.refresh(hero)
        return hero

@app.get("/heroes/")
def read_heroes():
    with Session(engine) as session:
        heroes = session.exec(select(Hero)).all()
        return heroes
```

----------------------------------------

TITLE: Application Entry Point with SQLModel
DESCRIPTION: Main application file showing table creation using imported SQLModel from database module.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/create-db-and-table.md#2025-04-13_snippet_6

LANGUAGE: Python
CODE:
```
# app.py
from .db import engine, SQLModel

SQLModel.metadata.create_all(engine)
```

----------------------------------------

TITLE: Defining Relationship Attributes in Model Classes
DESCRIPTION: Demonstrates how to define relationship attributes in both Team and Hero models to establish bidirectional relationships using back_populates parameter.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/relationship-attributes/define-relationships-attributes.md#2025-04-13_snippet_2

LANGUAGE: python
CODE:
```
from typing import List, Optional

from sqlmodel import Field, Relationship, Session, SQLModel, create_engine, select


class Team(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    headquarters: str

    heroes: List["Hero"] = Relationship(back_populates="team")


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None
    team_id: Optional[int] = Field(default=None, foreign_key="team.id")

    team: Optional[Team] = Relationship(back_populates="heroes")
```

----------------------------------------

TITLE: Defining Multiple SQLModel Classes for Hero Data
DESCRIPTION: Demonstrates creating separate model classes for database operations (Hero), data creation (HeroCreate), and public responses (HeroPublic). Only the Hero class is marked as a database table.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/multiple-models.md#2025-04-13_snippet_0

LANGUAGE: python
CODE:
```
class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None

class HeroCreate(SQLModel):
    name: str
    secret_name: str
    age: Optional[int] = None

class HeroPublic(SQLModel):
    id: int
    name: str
    secret_name: str
    age: Optional[int] = None
```

----------------------------------------

TITLE: Creating Database Engine and Tables in SQLModel
DESCRIPTION: Creates a SQLite database engine and defines a function to create all the tables based on the SQLModel classes. This code establishes the database structure with related tables.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/connect/create-connected-tables.md#2025-04-13_snippet_2

LANGUAGE: python
CODE:
```
import sqlite3

from sqlmodel import SQLModel, create_engine


sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"

engine = create_engine(sqlite_url)


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)
```

----------------------------------------

TITLE: Creating Hero Data with Team Relationships in Python using SQLModel
DESCRIPTION: Function to create sample hero and team data demonstrating many-to-many relationships. Creates teams 'Z-Force' and 'Preventers', and heroes 'Deadpond', 'Rusty-Man', and 'Spider-Boy' with their team associations.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/many-to-many/create-data.md#2025-04-13_snippet_0

LANGUAGE: python
CODE:
```
def create_heroes(session: Session):
    team_z_force = Team(name="Z-Force", headquarters="Sister Margaret's Bar")
    team_preventers = Team(name="Preventers", headquarters="Sharp Tower")

    hero_deadpond = Hero(name="Deadpond", secret_name="Dive Wilson")
    hero_rusty_man = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)
    hero_spider_boy = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")

        deadpond_team_z = HeroTeamLink(
            team=team_z,
            hero=hero_deadpond,
            is_training=False,
        )
        deadpond_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_deadpond,
            is_training=True,
        )
        spider_boy_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_spider_boy,
            is_training=True,
        )
        rusty_man_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_rusty_man,
            is_training=False,
        )

        session.add(deadpond_team_z)
        session.add(deadpond_preventers)
        session.add(spider_boy_preventers)
        session.add(rusty_man_preventers)

        session.commit()
```

----------------------------------------

TITLE: Creating a FastAPI Dependency for Database Session
DESCRIPTION: This snippet shows how to create a FastAPI dependency function that yields a database session. It uses a context manager to ensure proper session handling and cleanup.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/session-with-dependency.md#2025-04-13_snippet_0

LANGUAGE: Python
CODE:
```
def get_session():
    with Session(engine) as session:
        yield session
```

----------------------------------------

TITLE: Updating Multiple Path Operations with Session Dependency
DESCRIPTION: This snippet shows how to update multiple FastAPI path operations to use the get_session dependency. It demonstrates the consistent use of the session parameter across different CRUD operations.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/session-with-dependency.md#2025-04-13_snippet_2

LANGUAGE: Python
CODE:
```
from fastapi import Depends

def get_session():
    with Session(engine) as session:
        yield session

@app.post("/heroes/", response_model=HeroRead)
def create_hero(*, session: Session = Depends(get_session), hero: HeroCreate):
    db_hero = Hero.model_validate(hero)
    session.add(db_hero)
    session.commit()
    session.refresh(db_hero)
    return db_hero

@app.get("/heroes/", response_model=List[HeroRead])
def read_heroes(*, session: Session = Depends(get_session)):
    heroes = session.exec(select(Hero)).all()
    return heroes

@app.get("/heroes/{hero_id}", response_model=HeroRead)
def read_hero(*, session: Session = Depends(get_session), hero_id: int):
    hero = session.get(Hero, hero_id)
    if not hero:
        raise HTTPException(status_code=404, detail="Hero not found")
    return hero

@app.patch("/heroes/{hero_id}", response_model=HeroRead)
def update_hero(*, session: Session = Depends(get_session), hero_id: int, hero: HeroUpdate):
    db_hero = session.get(Hero, hero_id)
    if not db_hero:
        raise HTTPException(status_code=404, detail="Hero not found")
    hero_data = hero.model_dump(exclude_unset=True)
    for key, value in hero_data.items():
        setattr(db_hero, key, value)
    session.add(db_hero)
    session.commit()
    session.refresh(db_hero)
    return db_hero

@app.delete("/heroes/{hero_id}")
def delete_hero(*, session: Session = Depends(get_session), hero_id: int):
    hero = session.get(Hero, hero_id)
    if not hero:
        raise HTTPException(status_code=404, detail="Hero not found")
    session.delete(hero)
    session.commit()
    return {"ok": True}
```

----------------------------------------

TITLE: Querying a Database with SQLModel in Python
DESCRIPTION: Example showing how to query data from the database using SQLModel. Demonstrates creating a select statement with a filter condition and executing it to retrieve a specific hero by name.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/README.md#2025-04-13_snippet_3

LANGUAGE: Python
CODE:
```
from typing import Optional

from sqlmodel import Field, Session, SQLModel, create_engine, select


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


engine = create_engine("sqlite:///database.db")

with Session(engine) as session:
    statement = select(Hero).where(Hero.name == "Spider-Boy")
    hero = session.exec(statement).first()
    print(hero)
```

----------------------------------------

TITLE: Creating Database Engine in Python
DESCRIPTION: Create a single shared engine for the entire application to interact with the database.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs_src/tutorial/select/annotations/en/tutorial002.md#2025-04-13_snippet_2

LANGUAGE: Python
CODE:
```
engine = create_engine("sqlite:///database.db")
```

----------------------------------------

TITLE: Complete SELECT Implementation with SQLModel
DESCRIPTION: A complete example showing how to read data from a database using SQLModel, including model definition, database setup, and query execution.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/select.md#2025-04-13_snippet_10

LANGUAGE: python
CODE:
```
from typing import Optional
from sqlmodel import Field, Session, SQLModel, create_engine, select


class Hero(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    secret_name: str
    age: Optional[int] = None


sqlite_file_name = "database.db"
sqlite_url = f"sqlite:///{sqlite_file_name}"


engine = create_engine(sqlite_url, echo=True)


def create_db_and_tables():
    SQLModel.metadata.create_all(engine)


def create_heroes():
    hero_1 = Hero(name="Deadpond", secret_name="Dive Wilson")
    hero_2 = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")
    hero_3 = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)

    with Session(engine) as session:
        session.add(hero_1)
        session.add(hero_2)
        session.add(hero_3)

        session.commit()


def select_heroes():
    with Session(engine) as session:
        statement = select(Hero)
        results = session.exec(statement)
        for hero in results:
            print(hero)


def main():
    create_db_and_tables()
    create_heroes()
    select_heroes()


if __name__ == "__main__":
    main()
```

----------------------------------------

TITLE: Creating Explicit Link Model Relationships in SQLModel
DESCRIPTION: Code demonstrating how to create heroes, teams, and explicitly link them together using the HeroTeamLink model with additional data like the is_training status.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/many-to-many/link-with-extra-fields.md#2025-04-13_snippet_3

LANGUAGE: python
CODE:
```
def create_heroes():
    with Session(engine) as session:
        team_z = Team(name="Z-Force", headquarters="Sister Margaret's Bar")
        team_preventers = Team(name="Preventers", headquarters="Sharp Tower")

        hero_deadpond = Hero(name="Deadpond", secret_name="Dive Wilson")
        hero_rusty_man = Hero(name="Rusty-Man", secret_name="Tommy Sharp", age=48)
        hero_spider_boy = Hero(name="Spider-Boy", secret_name="Pedro Parqueador")

        deadpond_team_z = HeroTeamLink(
            team=team_z,
            hero=hero_deadpond,
            is_training=False,
        )
        deadpond_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_deadpond,
            is_training=True,
        )
        spider_boy_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_spider_boy,
            is_training=True,
        )
        rusty_man_preventers = HeroTeamLink(
            team=team_preventers,
            hero=hero_rusty_man,
            is_training=False,
        )

        session.add(deadpond_team_z)
        session.add(deadpond_preventers)
        session.add(spider_boy_preventers)
        session.add(rusty_man_preventers)

        session.commit()
```

----------------------------------------

TITLE: Comprehensive API Testing with Database Fixtures
DESCRIPTION: Shows implementation of multiple test cases using both client and session fixtures. Tests include creating heroes, handling invalid data, and reading hero lists.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/tests.md#2025-04-13_snippet_7

LANGUAGE: python
CODE:
```
from sqlmodel import Session
from fastapi.testclient import TestClient
from .main import Hero

def test_read_heroes(session: Session, client: TestClient):
    hero_1 = Hero(name="Deadpond")
    hero_2 = Hero(name="Rusty-Man")
    session.add(hero_1)
    session.add(hero_2)
    session.commit()

    response = client.get("/heroes/")
    data = response.json()

    assert response.status_code == 200
    assert len(data) == 2
    assert data[0]["name"] == "Deadpond"
    assert data[1]["name"] == "Rusty-Man"
```

----------------------------------------

TITLE: Implementing FastAPI Dependency in a Path Operation
DESCRIPTION: This code demonstrates how to use the get_session dependency in a FastAPI path operation. It shows the import of Depends, the dependency declaration in the function parameters, and the usage of the session within the function.
SOURCE: https://github.com/fastapi/sqlmodel/blob/main/docs/tutorial/fastapi/session-with-dependency.md#2025-04-13_snippet_1

LANGUAGE: Python
CODE:
```
from fastapi import Depends

def get_session():
    with Session(engine) as session:
        yield session

@app.post("/heroes/", response_model=HeroRead)
def create_hero(*, session: Session = Depends(get_session), hero: HeroCreate):
    db_hero = Hero.model_validate(hero)
    session.add(db_hero)
    session.commit()
    session.refresh(db_hero)
    return db_hero
