## 目录

- [框架概述与选型](#框架概述与选型)
- [路由与请求处理](#路由与请求处理)
- [依赖注入](#依赖注入)
- [数据验证与 Pydantic](#数据验证与-pydantic)
- [异步与性能](#异步与性能)
- [中间件与事件](#中间件与事件)
- [认证与授权](#认证与授权)
- [测试与部署](#测试与部署)

---

## 框架概述与选型

### Q1: FastAPI 是什么？它的核心特点有哪些？

FastAPI 是一个现代、高性能的 Python Web 框架，基于 Starlette（异步）和 Pydantic（数据验证）构建，专为构建 API 而设计。

**核心特点**：
1. **高性能**：底层使用 uvloop（基于 libuv），性能接近 Node.js，比原生 asyncio 快 2-4 倍，QPS 可达 10000+
2. **自动生成 API 文档**：基于 OpenAPI 标准，自动生成 Swagger UI（/docs）和 ReDoc（/redoc）
3. **原生异步支持**：`async def` + `await`，I/O 密集场景下极大提升并发能力
4. **类型安全**：通过 Python 类型注解自动完成请求参数校验、序列化、数据转换
5. **依赖注入**：内置 `Depends` 实现组件解耦，代码可测试性强

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

@app.get("/")
async def root():
    return {"message": "Hello FastAPI"}

@app.post("/items/")
async def create_item(item: Item):
    return {"name": item.name, "price": item.price}
```

### Q2: FastAPI vs Django vs Flask 如何选型？

| 维度 | FastAPI | Django | Flask |
|------|---------|--------|-------|
| 定位 | 异步 API 框架 | 全栈 Web 框架 | 轻量微框架 |
| 性能 (QPS) | 10000+ | 5000+ | 3000+ |
| 异步支持 | 原生 async/await | 5.0+ 部分支持 | 需扩展（Flask 3.0+ 支持 async） |
| ORM | 自选（SQLAlchemy 为主） | Django ORM（内置） | 自选 |
| Admin 后台 | 手动 / SQLAdmin | 内置 django-admin | Flask-Admin 扩展 |
| API 文档 | 自动生成 | DRF Spectacular | Flask-RESTX |
| WebSocket | 原生支持 | Django Channels | Flask-SocketIO |

**选型决策**：
- **FastAPI**：新 API 服务、AI/ML 模型服务、高并发微服务
- **Django**：需要 Admin 后台、全栈 Web 应用、CMS、团队有 Django 经验
- **Flask**：极简 API、遗留系统维护、学习目的

### Q3: FastAPI 为什么这么快？

底层组合拳：
1. **Starlette**：轻量级 ASGI 框架，异步事件循环处理
2. **uvloop**：基于 libuv 的事件循环，比 asyncio 原生循环快 2-4 倍
3. **Pydantic**：用 Rust 写的 `pydantic-core` 做数据验证，速度极快
4. **异步 IO**：请求处理中 I/O 等待时释放 CPU，不阻塞事件循环

```python
# 启动时安装 uvloop
import uvloop
import asyncio
asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())
```

---

## 路由与请求处理

### Q4: FastAPI 如何定义路由？路径参数、查询参数、请求体如何区分？

```python
from fastapi import FastAPI, Query, Path, Body

app = FastAPI()

# 路径参数：写在 URL 路径中
@app.get("/users/{user_id}")
async def get_user(user_id: int = Path(..., ge=1)):
    return {"user_id": user_id}

# 查询参数：URL ?key=value
@app.get("/users/")
async def list_users(page: int = Query(1, ge=1), limit: int = Query(10, le=100)):
    return {"page": page, "limit": limit}

# 请求体：POST/PUT/PATCH 的 JSON body
@app.post("/users/")
async def create_user(name: str = Body(...), age: int = Body(...)):
    return {"name": name, "age": age}

# 混合使用
@app.put("/users/{user_id}")
async def update_user(
    user_id: int,
    name: str = Body(...),
    from_admin: bool = Query(False)
):
    return {"user_id": user_id, "name": name, "from_admin": from_admin}
```

**区分规则**：
- URL 路径中定义的 → 路径参数
- 函数参数类型为 Pydantic Model → 请求体
- 基本类型 + 不在路径中 → 查询参数
- 显式声明 `Body()` / `Query()` / `Path()` → 按声明处理

---

## 依赖注入

### Q5: FastAPI 的 Depends 依赖注入原理是什么？有哪些应用场景？

`Depends` 是 FastAPI 的核心设计模式，组件不直接创建依赖对象，而是通过外部注入。请求进入时 FastAPI 自动解析依赖树并注入。

**场景一：数据库连接池注入**

```python
from sqlalchemy.ext.asyncio import AsyncSession

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session  # 请求结束后自动关闭

@app.get("/users/{user_id}")
async def get_user(user_id: int, db: AsyncSession = Depends(get_db)):
    user = await db.get(User, user_id)
    return user
```

**场景二：JWT 权限校验注入**

```python
async def get_current_user(token: str = Depends(oauth2_scheme)):
    payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
    user = await get_user_by_username(payload.get("sub"))
    if not user:
        raise HTTPException(status_code=401)
    return user

@app.get("/me")
async def read_me(current_user = Depends(get_current_user)):
    return current_user
```

**场景三：全局日志记录**

```python
async def log_request(request: Request):
    logger.info(f"{request.method} {request.url.path}")
    yield  # 请求执行
    logger.info(f"完成 {request.url.path}")

app = FastAPI(dependencies=[Depends(log_request)])  # 全局生效
```

**yield 的作用**：`yield` 前面的代码在请求前执行，`yield` 后面的代码在请求完成后执行（类似中间件的 before/after）。

---

## 数据验证与 Pydantic

### Q6: Pydantic 模型如何做数据验证？response_model 的作用？

```python
from pydantic import BaseModel, Field, validator
from typing import Optional
from datetime import datetime

class UserCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=50, description="用户名")
    email: str = Field(..., pattern=r"^\S+@\S+\.\S+$")
    age: int = Field(ge=0, le=150)
    password: str = Field(..., min_length=8)

    @validator("name")
    def name_must_not_be_blank(cls, v):
        if not v.strip():
            raise ValueError("名称不能为空白")
        return v.strip()

class UserResponse(BaseModel):
    id: int
    name: str
    email: str
    created_at: datetime

    class Config:
        from_attributes = True  # 支持 ORM 对象直接转换

# response_model 控制返回字段
@app.post("/users/", response_model=UserResponse)
async def create_user(user: UserCreate):
    db_user = await save_to_db(user)
    return db_user  # 自动过滤掉 password 字段
```

**response_model 的作用**：
- 过滤掉不想暴露的字段（如 password）
- 自动将 ORM 对象转为 Pydantic 模型
- 类型安全，文档自动生成
- 支持嵌套模型、联合类型等复杂结构

---

## 异步与性能

### Q7: async def 和普通 def 在 FastAPI 中有何区别？什么场景用哪个？

| | `async def` | 普通 `def` |
|------|-------------|-----------|
| 执行方式 | 事件循环中调度 | 线程池中执行 |
| I/O 操作 | 不阻塞，可并发 | 阻塞当前线程 |
| 适用场景 | I/O 密集型（数据库查询、外部 API） | CPU 密集型或同步库 |

```python
# async def：I/O 操作不阻塞事件循环
@app.get("/async-users/")
async def get_users():
    users = await db.fetch_all("SELECT * FROM users")  # 异步查询
    return users

# 普通 def：自动在线程池中执行（不阻塞事件循环）
@app.get("/sync-compute/")
def heavy_compute():
    return sum(range(10_000_000))  # CPU 密集，线程池执行
```

**常见误区**：`async def` + 同步阻塞库（如 `requests`）= 整个事件循环被卡住。必须用 `aiohttp` / `httpx` 等异步库替代。

### Q8: 如何避免在 async def 中阻塞事件循环？

```python
import httpx
import asyncio
from concurrent.futures import ThreadPoolExecutor

# 方案一：用异步库替代（推荐）
@app.get("/fetch-async/")
async def fetch_async():
    async with httpx.AsyncClient() as client:
        resp = await client.get("https://api.example.com/data")
        return resp.json()

# 方案二：同步代码丢线程池
@app.get("/fetch-sync/")
async def fetch_sync():
    loop = asyncio.get_running_loop()
    result = await loop.run_in_executor(None, requests.get, "https://api.example.com/data")
    return result.json()
```

---

## 中间件与事件

### Q9: FastAPI 中间件如何实现？有哪些应用场景？

```python
from fastapi import FastAPI, Request
import time

app = FastAPI()

# 请求计时中间件
@app.middleware("http")
async def add_process_time(request: Request, call_next):
    start = time.time()
    response = await call_next(request)
    process_time = time.time() - start
    response.headers["X-Process-Time"] = str(process_time)
    return response

# CORS 中间件
from fastapi.middleware.cors import CORSMiddleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://example.com"],
    allow_methods=["GET", "POST"],
    allow_headers=["*"],
)
```

**应用场景**：请求日志、CORS 处理、限流计数、全局异常捕获、响应头添加。

### Q10: FastAPI 的 on_startup / on_shutdown 事件怎么用？

```python
@app.on_event("startup")
async def startup():
    # 初始化数据库连接池
    await database.connect()
    # 加载模型到内存
    model = load_ml_model()

@app.on_event("shutdown")
async def shutdown():
    await database.disconnect()
```

> FastAPI 新版本推荐用 lifespan 上下文管理器替代 on_event。

---

## 认证与授权

### Q11: FastAPI 如何实现 JWT + OAuth2 认证？

```python
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from passlib.context import CryptContext
from jose import jwt
from datetime import datetime, timedelta

SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"
pwd_context = CryptContext(schemes=["bcrypt"])
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/login")

# 注册
@app.post("/register")
async def register(username: str = Body(...), password: str = Body(...)):
    hashed = pwd_context.hash(password)
    await save_user(username, hashed)
    return {"message": "注册成功"}

# 登录 → 签发 token
@app.post("/login")
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    user = await get_user(form_data.username)
    if not pwd_context.verify(form_data.password, user.hashed_password):
        raise HTTPException(401, "密码错误")
    token = jwt.encode(
        {"sub": user.username, "exp": datetime.utcnow() + timedelta(hours=2)},
        SECRET_KEY, algorithm=ALGORITHM
    )
    return {"access_token": token, "token_type": "bearer"}

# 受保护的接口
@app.get("/me")
async def me(token: str = Depends(oauth2_scheme)):
    payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
    return {"username": payload["sub"]}
```

---

## 测试与部署

### Q12: FastAPI 如何编写测试？

```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_create_user():
    response = client.post("/users/", json={"name": "张三", "email": "zs@test.com", "age": 25, "password": "12345678"})
    assert response.status_code == 200
    data = response.json()
    assert data["name"] == "张三"
    assert "password" not in data  # response_model 过滤了

def test_unauthorized():
    response = client.get("/me")
    assert response.status_code == 401
```

---

来源：FastAPI 官方文档、掘金、腾讯云、面试鸭 mianshiya.com
