# 中文版 README.md

---

# FastAPI MongoDB 待办事项清单

## 前置要求

1. FastAPI 基础知识
2. 已安装 Python 
3. 已安装 Mongodb 社区版服务器


## 安装依赖

1. fastAPI
2. uvicorn
3. pymongo

```shell
$ pip install fastapi uvicorn pymongo pymongo[srv]
```

## 创建项目结构


## 编辑 main.py

```python
from fastapi import FastAPI

app = FastAPI()
```

运行服务器

## 编辑 database.py

创建一个名为 database.py 的文件，并添加以下内容

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/mydb")

db = client.todo_app

collection_name = db["todos_app"]
```

**注意： `"mongodb://localhost:27017/mydb**"` 是可选的，因为 MongoClient 默认会连接这个端口和路径。**

如果你想使用 Atlas 或其他在线 MongoDB 平台，请将集群的 URL 替换为 MongoDB 客户端实例化时使用的参数。

## 创建 todos_* 文件

分别在 models、routes 和 schemas 文件夹中创建 todos_* 文件

## 编辑 todos_model.py

```python
from pydantic import BaseModel

class Todo(BaseModel):
    name: str
    description: str
    completed: bool
    date: str
```

## 编辑 todos_schemas.py

```python
def todo_serializer(todo) -> dict:
    return {
        "id": str(todo["_id"]),
        "name": todo["name"],
        "description": todo["description"],
        "completed": todo["completed"],
        "date": todo["date"],
    }

def todos_serializer(todos) -> list:
    return [todo_serializer(todo) for todo in todos]
```


## 构建 CRUD API

```python
from fastapi import APIRouter

from models.todos_model import Todo
from config.database import collection_name

from schemas.todos_schema import todos_serializer, todo_serializer
from bson import ObjectId

todo_api_router = APIRouter()

# 查询
@todo_api_router.get("/")
async def get_todos():
    todos = todos_serializer(collection_name.find())
    return todos

@todo_api_router.get("/{id}")
async def get_todo(id: str):
    return todos_serializer(collection_name.find_one({"_id": ObjectId(id)}))


# 新增
@todo_api_router.post("/")
async def create_todo(todo: Todo):
    _id = collection_name.insert_one(dict(todo))
    return todos_serializer(collection_name.find({"_id": _id.inserted_id}))


# 更新
@todo_api_router.put("/{id}")
async def update_todo(id: str, todo: Todo):
    collection_name.find_one_and_update({"_id": ObjectId(id)}, {
        "$set": dict(todo)
    })
    return todos_serializer(collection_name.find({"_id": ObjectId(id)}))

# 删除
@todo_api_router.delete("/{id}")
async def delete_todo(id: str):
    collection_name.find_one_and_delete({"_id": ObjectId(id)})
    return {"status": "ok"}
```


## 如需视频教程和更多教程，请访问 [我的 YouTube 频道](https://www.youtube.com/channel/UCQf9BYcqr8pzKrY14ZyMsbg)




---

# 英文版的README.md

---

# FastAPI MongoDB Todo List

## Prerequisites

1. FastAPI basics
2. Python installed 
3. Mongodb Community server installed


## Install Dependencies

1. fastAPI
2. uvicorn
3. pymongo

```shell
$ pip install fastapi uvicorn pymongo pymongo[srv]
```

## Create the Project structure


## Edit main.py

```python
from fastapi import FastAPI

app = FastAPI()
```

run the server

## Edit database.py

create a file called database.py and add the following content inside of it

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/mydb")

db = client.todo_app

collection_name = db["todos_app"]
```

**Note: the `"mongodb://localhost:27017/mydb**"` is optional since be default MongoClient will connect to this port and route.**

If you want to use Atlas or an online mongoDB platform, the the URL to the cluster in place to the argument used in the instantiation on the MongoDB client


## Create the todos_*

Create the todos_* files in models, routes and schemas folders respectively

## Edit todos_model.py

```python
from pydantic import BaseModel

class Todo(BaseModel):
    name: str
    description: str
    completed: bool
    date: str
```

## Edit todos_schemas.py

```python
def todo_serializer(todo) -> dict:
    return {
        "id": str(todo["_id"]),
        "name": todo["name"],
        "description": todo["description"],
        "completed": todo["completed"],
        "date": todo["date"],
    }

def todos_serializer(todos) -> list:
    return [todo_serializer(todo) for todo in todos]
```


## Build The CRUD API

```python
from fastapi import APIRouter

from models.todos_model import Todo
from config.database import collection_name

from schemas.todos_schema import todos_serializer, todo_serializer
from bson import ObjectId

todo_api_router = APIRouter()

# retrieve
@todo_api_router.get("/")
async def get_todos():
    todos = todos_serializer(collection_name.find())
    return todos

@todo_api_router.get("/{id}")
async def get_todo(id: str):
    return todos_serializer(collection_name.find_one({"_id": ObjectId(id)}))


# post
@todo_api_router.post("/")
async def create_todo(todo: Todo):
    _id = collection_name.insert_one(dict(todo))
    return todos_serializer(collection_name.find({"_id": _id.inserted_id}))


# update
@todo_api_router.put("/{id}")
async def update_todo(id: str, todo: Todo):
    collection_name.find_one_and_update({"_id": ObjectId(id)}, {
        "$set": dict(todo)
    })
    return todos_serializer(collection_name.find({"_id": ObjectId(id)}))

# delete
@todo_api_router.delete("/{id}")
async def delete_todo(id: str):
    collection_name.find_one_and_delete({"_id": ObjectId(id)})
    return {"status": "ok"}
```


## For The Video Tutorial And More Tutorials Visit [My YouTube Channel](https://www.youtube.com/channel/UCQf9BYcqr8pzKrY14ZyMsbg)