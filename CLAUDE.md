# CLAUDE.md

本文件为 Claude Code 在此仓库中工作时提供指导。

## 仓库用途

个人面试准备目录，用于存放结构化的 Markdown 面试题文件。目前专注于 Python 后端开发主题。

## 文件组织

* 面试笔记按语言和框架分层存放：
  * `python/` — Python 生态，按主题分文件夹管理：
    * `核心语法/` — Python 语言基础、面向对象编程、面试鸭题库
    * `并发/` — GIL、多线程/多进程、asyncio、I/O 多路复用
    * `网络/` — TCP/IP、HTTP/HTTPS、网络安全、系统设计
    * `数据库/` — 事务、索引、MySQL 引擎、Redis
    * `算法与数据结构/` — 链表、二叉树、排序、动态规划
    * `标准库/` — Python 标准库模块面试题
    * `性能优化/` — Python 性能分析与优化
    * `Flask/` — Flask 框架面试题（上下文、蓝图、扩展生态）
    * `FastAPI/` — FastAPI 异步框架面试题（依赖注入、Pydantic、JWT）
    * `django/` — Django 框架面试题
      * 每个知识模块独立一个 `.md` 文件（如 `01-Django 基础.md`、`02-orm.md`）
      * `assets/` — 静态资源（图片等）
  * 每个文件夹下文件从 01 开始独立编号

## 编写规范

* 内容使用中文
* 每个问答条目：`### QN: 问题` → 答案，必要时附带代码片段
* 按主题领域分类（基础、ORM、进阶等）
* 来源：汇总自知乎、CSDN、Stack Overflow、面试鸭 mianshiya.com 及官方文档
