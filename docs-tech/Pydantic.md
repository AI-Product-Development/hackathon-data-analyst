TITLE: Generating JSON Schema from a BaseModel
DESCRIPTION: Example demonstrating how to generate JSON schema from a Pydantic BaseModel with various field types including enums, unions, and field customizations. The example shows the structure of the generated schema and how to convert it to a JSON string.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/json_schema.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
import json
from enum import Enum
from typing import Annotated, Union

from pydantic import BaseModel, Field
from pydantic.config import ConfigDict


class FooBar(BaseModel):
    count: int
    size: Union[float, None] = None


class Gender(str, Enum):
    male = 'male'
    female = 'female'
    other = 'other'
    not_given = 'not_given'


class MainModel(BaseModel):
    """
    This is the description of the main model
    """

    model_config = ConfigDict(title='Main')

    foo_bar: FooBar
    gender: Annotated[Union[Gender, None], Field(alias='Gender')] = None
    snap: int = Field(
        default=42,
        title='The Snap',
        description='this is the value of snap',
        gt=30,
        lt=50,
    )


main_model_schema = MainModel.model_json_schema()  # (1)!
print(json.dumps(main_model_schema, indent=2))  # (2)!
"""
{
  "$defs": {
    "FooBar": {
      "properties": {
        "count": {
          "title": "Count",
          "type": "integer"
        },
        "size": {
          "anyOf": [
            {
              "type": "number"
            },
            {
              "type": "null"
            }
          ],
          "default": null,
          "title": "Size"
        }
      },
      "required": [
        "count"
      ],
      "title": "FooBar",
      "type": "object"
    },
    "Gender": {
      "enum": [
        "male",
        "female",
        "other",
        "not_given"
      ],
      "title": "Gender",
      "type": "string"
    }
  },
  "description": "This is the description of the main model",
  "properties": {
    "foo_bar": {
      "$ref": "#/$defs/FooBar"
    },
    "Gender": {
      "anyOf": [
        {
          "$ref": "#/$defs/Gender"
        },
        {
          "type": "null"
        }
      ],
      "default": null
    },
    "snap": {
      "default": 42,
      "description": "this is the value of snap",
      "exclusiveMaximum": 50,
      "exclusiveMinimum": 30,
      "title": "The Snap",
      "type": "integer"
    }
  },
  "required": [
    "foo_bar"
  ],
  "title": "Main",
  "type": "object"
}
"""
```

----------------------------------------

TITLE: Creating Custom Types with Annotated Pattern in Python
DESCRIPTION: Demonstrates how to create a reusable custom data type (PositiveInt) using the annotated pattern with Pydantic. The example validates that integers are greater than zero.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/types.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
from typing import Annotated

from pydantic import Field, TypeAdapter, ValidationError

PositiveInt = Annotated[int, Field(gt=0)]  # (1)!

ta = TypeAdapter(PositiveInt)

print(ta.validate_python(1))
#> 1

try:
    ta.validate_python(-1)
except ValidationError as exc:
    print(exc)
    """
    1 validation error for constrained-int
      Input should be greater than 0 [type=greater_than, input_value=-1, input_type=int]
    """
```

----------------------------------------

TITLE: Using Annotated pattern to add constraints to nested types
DESCRIPTION: Shows how to add validation constraints to elements of a collection type using the Annotated pattern.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/fields.md#2025-04-22_snippet_3

LANGUAGE: python
CODE:
```
from typing import Annotated

from pydantic import BaseModel, Field


class Model(BaseModel):
    int_list: list[Annotated[int, Field(gt=0)]]
    # Valid: [1, 3]
    # Invalid: [-1, 2]
```

----------------------------------------

TITLE: Accessing Validated Data in Field Validators with Python
DESCRIPTION: Demonstrates how to access already validated data in field validators using the ValidationInfo.data property. This example shows a password confirmation validator that checks if password_repeat matches the password field.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/validators.md#2025-04-22_snippet_14

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, ValidationInfo, field_validator


class UserModel(BaseModel):
    password: str
    password_repeat: str
    username: str

    @field_validator('password_repeat', mode='after')
    @classmethod
    def check_passwords_match(cls, value: str, info: ValidationInfo) -> str:
        if value != info.data['password']:
            raise ValueError('Passwords do not match')
        return value
```

----------------------------------------

TITLE: Basic Usage of validate_call Decorator in Python
DESCRIPTION: Demonstrates how to use the validate_call decorator to parse and validate function arguments using type annotations, including type coercion and validation error handling.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/validation_decorator.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
from pydantic import ValidationError, validate_call


@validate_call
def repeat(s: str, count: int, *, separator: bytes = b'') -> bytes:
    b = s.encode()
    return separator.join(b for _ in range(count))


a = repeat('hello', 3)
print(a)
#> b'hellohellohello'

b = repeat('x', '4', separator=b' ')
print(b)
#> b'x x x x'

try:
    c = repeat('hello', 'wrong')
except ValidationError as exc:
    print(exc)
    """
    1 validation error for repeat
    1
      Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='wrong', input_type=str]
    """
```

