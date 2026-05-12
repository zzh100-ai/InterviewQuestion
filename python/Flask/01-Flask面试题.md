## 目录

- [框架概述](#框架概述)
- [请求与上下文](#请求与上下文)
- [路由与蓝图](#路由与蓝图)
- [扩展生态](#扩展生态)
- [认证与安全](#认证与安全)
- [部署与优化](#部署与优化)

---

## 框架概述

### Q1: Flask 是什么？它和 Django 有什么区别？

Flask 是一个用 Python 编写的轻量级 Web 微框架，基于 Werkzeug（WSGI 工具库）和 Jinja2（模板引擎），核心只提供路由、请求/响应等基础功能，其他能力通过扩展按需添加。

| 维度 | Flask | Django |
|------|-------|--------|
| 定位 | 微框架，按需扩展 | 全栈框架，"电池全含" |
| ORM | 自选（Flask-SQLAlchemy） | 内置 Django ORM |
| Admin | Flask-Admin 扩展 | 内置 django-admin |
| 表单 | Flask-WTF 扩展 | 内置 Forms API |
| 项目结构 | 自由组织 | 约定优于配置（app 模式） |
| 学习曲线 | 平缓 | 较陡 |
| 适用场景 | 微服务、API、小型项目 | 全栈 Web 应用、CMS |

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, Flask!'

if __name__ == '__main__':
    app.run(debug=True)
```

### Q2: Flask 的核心设计哲学是什么？

"微框架，大生态"——核心极简（约 10 个依赖），但通过 Flask 扩展生态可以按需组装出完整的 Web 应用。不像 Django 一次给你全部，Flask 让你自己选每个部件。

---

## 请求与上下文

### Q3: Flask 的请求上下文（Request Context）和应用上下文（Application Context）分别是什么？

Flask 通过上下文机制让全局变量在正确的时机指向正确的值，避免了把 request 对象通过层层参数传递。

| 上下文 | 典型变量 | 生命周期 | 说明 |
|--------|----------|----------|------|
| 应用上下文 | `current_app`、`g` | 请求期间 + 手动推送 | 代表 Flask 应用本身 |
| 请求上下文 | `request`、`session` | 单次请求 | 代表当前 HTTP 请求 |

```python
from flask import Flask, request, g, current_app

app = Flask(__name__)

@app.before_request
def before_request():
    g.user = get_current_user()  # g 在单次请求内共享数据

@app.route('/')
def index():
    # request 自动指向当前请求，无需传参
    return f"Hello, {request.remote_addr}"
```

**为什么需要上下文？** Flask 可以同时运行多个应用（应用分发），也可能一个应用同时处理多个请求。上下文机制确保 `request` 等变量在正确的时机指向正确的对象，对 WSGI 服务器的多线程/多进程模型透明。

### Q4: `g` 对象有什么作用？

`g` 是 Flask 提供的全局命名空间，在单次请求的生命周期内共享数据。常用于在 `before_request` 中加载用户信息，在视图函数中直接使用。

```python
from flask import g

@app.before_request
def load_user():
    g.user = get_current_user_from_db()

@app.route('/dashboard')
def dashboard():
    return f"欢迎 {g.user.name}"
```

---

## 路由与蓝图

### Q5: Flask 的路由装饰器原理是什么？

`@app.route()` 装饰器内部调用 `app.add_url_rule()`，将 URL 规则、视图函数、HTTP 方法注册到应用的 URL map 中。

```python
# 下面两种写法等价
@app.route('/users/<int:user_id>', methods=['GET', 'POST'])
def user(user_id):
    return f"User {user_id}"

def user(user_id):
    return f"User {user_id}"
app.add_url_rule('/users/<int:user_id>', view_func=user, methods=['GET', 'POST'])
```

**路由变量转换器**：`<string:name>`（默认）、`<int:id>`、`<float:value>`、`<path:filepath>`、`<uuid:id>`。

### Q6: Flask 的蓝图（Blueprint）是什么？如何使用？

Blueprint 用于将应用拆分为模块化的组件，每个 Blueprint 可以有独立的路由、模板、静态文件。

```python
# auth.py
from flask import Blueprint

auth_bp = Blueprint('auth', __name__, url_prefix='/auth')

@auth_bp.route('/login')
def login():
    return "登录页面"

@auth_bp.route('/register')
def register():
    return "注册页面"

# app.py
from flask import Flask
from auth import auth_bp

app = Flask(__name__)
app.register_blueprint(auth_bp)  # 注册蓝图
```

**适用场景**：大型应用按功能模块拆分（用户、订单、后台管理等），各模块独立维护。

---

## 扩展生态

### Q7: Flask 有哪些常用的扩展？

| 扩展 | 用途 |
|------|------|
| **Flask-SQLAlchemy** | ORM，数据库操作 |
| **Flask-Migrate** | 数据库迁移（基于 Alembic） |
| **Flask-Login** | 用户会话管理 |
| **Flask-WTF** | 表单验证与 CSRF 保护 |
| **Flask-JWT-Extended** | JWT 认证 |
| **Flask-SocketIO** | WebSocket 支持 |
| **Flask-Caching** | 缓存（支持 Redis/Memcached） |
| **Flask-CORS** | 跨域请求处理 |
| **Flask-RESTful / Flask-RESTX** | RESTful API 构建 |
| **Flask-Mail** | 邮件发送 |

```python
# Flask-SQLAlchemy 示例
from flask_sqlalchemy import SQLAlchemy

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True)
```

### Q8: Flask 中请求钩子有哪些？执行顺序是什么？

```python
@app.before_first_request  # 第一个请求前（已废弃，改用 before_app_first_request 扩展）
@app.before_request         # 每次请求前
@app.after_request          # 每次请求后（无异常时）
@app.teardown_request       # 每次请求后（无论有无异常）
```

执行顺序：`before_request` → 视图函数 → `after_request` → `teardown_request`。

---

## 认证与安全

### Q9: Flask 如何实现 JWT 认证？

```python
from flask import Flask, request, jsonify
import jwt
from datetime import datetime, timedelta

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your-secret-key'

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    # 验证用户名密码...
    token = jwt.encode({
        'user': username,
        'exp': datetime.utcnow() + timedelta(hours=2)
    }, app.config['SECRET_KEY'], algorithm='HS256')
    return jsonify({'token': token})

# 验证装饰器
def token_required(f):
    from functools import wraps
    @wraps(f)
    def decorated(*args, **kwargs):
        token = request.headers.get('Authorization', '').replace('Bearer ', '')
        if not token:
            return jsonify({'message': 'Token 缺失'}), 401
        try:
            data = jwt.decode(token, app.config['SECRET_KEY'], algorithms=['HS256'])
            current_user = data['user']
        except Exception:
            return jsonify({'message': 'Token 无效'}), 401
        return f(current_user, *args, **kwargs)
    return decorated

@app.route('/protected')
@token_required
def protected(current_user):
    return jsonify({'message': f'你好 {current_user}'})
```

---

## 部署与优化

### Q10: Flask 应用如何部署到生产环境？

Flask 内置的开发服务器不适用于生产环境。生产部署标准架构：

```
Nginx (反向代理 + 静态文件)
  ↓
Gunicorn / uWSGI (WSGI 服务器, 多 worker)
  ↓
Flask 应用
```

```bash
# Gunicorn 启动（4 个 worker 进程）
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

**Flask 3.0+ 的新变化**：Flask 3.0 开始支持 `async def` 视图函数，可以使用 ASGI 服务器（如 Uvicorn）部署以获得更好的异步并发能力。

### Q11: WSGI 和 ASGI 有什么区别？Flask 用哪个？

| | WSGI | ASGI |
|------|------|------|
| 全称 | Web Server Gateway Interface | Asynchronous Server Gateway Interface |
| 协议 | 同步，一次请求一个连接 | 异步，支持并发、WebSocket |
| 服务器 | Gunicorn、uWSGI | Uvicorn、Daphne、Hypercorn |
| Flask 支持 | 原生支持（Flask 是 WSGI 应用） | Flask 3.0+ 支持 async 视图（用 ASGI 服务器运行） |

```bash
# Flask 3.0+ 用 ASGI 运行
pip install uvicorn
uvicorn app:app --reload  # 注意：Flask app 需要包装为 ASGI
```

### Q12: Flask 有哪些性能优化手段？

1. **使用生产级 WSGI 服务器**（Gunicorn 替代 Flask dev server）
2. **多 worker / 多线程**（Gunicorn `-w 4 --threads 2`）
3. **静态文件走 Nginx**（不经过 Python 层）
4. **缓存数据库查询结果**（Redis / Flask-Caching）
5. **数据库查询优化**（避免 N+1，建立合适的索引）
6. **使用连接池**（SQLAlchemy `pool_size` + `max_overflow`）
7. **用异步库替换同步阻塞调用**（Flask 3.0+ async 视图）

---

来源：Flask 官方文档、博客园、腾讯云、面试鸭 mianshiya.com
