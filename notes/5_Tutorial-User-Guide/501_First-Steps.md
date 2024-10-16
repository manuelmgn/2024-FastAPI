# Tutorial User Guide - First Steps

## Very first steps

- FastAPI is a Python class that provides all the functionality for your API.
- FastAPI is a class that inherits directly from Starlette.
- The simplest FastAPI file (`main.py`):

    ```py
    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/")
    async def root():
        return {"message": "Hello World"}
    ```

- Run it
  - `fastapi dev main.py`
  ![fastapi dev main](<../img/fastapi dev main.png>)
  - The app should be located in `http://127.0.0.1:8000` and showing a JSON Response `{"message": "Hello World"}`. 
  - Interactive API docs: `http://127.0.0.1:8000/docs`.
  - Alternative API docs: `http://127.0.0.1:8000/redoc`.


## OpenAPI

- **FastAPI generates a "schema" with all your API using the OpenAPI standard for defining APIs**.
- **"Schema"**
  - A "schema" is a definition or description of something. Not the code that implements it, but just an abstract description.
- **API "schema"**
  - In this case, OpenAPI is a specification that dictates how to define a schema of your API.
  - This schema definition includes your API paths, the possible parameters they take, etc.
- **Data "schema"**
  - The term "schema" might also refer to the shape of some data, like a JSON content.
  - In that case, it would mean the JSON attributes, and data types they have, etc.
- **OpenAPI** and **JSON Schema**
  - OpenAPI defines an API schema for your API. And that schema includes definitions (or "schemas") of the data sent and received by your API using JSON Schema, the standard for JSON data schemas.
- Check the `openapi.json`
  - If you are curious about how the raw OpenAPI schema looks like, FastAPI automatically generates a JSON (schema) with the descriptions of all your API.
  - You can see it directly at: http://127.0.0.1:8000/openapi.json.
  - It will show a JSON starting with something like:

    ```json
    {
        "openapi": "3.1.0",
        "info": {
            "title": "FastAPI",
            "version": "0.1.0"
        },
        "paths": {
            "/items/": {
                "get": {
                    "responses": {
                        "200": {
                            "description": "Successful Response",
                            "content": {
                                "application/json": {
    ```

- What is OpenAPI for
  - The OpenAPI schema is what powers the two interactive documentation systems included.
  - And there are dozens of alternatives, all based on OpenAPI. You could easily add any of those alternatives to your application built with FastAPI.
  - You could also use it to generate code automatically, for clients that communicate with your API. For example, frontend, mobile or IoT applications.

## Step by step

```py
# Importing FastAPI
from fastapi import FastAPI

# Creating a FastAPI "instance".
# This will be the main point of interaction to create all your API
app = FastAPI()

# Creating a path operation
# While building an API, the "path" is the main way to separate "concerns" and "resources".
# Decorators -> @decorator
# Operations -> Using the HTTP Method 'get'

@app.get("/")
# Defining the path operation function
async def root():
    # Returning content
    return {"message": "Hello World"}
```

- Decorators `@decorator`
  - You put it on top of a function. Like a pretty decorative hat (I guess that's where the term came from).
  - A "decorator" takes the function below and does something with it.
  - In our case, this decorator tells FastAPI that the function below corresponds to the path / with an operation get.
  - It is the "path operation decorator".