----------------------------------------

TITLE: Creating a Reusable Type with Validators Using the Annotated Pattern in Python
DESCRIPTION: This code snippet demonstrates creating a reusable custom type with validators using Pydantic's Annotated pattern. It defines an EvenNumber type that validates input values are even integers, then uses it in multiple model classes.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/validators.md#2025-04-22_snippet_8

LANGUAGE: python
CODE:
```
from typing import Annotated

from pydantic import AfterValidator, BaseModel


def is_even(value: int) -> int:
    if value % 2 == 1:
        raise ValueError(f'{value} is not an even number')
    return value


EvenNumber = Annotated[int, AfterValidator(is_even)]


class Model1(BaseModel):
    my_number: EvenNumber


class Model2(BaseModel):
    other_number: Annotated[EvenNumber, AfterValidator(lambda v: v + 2)]


class Model3(BaseModel):
    list_of_even_numbers: list[EvenNumber]  # (1)!
```

----------------------------------------

TITLE: Validating Data with Pydantic Model Methods in Python
DESCRIPTION: Demonstrates the three main validation methods in Pydantic: model_validate() for dictionary data, model_validate_json() for JSON strings, and model_validate_strings() for string value dictionaries. The example shows successful validation as well as handling validation errors for each method.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_11

LANGUAGE: python
CODE:
```
from datetime import datetime
from typing import Optional

from pydantic import BaseModel, ValidationError


class User(BaseModel):
    id: int
    name: str = 'John Doe'
    signup_ts: Optional[datetime] = None


m = User.model_validate({'id': 123, 'name': 'James'})
print(m)
#> id=123 name='James' signup_ts=None

try:
    User.model_validate(['not', 'a', 'dict'])
except ValidationError as e:
    print(e)
    """
    1 validation error for User
      Input should be a valid dictionary or instance of User [type=model_type, input_value=['not', 'a', 'dict'], input_type=list]
    """

m = User.model_validate_json('{"id": 123, "name": "James"}')
print(m)
#> id=123 name='James' signup_ts=None

try:
    m = User.model_validate_json('{"id": 123, "name": 123}')
except ValidationError as e:
    print(e)
    """
    1 validation error for User
    name
      Input should be a valid string [type=string_type, input_value=123, input_type=int]
    """

try:
    m = User.model_validate_json('invalid JSON')
except ValidationError as e:
    print(e)
    """
    1 validation error for User
      Invalid JSON: expected value at line 1 column 1 [type=json_invalid, input_value='invalid JSON', input_type=str]
    """

m = User.model_validate_strings({'id': '123', 'name': 'James'})
print(m)
#> id=123 name='James' signup_ts=None

m = User.model_validate_strings(
    {'id': '123', 'name': 'James', 'signup_ts': '2024-04-01T12:00:00'}
)
print(m)
#> id=123 name='James' signup_ts=datetime.datetime(2024, 4, 1, 12, 0)

try:
    m = User.model_validate_strings(
        {'id': '123', 'name': 'James', 'signup_ts': '2024-04-01'}, strict=True
    )
except ValidationError as e:
    print(e)
    """
    1 validation error for User
    signup_ts
      Input should be a valid datetime, invalid datetime separator, expected `T`, `t`, `_` or space [type=datetime_parsing, input_value='2024-04-01', input_type=str]
    """
```

----------------------------------------

TITLE: Setting default values for model fields
DESCRIPTION: Demonstrates two ways to provide default values for model fields: direct assignment and using the default parameter in Field().
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/fields.md#2025-04-22_snippet_4

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, Field


class User(BaseModel):
    # Both fields aren't required:
    name: str = 'John Doe'
    age: int = Field(default=20)
```

----------------------------------------

TITLE: Creating and Using Generic Models with Python 3.9+ Syntax
DESCRIPTION: Example of creating a reusable HTTP response payload wrapper using generic models in Pydantic with the traditional syntax. Shows how to declare type variables, create a generic model class, and use it with different type parameters.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_14

LANGUAGE: python
CODE:
```
from typing import Generic, TypeVar

from pydantic import BaseModel, ValidationError

DataT = TypeVar('DataT')  # (1)!


class DataModel(BaseModel):
    number: int


class Response(BaseModel, Generic[DataT]):  # (2)!
    data: DataT  # (3)!


print(Response[int](data=1))
#> data=1
print(Response[str](data='value'))
#> data='value'
print(Response[str](data='value').model_dump())
#> {'data': 'value'}

