# Django 进阶与源码

>整理时间：2026-04-29 | 来源：知乎、CSDN、Stack Overflow、官方文档等

---

## 目录

1. [信号的同步性](#q1-django-信号是同步的还是异步的)
2. [Manager](#q2-django-的-manager-是什么)
3. [migrate vs makemigrations](#q3-migrate-和-makemigrations-的区别)
4. [数据库连接](#q4-django-如何处理数据库连接)
5. [Admin 自定义](#q5-django-admin-如何自定义)
6. [JWT Token](#q6-django-如何生成和校验-jwt-token)
7. [QuerySet 惰性](#q7-django-中-queryset-是惰性的吗)
8. [URL 命名空间](#q8-django-url-路由命名空间有什么用)

---

### Q1: Django 信号是同步的还是异步的？

默认是**同步**的。信号接收器会在发送信号的同一线程中执行，会阻塞当前请求。如需异步，需要结合 Celery 等任务队列。

### Q2: Django 的 `Manager` 是什么？

Manager 是 Django 模型与数据库交互的接口。每个模型至少有一个默认 Manager（`objects`）。可自定义：

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(status='published')

class Book(models.Model):
    objects = models.Manager()  # 默认
    published = PublishedManager()  # 自定义
```

### Q3: `migrate` 和 `makemigrations` 的区别？

| 命令 | 作用 |
|---|---|
| `makemigrations` | 基于 Model 变更**生成**迁移文件 |
| `migrate` | 将迁移文件**应用**到数据库 |

### Q4: Django 如何处理数据库连接？

Django 默认使用持久连接（`CONN_MAX_AGE` 控制）。请求开始时从连接池获取连接，结束时归还。`CONN_MAX_AGE=0` 表示每次请求新建连接。

### Q5: Django Admin 如何自定义？

```python
@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ['title', 'author', 'created_at']  # 列表页显示字段
    list_filter = ['status', 'created_at']  # 过滤
    search_fields = ['title', 'author__name']  # 搜索
    ordering = ['-created_at']
    actions = ['make_published']  # 批量操作

    def make_published(self, request, queryset):
        queryset.update(status='published')
```

### Q6: Django 如何生成和校验 JWT Token？

通常使用 `djangorestframework-simplejwt`：

```python
from rest_framework_simplejwt.tokens import RefreshToken

# 生成
def get_tokens_for_user(user):
    refresh = RefreshToken.for_user(user)
    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }

# 视图
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView
```

### Q7: Django 中 `queryset` 是惰性的吗？

是的。QuerySet 是惰性求值的，在以下情况才会真正执行 SQL：

* 迭代（`for obj in queryset`）
* 切片（`queryset[:10]`）
* `len()` / `list()` / `bool()`
* `repr()` 在交互式中

### Q8: Django URL 路由命名空间有什么用？

防止不同 app 的 URL 名称冲突，并支持重用：

```python
# project/urls.py
path('blog/', include(('blog.urls', 'blog'), namespace='blog'))

# 模板中
{% url 'blog:post_detail' post.id %}
# 视图中
reverse('blog:post_detail', args=[post.id])
```
