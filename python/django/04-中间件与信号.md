# Django 中间件与信号

>整理时间：2026-04-29 | 来源：知乎、CSDN、Stack Overflow、官方文档等

---

## 目录

1. [自定义中间件](#q1-如何自定义-django-中间件)
2. [信号概述](#q2-django-信号是什么常用信号有哪些)
3. [信号使用方式](#q3-django-信号的使用方式)
4. [中间件 vs 信号](#q4-django-中间件和信号的使用场景对比)

---

### Q1: 如何自定义 Django 中间件？

```python
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # 请求前处理
        print('请求进入')
        response = self.get_response(request)
        # 响应后处理
        print('响应返回')
        return response
```

### Q2: Django 信号是什么？常用信号有哪些？

信号是一种观察者模式的实现，允许解耦的应用在特定事件发生时得到通知。

常用信号：

* `pre_save` / `post_save` — 模型保存前/后
* `pre_delete` / `post_delete` — 模型删除前/后
* `m2m_changed` — 多对多关系变化
* `request_started` / `request_finished` — 请求开始/结束
* `user_logged_in` / `user_logged_out` — 用户登录/登出

### Q3: Django 信号的使用方式？

```python
from django.db.models.signals import post_save
from django.dispatch import receiver

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance)

# 或者手动注册
post_save.connect(create_user_profile, sender=User)
```

### Q4: Django 中间件和信号的使用场景对比？

| | 中间件 | 信号 |
|---|---|---|
| 粒度 | 请求/响应级别 | 模型/应用级别 |
| 适用 | 认证、日志、CORS | 缓存失效、搜索索引更新 |
| 耦合度 | 较低 | 极低 |
| 执行方式 | 同步链式 | 同步（默认） |