data = DataModel(number=1)
print(Response[DataModel](data=data).model_dump())
#> {'data': {'number': 1}}
try:
    Response[int](data='value')
except ValidationError as e:
    print(e)
    """
    1 validation error for Response[int]
    data
      Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='value', input_type=str]
    """
```

----------------------------------------

TITLE: Implementing Password Matching Validation with Model After Validator in Python
DESCRIPTION: This code snippet demonstrates using a model-level 'after' validator in Pydantic to check if two password fields match after individual field validation. It ensures password consistency before the model is considered valid.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/validators.md#2025-04-22_snippet_10

LANGUAGE: python
CODE:
```
from typing_extensions import Self

from pydantic import BaseModel, model_validator


class UserModel(BaseModel):
    username: str
    password: str
    password_repeat: str

    @model_validator(mode='after')
    def check_passwords_match(self) -> Self:
        if self.password != self.password_repeat:
            raise ValueError('Passwords do not match')
        return self
```

----------------------------------------

TITLE: Defining a Basic Pydantic Model in Python
DESCRIPTION: This snippet demonstrates how to create a basic Pydantic model with required and optional fields, as well as model configuration.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, ConfigDict


class User(BaseModel):
    id: int
    name: str = 'Jane Doe'

    model_config = ConfigDict(str_max_length=10)  # (1)!
```

----------------------------------------

TITLE: Using default_factory for dynamic default values
DESCRIPTION: Shows how to use the default_factory parameter to generate default values dynamically, such as generating a unique ID.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/fields.md#2025-04-22_snippet_5

LANGUAGE: python
CODE:
```
from uuid import uuid4

from pydantic import BaseModel, Field


class User(BaseModel):
    id: str = Field(default_factory=lambda: uuid4().hex)
```

----------------------------------------

TITLE: Overriding Strict Mode for Individual Fields
DESCRIPTION: Shows how to override the global strict mode configuration for individual fields using Field(strict=False), allowing specific fields to perform type coercion even in a model with strict mode enabled.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/strict_mode.md#2025-04-22_snippet_8

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, ConfigDict, Field


class User(BaseModel):
    model_config = ConfigDict(strict=True)

    name: str
    age: int = Field(strict=False)
```

----------------------------------------

TITLE: Creating and Using Generic Models with Python 3.12+ Syntax (PEP 695)
DESCRIPTION: Example showing how to create and use generic models in Pydantic using the new type parameter syntax introduced in Python 3.12 via PEP 695. Demonstrates creating the same HTTP response wrapper with the simplified syntax.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_15

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, ValidationError


class DataModel(BaseModel):
    number: int


class Response[DataT](BaseModel):  # (1)!
    data: DataT  # (2)!


print(Response[int](data=1))
#> data=1
print(Response[str](data='value'))
#> data='value'
print(Response[str](data='value').model_dump())
#> {'data': 'value'}

data = DataModel(number=1)
print(Response[DataModel](data=data).model_dump())
#> {'data': {'number': 1}}
try:
    Response[int](data='value')
except ValidationError as e:
    print(e)
    """
    1 validation error for Response[int]
    data
      Input should be a valid integer, unable to parse string as an integer [type=int_parsing, input_value='value', input_type=str]
    """
```

----------------------------------------

TITLE: Using Serialization Context in Pydantic
DESCRIPTION: Demonstrates how to use context objects in serialization to dynamically control serialization behavior. This example shows filtering text by stopwords that are passed through the context parameter.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/serialization.md#2025-04-22_snippet_11

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, SerializationInfo, field_serializer


class Model(BaseModel):
    text: str

    @field_serializer('text')
    def remove_stopwords(self, v: str, info: SerializationInfo):
        context = info.context
        if context:
            stopwords = context.get('stopwords', set())
            v = ' '.join(w for w in v.split() if w.lower() not in stopwords)
        return v


model = Model.model_construct(**{'text': 'This is an example document'})
print(model.model_dump())  # no context
#> {'text': 'This is an example document'}
print(model.model_dump(context={'stopwords': ['this', 'is', 'an']}))
#> {'text': 'example document'}
print(model.model_dump(context={'stopwords': ['document']}))
#> {'text': 'This is an example'}
```

----------------------------------------

TITLE: Implementing Faux Immutability with Frozen Config
DESCRIPTION: Shows how to make Pydantic models immutable using model_config['frozen'] = True. Demonstrates that direct attribute changes raise errors while nested mutable objects can still be modified.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_28

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, ConfigDict, ValidationError


class FooBarModel(BaseModel):
    model_config = ConfigDict(frozen=True)

    a: str
    b: dict


foobar = FooBarModel(a='hello', b={'apple': 'pear'})

try:
    foobar.a = 'different'
except ValidationError as e:
    print(e)
    """
    1 validation error for FooBarModel
    a
      Instance is frozen [type=frozen_instance, input_value='different', input_type=str]
    """

print(foobar.a)
#> hello
print(foobar.b)
#> {'apple': 'pear'}
foobar.b['apple'] = 'grape'
print(foobar.b)
#> {'apple': 'grape'}
```

