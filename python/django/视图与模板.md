# Django 视图与模板

>整理时间：2026-04-29 | 来源：知乎、CSDN、Stack Overflow、官方文档等

---

## 目录

1. [FBV vs CBV](#q1-fbv-和-cbv-的区别各有什么优缺点)
2. [通用类视图](#q2-django-常用的通用类视图有哪些)
3. [模板标签与过滤器](#q3-django-模板中常用标签和过滤器有哪些)
4. [自定义过滤器](#q4-django-模板中如何自定义过滤器)
5. [上下文处理器](#q5-django-的上下文处理器是什么)

---

### Q1: FBV 和 CBV 的区别？各有什么优缺点？

| | FBV（函数视图） | CBV（类视图） |
|---|---|---|
| 定义 | 函数 | 类 |
| 代码量 | 简单逻辑更少 | 复杂逻辑更少 |
| 复用 | 装饰器 | 继承/Mixin |
| 灵活性 | 高 | 中等 |
| 适用 | 简单页面 | CRUD 接口 |

```python
# FBV
def book_list(request):
    books = Book.objects.all()
    return render(request, 'book_list.html', {'books': books})

# CBV
class BookListView(ListView):
    model = Book
    template_name = 'book_list.html'
```

### Q2: Django 常用的通用类视图有哪些？

| 视图类 | 用途 |
|---|---|
| `TemplateView` | 渲染模板 |
| `ListView` | 对象列表 |
| `DetailView` | 单个对象详情 |
| `CreateView` | 创建对象 |
| `UpdateView` | 更新对象 |
| `DeleteView` | 删除对象 |
| `FormView` | 处理表单 |
| `RedirectView` | 重定向 |

### Q3: Django 模板中常用标签和过滤器有哪些？

**过滤器：**

```text
{{ value|length }}         # 长度
{{ value|date:"Y-m-d" }}   # 日期格式化
{{ value|default:"N/A" }}  # 默认值
{{ value|truncatechars:10 }} # 截断
{{ value|safe }}           # 不转义 HTML
```

**标签：**

```text
{% if %}...{% endif %}
{% for %}...{% endfor %}
{% block %}...{% endblock %}
{% extends "base.html" %}
{% include "header.html" %}
{% url 'view_name' arg1 %}
{% csrf_token %}
```

### Q4: Django 模板中如何自定义过滤器？

```python
# app/templatetags/my_filters.py
from django import template
register = template.Library()

@register.filter
def multiply(value, arg):
    return value * arg

# 模板中使用
{% load my_filters %}
{{ price|multiply:quantity }}
```

### Q5: Django 的上下文处理器是什么？

上下文处理器是一个函数，接收 `request` 参数，返回一个字典，该字典会合并到所有模板的上下文中。

```python
# settings.py
TEMPLATES = [
    {
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
            ],
        },
    },
]
```
