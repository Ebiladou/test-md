# How To Test a FastAPI Application

## Introduction
Testing applications built on fastapi is undoubtedly easy. FastAPI provides mechanisms to ensure that tests can be adequetely written and run in for the application. Testing in FastAPI is made possible by Starlette amongst others, which provides an ASGI test client that allows for simulating requests to the FastAPI application. This test client is used by FastAPI's TestClient to send requests and test the API endpoints.


## Testing in FastAPI

### Install Test Dependencies 

For testing, we primarily need two dependencies. 
- HTTPX: The HTTPX sends HTTP requests to the API endpoints, allowing for verification of responses and manipulates the API behaviour. It sends requests, receives responses, and enables the assertions of the status code, response data, and other details. HTTPX is often used through the TestClient or AsyncClient (for asyncronous applications) provided by Starlette. 

- Pytest: Pytest is a python testing framework for writing and running tests. With Pytest, you can verify the code behavior and catch any errors or bugs.

#### Recap on how it works:
Starlette provides the TestClient for sending requests to the FastAPI application. --> The TestClient uses HTTPX to send requests and receive responses. --> Pytest writes the test case (the function body--request) which in turn uses the TestClient to send requests and assert the responses.

To install the dependencies, run: (Starlette comes installed with FastAPI)
` pip install pytest httpx `

### Writing the First Test
We have a blog post API that creates Users, for instance. We can asertain that the endpoint works as expected. That is; creates our user, or fails to, if the email provided is already registered. 

``` 
@router.post("/user", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
async def create_user(user: User):
    existing_email = await User.find_one(User.email == user.email)
    if existing_email:
        raise HTTPException(status_code=400, detail="Email already registered")

    existing_username = await User.find_one(User.username == user.username)
    if existing_username:
        raise HTTPException(status_code=400, detail="Username is already taken")

    user_data = user.model_dump()
    new_user = User(**user_data)
    await new_user.insert()  
    return new_user 

```
 
(the assumption is that there is a conftest file defining the async client).

```
import pytest
from app.main import app
from app.models import User

@pytest.mark.asyncio
async def test_user_create_ok(client):

    user_data = {
        "username": "Big Lads",
        "email": "testuser@test.com",
        "password": "testuser",
        "bio": "Life's tuff"
    }

    response = await client.post(
        url="/api/v1/user/",
        json=user_data
    )

    assert response.status_code == 201
    assert response.json() is not None
    assert response.json()["username"] == user_data["username"]
    assert response.json()["email"] == user_data["email"]
    assert response.json()["bio"] == user_data["bio"]

```

The test replicates the User creation using all our dependencies; `pytest`, `httpx`, and `Starlette` under the hood. 

`pytest` essemtially models the entire user creation following how it is defined in the function. We send the POST request like we would do with a testing tool like Postman. We have Client.post doing that, using the `httpx` to send an HTTP POST request to the `/user/` endpoint of the FastAPI app, allowing us test the full request and response. 

Since FastAPI is built on `Starlette`, Starlette handles the actual routing, request, and response generation during the test. When `AsyncClient` sends the request, Starlette receives it, routes it through FastAPI’s dependency system and executes the `create_user` endpoint, and returns the response, which is then verified by the test.

To verify that our test pass, we use the `assert` statement. The assert statement is from pytest, and we use it to verify that a certain condition is true. If the condition is false, the test fails and we get an error. In the route logic, our status code is `status_code=status.HTTP_201_CREATED` so we assert precisely that in the test. If we get a falsy, the test will fail. 


