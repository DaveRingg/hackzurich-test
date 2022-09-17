# HackZurich 22 Learning
## React native
### Dependencies
#### Setup Node & Watchmen
```
$ brew install node
$ brew install watchman
```
#### Ruby
React Native uses a .ruby-version file to make sure that your version of Ruby is aligned with what is needed. Currently, macOS 12.5.1 is shipped with Ruby 2.6.8, which is not what is required by React Native. Our suggestion is to install a Ruby version manager and to install the proper version of Ruby in your system.

- rbenv (https://github.com/rbenv/rbenv)

To check current ruby version:
```
$ ruby --version
```

#### Bundler
https://bundler.io

#### Cocoapods
https://cocoapods.org/

### Create new React Application
Uninstall current react native version!
```
$ npm uninstall -g react-native-cli @react-native-community/cli
```
Create new Project
```
$ npx react-native init AwesomeProject
```

### Running your React Native application​
#### Step 1: Start Metro
To start Metro, run npx react-native start inside your React Native project folder:
```
$ npx react-native start
```
#### Step 2: Start your application​
Let Metro Bundler run in its own terminal. Open a new terminal inside your React Native project folder. Run the following:
```
$ npx react-native run-ios
```
#### Edit your application
Hit ⌘R in your iOS Simulator to reload the app and see your changes!


## Fast API
### Requirements
Python 3.7+

FastAPI stands on the shoulders of giants:
- Starlette (https://www.starlette.io/) for the web parts.
- Pydantic (https://pydantic-docs.helpmanual.io/) for the data parts.

### Installation
Create new python env
```
$ python3 -m venv hackzurich-env
```
Activate new env
```
$ source hackzurich-env/bin/activate
```
Install FastAPI
```
$ (hackzurich-env) pip install fastapi
```
Install Install Uvicorn
```
$ (hackzurich-env) pip install "uvicorn[standard]"
```
Create new api file ```main.py```
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
Run it
```
uvicorn main:app --reload
```

Open in the browser http://127.0.0.1:8000/items/5?q=somequery

You will see the JSON response as:
```
{"item_id": 5, "q": "somequery"}
```

#### Interactive API docs
Now go to http://127.0.0.1:8000/docs
You will see the automatic interactive API documentation (provided by Swagger UI)

#### Alternative API docs
And now, go to http://127.0.0.1:8000/redoc
You will see the alternative automatic documentation (provided by ReDoc)

#### Example upgrade
Now modify the file main.py to receive a body from a PUT request.

Declare the body using standard Python types, thanks to Pydantic.

```
from typing import Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float
    is_offer: Union[bool, None] = None


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}


@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```