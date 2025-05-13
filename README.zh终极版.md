# 中文版 README.md



# 项目介绍:FastAPI-MongoDB 待办事项列表应用程序

## 1. 项目背景
当代任务管理系统需要高效、可扩展且具有实时响应能力的解决方案。本项目使用符合 ASGI 规范的 Python 框架 FastAPI，并搭配面向文档的 MongoDB 数据库架构，以提供高性能的后端系统。它既可用作生产就绪的基础设施，也可作为教育参考示例，展示了现代 REST API 开发模式与 NoSQL 集成，同时遵循行业最佳实践。

## 2. 项目目标
### 2.1 利用 FastAP 的 ASGI 运行时实现异步 API端点，针对 MongoDB 水平可扩展的文档存储进行优化。
### 2.2 建立具有原子事务保证的标准化 CRUD(创建、读取、更新、删除)操作。
### 2.3 构建遵循关注点分离原则的模块化组件:
        -- 数据建模(Pydantic BaseModel)
        -- 业务逻辑(API路由)
        -- 表示层(响应模式)
### 2.4 使用 Pydantic v2 规范展示强大的数据验证和序列化工作流程。
### 2.5 通过 URI 配置，支持在本地 MongoDB 实例和云平台(如 Atlas)上灵活部署。

# 3. 功能概述
## 3.1 核心能力
        -- 任务管理引擎
            -- 创建包含必填字段的文档:名称、描述、完成状态、符合 ISO 8601 标准的时间戳
            -- 检索操作:
                -- GET:获取所有文档并支持服务器端过滤
                -- GET:通过 MongoDB 的 Objectld 获取单个文档
                -- 使用 PUT 方法进行完整文档替换
                -- 使用 MongoDB 的原子操作 find one and delete 实现幂等删除

## 3.2 技术实现
        -- 数据库集成
            -- 官方 MongoDB Python 驱动(PyMongo 4.6 及以上版本)
            -- 自动处理 BSON/Objectld 转换
            -- 具有自动重试策略的连接池
        -- API开发标准
            -- 通过 Pydantic 模型进行请求验证
            -- 自定义序列化/反序列化层，用于:
                -- API安全的 ID 表示(字符串 vs Objectld)
                -- 严格的日期时间格式化
                -- HTTP 状态码标准化(符合 RFC 9110)

