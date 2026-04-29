# Django 安全

>整理时间：2026-04-29 | 来源：知乎、CSDN、Stack Overflow、官方文档等

---

## 目录

1. [内置安全保护](#q1-django-默认提供哪些安全保护)
2. [安全加固建议](#q2-django-项目的安全加固建议有哪些)
3. [文件上传安全](#q3-文件上传安全注意事项)

---

### Q1: Django 默认提供哪些安全保护？

| 保护机制 | 说明 |
|---|---|
| CSRF 保护 | 跨站请求伪造防护，表单中需 `{% csrf_token %}` |
| XSS 防护 | 模板自动转义 HTML，`safe` 过滤器显式标记 |
| SQL 注入防护 | ORM 使用参数化查询 |
| 点击劫持防护 | `X-Frame-Options: DENY` |
| 安全的 Session | Session 数据存服务端，Cookie 只存 sessionid |
| 密码哈希 | 默认使用 PBKDF2 + SHA256 |

### Q2: Django 项目的安全加固建议有哪些？

```python
# 生产环境 settings.py
DEBUG = False
ALLOWED_HOSTS = ['your-domain.com']
SECRET_KEY = os.environ['DJANGO_SECRET_KEY']  # 环境变量

# HTTPS 强制
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True

# HSTS
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
```

### Q3: 文件上传安全注意事项？

* 验证文件类型（白名单，不依赖扩展名）
* 限制文件大小
* 使用 `django-storages` 存到云存储（非本地）
* 不在用户可访问的目录执行上传文件

```python
def validate_file_extension(value):
    ext = os.path.splitext(value.name)[1]
    if ext.lower() not in ['.jpg', '.png', '.pdf']:
        raise ValidationError('不支持的文件类型')
```
