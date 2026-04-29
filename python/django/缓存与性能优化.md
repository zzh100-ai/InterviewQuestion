# Django 缓存与性能优化

>整理时间：2026-04-29 | 来源：知乎、CSDN、Stack Overflow、官方文档等

---

## 目录

1. [缓存后端](#q1-django-支持哪些缓存后端)
2. [缓存使用方式](#q2-django-缓存的使用方式)
3. [查询优化手段](#q3-django-数据库查询优化有哪些手段)
4. [读写分离](#q4-django-中如何做数据库读写分离)

---

### Q1: Django 支持哪些缓存后端？

| 后端 | 说明 |
|---|---|
| `MemcachedCache` | 基于内存，高性能 |
| `RedisCache` | 功能丰富，支持持久化 |
| `DatabaseCache` | 存数据库 |
| `FileBasedCache` | 存文件系统 |
| `LocMemCache` | 进程内内存（默认，开发用） |
| `DummyCache` | 不缓存（测试用） |

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
    }
}
```

### Q2: Django 缓存的使用方式？

```python
from django.core.cache import cache

# 设置缓存
cache.set('key', 'value', timeout=60)
# 获取（无则返回 None）
cache.get('key')
# 获取或设置
cache.get_or_set('key', 'default', timeout=60)
# 删除
cache.delete('key')

# 视图缓存装饰器
from django.views.decorators.cache import cache_page
@cache_page(60 * 15)  # 缓存 15 分钟
def my_view(request):
    ...

# 模板片段缓存
{% load cache %}
{% cache 500 sidebar request.user.username %}
    <!-- 缓存 500 秒 -->
{% endcache %}
```

### Q3: Django 数据库查询优化有哪些手段？

1. 使用 `select_related` / `prefetch_related`
2. 使用 `only()` / `defer()` 只加载需要的字段
3. 使用 `values()` / `values_list()` 返回字典而非 Model 实例
4. 使用 `bulk_create` / `bulk_update` 批量操作
5. 使用 `exists()` 代替 `count()` 检查记录是否存在
6. 添加数据库索引
7. 使用数据库连接池（django-db-connection-pool）

### Q4: Django 中如何做数据库读写分离？

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'write_db',
        'HOST': '主库地址',
    },
    'replica': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'read_db',
        'HOST': '从库地址',
    },
}

DATABASE_ROUTERS = ['path.to.ReadWriteRouter']

class ReadWriteRouter:
    def db_for_read(self, model, **hints):
        return 'replica'

    def db_for_write(self, model, **hints):
        return 'default'
```