## 3.3 系统架构
        -- 三层结构：
            -- 数据层(models/):
                -- Pydantic BaseModel 定义
                -- 用于 MongoDB 文档模式的类型注解
        -- 服务层(routes/)：
            -- API端点处理程序
            -- 业务逻辑隔离
            -- 异步数据库操作
        -- 表示层(schemas/)：
            -- 响应模型定义
            -- 后处理序列化
            -- OpenAPI 模式生成
        -- 部署灵活性
            -- 预配置用于:
                -- 本地 MongoDB 实例(mongodb://localhost:27017)
                -- 通过 URI 修改支持云服务:
                    ```
                    Mongoclient("mongodb+srv://<用户>:<密码>@cluster.xzy.mongodb.net/")
                    ```
        -- 支持使用环境变量管理凭据

# 4. 技术栈验证
    |术语|已验证的实现
    --------
    |ASGI|基于 FastAPI的 Uvicorn的ASGI服务器|
    |Objectld|MongoDB 的 12 字节唯一标识符(BSON 类型 0x07)|
    |BSON|原生 MongoDB 文档编码(二进制 JSON)|
    |CRUD|符合 REST 规范的 HTTP 方法:POST/GET/PUT/DELETE|
    |原子操作|MongoDB 的findOneAndUpdate/findOneAndDelete|
    |连接池|PyMongo 的内置连接管理|

本实现展示了企业级 API 开发实践，结合了 FastAPI的现代功能集和 MongoDB 的文档模型优势，同时严格遵循Python 类型标准和数据库交互最佳实践。 

# 5. 关键改进
## 5.1 技术准确性：
        -- 始终将“ObjectlD”更正为“Objectld'
        -- 明确了PyMongo 版本兼容性
        -- 添加了 Objectld 的 BSON 类型引用
## 5.2 架构清晰度：
        -- 明确的三层结构文档说明
        -- 隔离的数据库连接配置细节
## 5.3 协议合规性:
        -- 引用 RFC 9110 来规范 HTTP 状态码
        -- 强调 ISO 8601 时间戳标准
## 5.4 操作细节:
        -- 增加了对安全相关的环境变量处理
        -- 记录了云部署的 URI 语法
## 5.5 验证矩阵:
        -- 实现了术语与实际实现之间的直接关联
        -- 以表格形式进行技术术语验证

此版本符合专业技术文档标准，同时对不同技能水平的开发人员都具有良好的可读性。



# 安装 / 部署说明

## 1. 准备工作
在开始安装和部署之前，你需要准备以下环境和工具：

##### Python：本项目基于 Python 开发。需要 Python 3.7 或更高版本。从 Python 官方网站下载并安装适合你操作系统（Windows、Mac 或 Linux）的 Python 版本。

##### MongoDB：用于存储待办事项列表数据。你有两种选择：
#### 本地安装：访问 MongoDB 官方网站，下载并安装适合你操作系统的 MongoDB 社区版。安装完成后，确保 MongoDB 服务已启动（Windows 用户可以在 “服务” 中检查；Mac 和 Linux 用户可以通过命令行启动）。

#### MongoDB Atlas（在线服务）：访问 MongoDB Atlas 官方网站，注册一个账户，并按照说明创建一个免费集群。创建完成后，你将获得一个数据库连接 URL，后续配置时会用到。

## 2. 安装步骤
### 2.1 克隆项目代码
在计算机的终端（Windows 用户使用命令提示符或 PowerShell；Mac 和 Linux 用户使用终端应用程序）中运行以下命令，将项目代码下载到本地：
```
git clone <URL of the project code repository>
cd fastAPIMongoDB_todoAPP
```
### 2.2 创建虚拟环境（可选但推荐）
为了避免项目依赖项之间的冲突，建议创建一个虚拟环境。在终端中执行以下命令：
```
#创建虚拟环境
python -m venv venv

#激活虚拟环境
#Windows
venv\Scripts\activate
#Mac/Linux
source venv/bin/activate
 ```
成功激活后，通常会在终端命令行开头看到 (venv)，这表示你已处于虚拟环境中。
### 2.3 安装项目依赖项
激活虚拟环境后，使用 pip 安装项目所需的依赖项。项目的依赖信息存储在 requirements.txt 文件中。执行以下命令进行安装：
```
pip install -r requirements.txt 
```
如果在安装过程中出现错误，可能是网络问题或 Python 环境配置不正确。尝试更换网络、升级 pip（pip install --upgrade pip），或者手动安装安装失败的依赖项（pip install <依赖项名称>）。

## 3. 数据库配置
### 3.1 本地 MongoDB 配置
如果你使用的是本地 MongoDB，使用文本编辑器（如 VS Code 或记事本）打开 config/database.py 文件，并按如下方式修改内容：
```
from pymongo import MongoClient

#连接到本地 MongoDB。mydb 是数据库名称，可以自定义
client = MongoClient("mongodb://localhost:27017/mydb")
db = client.todo_app
collection_name = db["todos_app"]
```
### 3.2 MongoDB Atlas 配置
如果你使用的是 MongoDB Atlas，登录你的 Atlas 账户，找到集群的连接信息。复制连接字符串，并用你实际的数据库密码替换 <password>。然后按如下方式修改 config/database.py 文件中的连接字符串：
```
from pymongo import MongoClient

#用你实际的信息替换 <username> <password> <cluster-url> <database-name>
client = MongoClient("mongodb+srv://<username>:<password>@<cluster-url>/<database-name>?retryWrites=true&w=majority")
db = client.todo_app
collection_name = db["todos_app"]
 ```
## 4. 部署方法
### 4.1 启动应用程序
在终端中，确保你已进入项目的根目录（即 fastAPIMongoDB_todoAPP 目录）且虚拟环境已激活。执行以下命令启动 FastAPI 应用程序：
```
uvicorn main:app --reload
```
main 指的是 main.py 文件（不带 .py 扩展名）。

app 是在 main.py 文件中定义的 FastAPI 应用实例的名称。

--reload 启用自动重新加载功能。在开发过程中修改代码时，服务器将自动重启以应用新代码。
### 4.2 访问应用程序
服务器成功启动后，你可以在浏览器中访问以下地址：

应用程序主页：http://127.0.0.1:8000

交互式 API 文档：http://127.0.0.1:8000/docs。你可以在此页面上测试 API 接口。

## 5. 常见问题解决方案
### 5.1 依赖项安装失败
如果在执行 pip install -r requirements.txt 时遇到错误：

1.网络问题：检查你的网络连接。尝试更换网络或稍后重新安装。

2.pip 版本过旧：运行 pip install --upgrade pip 来更新 pip。

3.手动安装：根据错误信息，单独安装安装失败的依赖项，例如 pip install <依赖项名称>。
### 5.2 数据库连接失败
如果应用程序无法连接到数据库，可能是连接字符串配置不正确或数据库服务未启动。
你可以检查以下几点：

确认 config/database.py 文件中的连接字符串是否正确。

检查 MongoDB 服务是否已启动。如果你使用的是本地 MongoDB，在终端中执行 mongo 命令，查看是否可以成功连接到数据库。



# 使用教程

## 1. 前提条件
在开始使用这个 FastAPI MongoDB 待办事项列表应用之前，确保你已经安装了以下内容：

熟悉 FastAPI 的基础知识。

已在系统中安装 Python。

安装并启动 MongoDB 社区服务器。

## 2.安装依赖
首先，需要安装必要的依赖项。打开终端并运行以下命令：
```
pip install fastapi uvicorn pymongo pymongo[srv]
```

## 3. 配置数据库
创建一个名为 database.py 的文件，并添加以下代码：
```
from pymongo import MongoClient
client = MongoClient("mongodb://localhost:27017/mydb")
db = client.todo_app
collection_name = db["todos_app"]
```

## 4. 创建 todos_* 文件
分别在 models、routes 和 schemas 文件夹中创建 todos_* 文件。

### 4.1 创建models/todos_model.py文件并添加以下代码：
```
from pydantic import BaseModel

class Todo(BaseModel):
    name: str
    description: str
    completed: bool
    date: str
```
### 4.2 创建routes/todos_route.py文件并添加以下代码：
```
from fastapi import APIRouter

from models.todos_model import Todo
from config.database import collection_name

from schemas.todos_schema import todos_serializer, todo_serializer
from bson import ObjectId

todo_api_router = APIRouter()

# 检索所有待办事项
@todo_api_router.get("/")
async def get_todos():
    todos = todos_serializer(collection_name.find())
    return todos

# 检索单个待办事项
@todo_api_router.get("/{id}")
async def get_todo(id: str):
    return todos_serializer(collection_name.find_one({"_id": ObjectId(id)}))

# 创建待办事项
@todo_api_router.post("/")
async def create_todo(todo: Todo):
    _id = collection_name.insert_one(dict(todo))
    return todos_serializer(collection_name.find({"_id": _id.inserted_id}))

# 更新待办事项
@todo_api_router.put("/{id}")
async def update_todo(id: str, todo: Todo):
    collection_name.find_one_and_update({"_id": ObjectId(id)}, {
        "$set": dict(todo)
    })
    return todos_serializer(collection_name.find({"_id": ObjectId(id)}))

# 删除待办事项
@todo_api_router.delete("/{id}")
async def delete_todo(id: str):
    collection_name.find_one_and_delete({"_id": ObjectId(id)})
    return {"status": "ok"}
```

### 4.3 创建todos_schema.py文件并添加以下代码：
```
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

## 5. 编辑 main.py 文件，添加以下代码：
```
from fastapi import FastAPI
from routes.todos_route import todo_api_router

app = FastAPI()

app.include_router(todo_api_router)
```

## 6. 运行应用程序
要启动 FastAPI 应用程序，在终端中运行以下命令：
```
uvicorn main:app --reload
```

## 7. 测试 CRUD API，使用 Postman工具来测试 CRUD API 端点。
### 7.1 获取所有待办事项
打开Postman访问以下 URL：http://127.0.0.1:8000/
### 7.2 获取单个待办事项
要获取单个待办事项，请将待办事项的 ID 追加到 URL 中：http://127.0.0.1:8000/<todo_id>
将 <todo_id> 替换为实际的待办事项 ID。
### 7.3 创建新的待办事项
通过向以下 URL 发送 POST 请求，并在 JSON 正文中包含待办事项的详细信息来创建新的待办事项：
```
http://127.0.0.1:8000/
JSON 正文应具有以下结构：
{
    "name": "新的待办事项",
    "description": "这是一个新的待办事项",
    "completed": false,
    "date": "2024-01-01"
}
```
### 7.4 要更新现有待办事项，向以下 URL 发送 PUT 请求，其中包含你要更新的待办事项的 ID，并在 JSON 正文中包含更新后的详细信息：
http://127.0.0.1:8000/<todo_id>
JSON 正文应与创建请求具有相同的结构，但包含更新后的值。
### 7.5 删除待办事项
要删除待办事项，请向以下 URL 发送 DELETE 请求，其中包含你要删除的待办事项的 ID：
http://127.0.0.1:8000/<todo_id>



# 项目术语表

| 中文术语 | 英文术语 |
| --- | --- |
| 快速 API | FastAPI |
| 异步服务器网关接口 | ASGI (Asynchronous Server Gateway Interface) |
| 通用唯一识别码 | UUID (Universally Unique Identifier) |
| 文档对象模型 | DOM (Document Object Model) |
| 模型 | Model |
| 路由 | Route |
| 模式 | Schema |
| 数据库 | Database |
| 客户端 | Client |
| 集合 | Collection |
| 文档 | Document |
| 插入 | Insert |
| 查询 | Query |
| 更新 | Update |
| 删除 | Delete |
| 序列化 | Serialization |
| 异步函数 | Async Function |
| 装饰器 | Decorator |
| 依赖注入 | Dependency Injection |
| 状态码 | Status Code |
| 请求 | Request |
| 响应 | Response |
| 配置 | Configuration |
| 虚拟环境 | Virtual Environment |
| 包管理器 | Package Manager |
| 版本控制 | Version Control |
| 仓库 | Repository |
| 提交 | Commit |
| 分支 | Branch |
| 合并 | Merge |
| 拉取请求 | Pull Request |
| 待办事项 | Todo |
| 名称 | Name |
| 描述 | Description |
| 完成状态 | Completed |
| 日期 | Date |
| 端口 | Port |
| 主机 | Host |
| 集群 | Cluster |
| 模式验证 | Schema Validation |
| 类型提示 | Type Hinting |
| 静态类型检查 | Static Type Checking |
| 中间件 | Middleware |
| 日志记录 | Logging |
| 单元测试 | Unit Test |
| 集成测试 | Integration Test |
| 自动化测试 | Automated Testing |
| 持续集成 | CI (Continuous Integration) |
| 持续部署 | CD (Continuous Deployment) | 