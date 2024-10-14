# Python Types Intro

- [Declaring types](#declaring-types)
  - [Simple types](#simple-types)
  - [Generic types](#generic-types)
  - [Classes as types](#classes-as-types)
- [Pydantic models](#pydantic-models)
- [Type Hints with Metadata Annotations](#type-hints-with-metadata-annotations)

## Declaring types

Types Hints → `name: str`

-   They help the IDE know the types of our variables, so we get help **completion** and also **error checks**.

### Simple types

-   All standard Python types are supported by FastAPI, such as `int`, `float`, `bool`, `bytes` or `str`. These are _simply types_

### Generic types

-   We also have _generic types_, those that can contain other values. Their internal values can have also their own type. These are `dict`, `list`, `set` and `tuple`.
-   The Python module **`typing`** exists to support the declaration of generic types.
-   Syntax may vary depending on the Python version.
-   `list`:

    ```py
    def process_items(items: list[str]):
        for item in items:
            print(item)
    ```

    -   Those internal types in the square brackets are called "type parameters".

-   `tuple` and `set`

    ```py
    def process_items(items_t: tuple[int, int, str], items_s: set[bytes]):
        return items_t, items_s
    ```

-   `dict`

    -   2 parameters: 1st: key, 2nd: values

    ```py
    def process_items(prices: dict[str, float]):
        for item_name, item_price in prices.items():
            print(item_name)
            print(item_price)
    ```

-   Union

    -   You can declare that a variable can be any of several types, for example, an _int_ or a _str_.

        ```py
        def process_item(item: int | str):
            # Int or str
            print(item)
        ```

-   Possibly `None`

    -   You can declare that a value could have a type, like `str`, but that it could also be `None`.

        ```py
        from typing import Optional

        def say_hi(name: Optional[str] = None):
            if name is not None:
                print(f"Hey {name}!")
            else:
                print("Hello World")
        ```

    -   Using `Optional[str]` instead of just str will let the editor help you detect errors where you could be assuming that a value is always a str, when it could actually be None too.
    -   `Optional[Something]` is actually a shortcut for `Union[Something, None]`, they are equivalent.

-   Using `Union` or `Optional`
    -   If you are using a Python version below 3.10, here's a tip from my very subjective point of view:
    -   🚨 Avoid using `Optional[SomeType]`. Instead use `Union[SomeType, None]`.

### Classes as types

-   We can declare a class as the type of a variable.

    ```py
    # Let's say you have a class Person, with a name:

    class Person:
        def __init__(self, name: str):
            self.name = name


    def get_person_name(one_person: Person):
        return one_person.name

    # Notice that this means "one_person is an instance of the class Person".
    ```

## Pydantic models

-   FastAPI 💖 Pydantic.
-   Pydantic is a Python library to perform data validation.
-   You declare the "shape" of the data as classes with attributes. And each attribute has a type.
-   Then you create an instance of that class with some values and it will validate the values, convert them to the appropriate type (if that's the case) and give you an object with all the data.
-   And you get all the editor support with that resulting object.

```py
from datetime import datetime

from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str = "John Doe"
    signup_ts: datetime | None = None
    friends: list[int] = []


external_data = {
    "id": "123",
    "signup_ts": "2017-06-01 12:22",
    "friends": [1, "2", b"3"],
}
user = User(**external_data)
print(user)
# > User id=123 name='John Doe' signup_ts=datetime.datetime(2017, 6, 1, 12, 22) friends=[1, 2, 3]
print(user.id)
# > 123
```

## Type Hints with Metadata Annotations

-   Python also has a feature that allows putting additional metadata in these type hints using Annotated.
-   They doesn't affect the code and for editors and other tools the type is still `str`.
-   They're used to provide FastAPI additional metadata about how the application should behave.
-   **The first type parameter we pass to Annotated is the actual type**. The rest, is just metadata for other tools

```py
from typing import Annotated


def say_hello(name: Annotated[str, "this is just metadata"]) -> str:
    return f"Hello {name}"
```