----------------------------------------

TITLE: Basic Pydantic Model Validation Example
DESCRIPTION: Shows how to create a User model with various field types including integers, strings, datetime, and nested dictionaries. Demonstrates successful validation and data coercion.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/index.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
from datetime import datetime

from pydantic import BaseModel, PositiveInt


class User(BaseModel):
    id: int
    name: str = 'John Doe'
    signup_ts: datetime | None
    tastes: dict[str, PositiveInt]


external_data = {
    'id': 123,
    'signup_ts': '2019-06-01 12:22',
    'tastes': {
        'wine': 9,
        b'cheese': 7,
        'cabbage': '1',
    },
}

user = User(**external_data)

print(user.id)
#> 123
print(user.model_dump())
```

----------------------------------------

TITLE: Type Coercion with validate_call in Python
DESCRIPTION: Shows how the validate_call decorator coerces parameter types based on function annotations, automatically converting a string date to a date object before function execution.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/validation_decorator.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
from datetime import date

from pydantic import validate_call


@validate_call
def greater_than(d1: date, d2: date, *, include_equal=False) -> date:  # (1)!
    if include_equal:
        return d1 >= d2
    else:
        return d1 > d2


d1 = '2000-01-01'  # (2)!
d2 = date(2001, 1, 1)
greater_than(d1, d2, include_equal=True)
```

----------------------------------------

TITLE: Creating Dynamic Models at Runtime
DESCRIPTION: Demonstrates how to create Pydantic models dynamically using the create_model function. Shows basic field definition, equivalent static models, and how to use advanced features like Field, alias, and annotations.
SOURCE: https://github.com/pydantic/pydantic/blob/main/docs/concepts/models.md#2025-04-22_snippet_21

LANGUAGE: python
CODE:
```
from pydantic import BaseModel, create_model

DynamicFoobarModel = create_model('DynamicFoobarModel', foo=str, bar=(int, 123))

# Equivalent to:


class StaticFoobarModel(BaseModel):
    foo: str
    bar: int = 123

</final_file_content>

IMPORTANT: For any future changes to this file, use the final_file_content shown above as your reference. This content reflects the current state of the file, including any auto-formatting (e.g., if you used single quotes but the formatter converted them to double quotes). Always base your SEARCH/REPLACE operations on this final version to ensure accuracy.

<environment_details>
# VSCode Visible Files
testing/docs/tech-guides/Pydantic.md

# VSCode Open Tabs
session-notes/MCP-dev/initial notes.md
session-notes/MCP-dev/test.md
../../../Users/dabr/OneDrive - Tonkin + Taylor Group Ltd/Documents/Cline/MCP/context7-mcp/Dockerfile
../../../Users/dabr/AppData/Roaming/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json
session-notes/MCP-dev/01-VSCode-local-setup.md
session-notes/MCP-dev/00-Session-Notes.md
session-notes/MCP-dev/04-MCP-Development-Guide.md
session-notes/MCP-dev/05-Sequential-Thinking-Setup.md
session-notes/MCP-dev/06-Brave-Search-Setup.md
session-notes/MCP-dev/07_Knowledge-Graph-Memory-Setup.md
session-notes/MCP-dev/08-Magic-UI-Setup.md
testing/docs/products/02_product_manager_PRD.md
testing/docs/products/03_UX_designer_UIDD.md
testing/docs/products/04_solution_architect_Architecture_Guide.md
testing/docs/products/05_data_architect_Database_Design.md
testing/docs/products/06_senior_api_developer_API_Design.md
testing/docs/products/07_planner_Plan.md
testing/docs/tech-guides/Flutter.md
testing/docs/tech-guides/Python.md
testing/docs/tech-guides/FastAPI.md
testing/docs/tech-guides/SQLModel.md
testing/docs/tech-guides/SQLite.md
testing/docs/tech-guides/aiosqlite.md
testing/docs/tech-guides/httpx.md
testing/docs/tech-guides/n8n.md
testing/docs/tech-guides/Flutter-State-Management.md
testing/docs/tech-guides/Pydantic.md

# Current Time
4/27/2025, 4:47:49 PM (Pacific/Auckland, UTC+12:00)

# Context Window Usage
592,807 / 1,048.576K tokens used (57%)

# Current Mode
ACT MODE
</environment_details>
