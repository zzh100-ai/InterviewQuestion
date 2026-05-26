# 一、NumPy 基础与核心概念

## 1. NumPy 是什么？它解决了什么问题？

**精简回答**：NumPy（Numerical Python）是 Python 科学计算的核心库，提供了高性能的多维数组对象 ndarray 和大量数学运算函数。当需要一次性处理十万个数字、百万次矩阵运算时，纯 Python 的表现会变得非常慢。

**为什么慢**：Python 列表允许存储不同类型的数据，这种灵活性付出了性能代价。

**NumPy 的解决方案**：放弃灵活性换取速度。ndarray 专门存放同类型数值，在内存中连续排列，一次性批量处理，比 Python 原生列表快几十倍甚至上百倍。

## 2. ndarray 和 Python 列表有什么区别？

| 对比项   | ndarray                     | Python 列表    |
| -------- | --------------------------- | -------------- |
| 数据类型 | 必须同质（全 int/float 等） | 可混合任意类型 |
| 内存布局 | 连续存储                    | 存储对象引用   |
| 运算方式 | 向量化运算（批量执行）      | 需要显式循环   |
| 性能     | 快几十到上百倍              | 慢             |
| 灵活性   | 较低                        | 极高           |

**面试小课堂**：`np.array([1,2,3]) * 2` 得到 `[2,4,6]`（逐元素乘），而 Python 列表 `[1,2,3]*2` 得到 `[1,2,3,1,2,3]`（重复拼接）。能答清楚这个区别，说明你懂了 NumPy 的本质。

## 3. ndarray 为什么这么快？三个关键原因

**精简回答**：

| 原因           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| **类型统一**   | 所有元素同一类型，计算机用固定步长遍历，无需像 Python 列表那样先弄清下一个元素是什么类型、多大、存在哪里 |
| **内存连续**   | 所有元素紧挨着存放在一段连续内存空间中，CPU 缓存命中率高     |
| **向量化运算** | 批量操作，底层 C 语言实现，避免 Python 循环开销              |

## 4. axis 的含义是什么？

**精简回答**：axis 指定 NumPy/Pandas 函数沿哪个方向操作。

| axis     | 含义     | 通俗理解                 |
| -------- | -------- | ------------------------ |
| `axis=0` | 跨行运算 | 沿列向下操作，压缩行维度 |
| `axis=1` | 跨列运算 | 沿行向右操作，压缩列维度 |

```python
import numpy as np
arr = np.array([[1, 2, 3],
                [4, 5, 6]])

print(np.sum(arr, axis=0))   # 沿列：每列求和 → [5, 7, 9]
print(np.sum(arr, axis=1))   # 沿行：每行求和 → [6, 15]
```

> **记忆技巧**：`axis=0` 是跨行操作，会消灭行（得到单行结果），指向"纵向"聚合；`axis=1` 是跨列操作，会消灭列（得到单列结果），指向"横向"聚合。

## 5. 什么是广播机制？

**精简回答**：广播是 NumPy 对不同形状数组执行算术运算的规则。较小的数组会在较大数组上“广播”，扩展成相同形状，实现向量化运算，让循环在 C 层而不是 Python 中发生。

**广播规则**：从尾部维度开始比较，维度大小必须相等或其中一个为 1，否则无法广播。

```python
# 标量广播
a = np.array([1, 2, 3])
b = 2
print(a * b)   # [2, 4, 6]  标量 2 广播到和 a 相同形状

# 行/列广播
col = np.arange(3).reshape(3, 1)   # 3x1 列向量
row = np.arange(3)                  # 1x3 行向量
print(col + row)                    # 3x3 矩阵，广播产生所有组合
```

## 6. 什么是通用函数？

**精简回答**：通用函数是 NumPy 中对 ndarray 进行逐元素运算的函数，分为一元 ufunc（对单个数组）和二元 ufunc（对两个数组）。

| 类型       | 示例                                                         |
| ---------- | ------------------------------------------------------------ |
| 一元 ufunc | `np.abs`、`np.sqrt`、`np.exp`、`np.sin`、`np.log`            |
| 二元 ufunc | `np.add`、`np.subtract`、`np.multiply`、`np.divide`、`np.power` |

```python
# 向量化运算，比 Python 循环快百倍以上
x = np.array([1, 4, 9, 16])
print(np.sqrt(x))   # [1., 2., 3., 4.]
```

## 7. reshape 和 resize 有什么区别？

| 对比   | reshape            | resize               |
| ------ | ------------------ | -------------------- |
| 返回值 | 返回新数组（视图） | 返回新数组或原地修改 |
| 原数组 | 不变               | 根据方法而定         |
| 内存   | 视图，共享数据     | 可能复制             |

```python
arr = np.arange(6)
# reshape：返回新视图，原数组不变
new = arr.reshape(2, 3)
# resize（方法）：原地修改形状，返回 None
arr.resize(2, 3)
# resize（函数）：返回新数组，原数组不变
new2 = np.resize(arr, (2, 3))
```

## 8. 视图与副本的区别是什么？

| 对比     | 视图（View）                   | 副本（Copy）                    |
| -------- | ------------------------------ | ------------------------------- |
| 数据关系 | 共享数据，修改视图会影响原数组 | 完全独立，互不影响              |
| 内存占用 | 小（仅元数据）                 | 大（复制全部数据）              |
| 触发方式 | 基础切片、reshape（多数情况）  | `copy()` 方法、花式索引（通常） |

```python
arr = np.arange(10)
view = arr[::2]        # 视图，共享数据
copy = arr[[0,2,4]]    # 副本（花式索引通常返回副本）
view[0] = 100          # arr[0] 也会变成 100
```

---

# 二、NumPy 数组操作与高级索引

## 9. NumPy 的索引方式有哪些？

**精简回答**：NumPy 提供多种索引机制，灵活度远高于 Python 列表。

| 索引类型                 | 说明                            | 示例              |
| ------------------------ | ------------------------------- | ----------------- |
| 基础索引                 | 整数/切片                       | `arr[1:5:2]`      |
| 整数数组索引（花式索引） | 整数列表/数组指定多个非连续位置 | `arr[[0,2,4]]`    |
| 布尔索引                 | 布尔数组筛选                    | `arr[arr > 5]`    |
| 混合索引                 | 切片 + 花式索引组合             | `arr2d[:, [0,2]]` |

```python
arr = np.array([10, 20, 30, 40, 50])

# 布尔索引：筛选 > 25 的元素
mask = arr > 25          # [False, False, True, True, True]
filtered = arr[mask]     # [30, 40, 50]

# 花式索引：用整数数组指定位置
indices = [0, 2, 4]
selected = arr[indices]  # [10, 30, 50]

# 花式索引重排行顺序
arr2d = np.array([[0,1,2], [3,4,5], [6,7,8]])
reordered = arr2d[[1,0,2]]  # 按 [1,0,2] 顺序重排行
```

## 10. 布尔索引和花式索引各有什么适用场景？

| 索引类型 | 适用场景                                         | 优点               |
| -------- | ------------------------------------------------ | ------------------ |
| 布尔索引 | 条件筛选、数据清洗、异常值过滤                   | 直观，直接表达条件 |
| 花式索引 | 提取非连续位置、重排元素顺序、按自定义顺序取数据 | 灵活，可任意重排   |

```python
# 布尔索引：筛选满足条件的行
data = np.random.randn(1000, 5)
filtered = data[data[:, 0] > 0]    # 筛选第一列 > 0 的所有行

# 花式索引：按自定义顺序提取列
col_order = [3, 0, 2, 4, 1]        # 自定义列顺序
reordered = data[:, col_order]
```

---

# 三、NumPy 随机数与线性代数

## 11. 如何生成随机数？为什么要设置随机种子？

```python
# 常用随机函数
np.random.rand(3, 4)        # 均匀分布 [0,1)
np.random.randn(3, 4)       # 标准正态分布
np.random.randint(1, 100, size=(3,4))  # 整数随机数组
np.random.normal(loc=0, scale=1, size=1000)  # 正态分布

# 设置随机种子——保证每次运行结果相同
np.random.seed(666)         # 常见面试考点
```

**随机种子的作用**：固定后，每次运行生成的随机数序列完全相同，保障实验可复现，在调参、对比算法效果时至关重要。

## 12. NumPy 线性代数模块有哪些常用函数？

```python
import numpy as np
from numpy import linalg as LA

A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# 矩阵乘法（推荐）
C = np.dot(A, B)           # 或 A @ B

# 矩阵求逆、行列式、特征值
A_inv = LA.inv(A)          # 逆矩阵
det = LA.det(A)            # 行列式
eig_vals, eig_vecs = LA.eig(A)  # 特征值和特征向量

# 解线性方程组：Ax = b
b = np.array([1, 2])
x = LA.solve(A, b)
```

**常用线性代数功能**：矩阵求逆、解线性方程组、特征值分解、奇异值分解等。

---

# 四、Pandas 基础与数据结构

## 13. Pandas 的基本数据结构有哪些？

**精简回答**：Pandas 提供两种主要数据结构，均建立在 NumPy 之上。

| 数据结构  | 维度 | 说明                   | 类比                      |
| --------- | ---- | ---------------------- | ------------------------- |
| Series    | 1 维 | 带标签的一维数组       | Python 字典或带索引的列表 |
| DataFrame | 2 维 | 带行索引和列标签的表格 | Excel 表格、SQL 表        |

```python
import pandas as pd

# Series
s = pd.Series([1, 2, 3], index=['a', 'b', 'c'])
print(s)   # a→1, b→2, c→3

# DataFrame
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'city': ['北京', '上海', '广州']
})
```

## 14. NumPy 和 Pandas 的区别是什么？

| 对比项       | NumPy               | Pandas                                |
| ------------ | ------------------- | ------------------------------------- |
| 核心数据结构 | ndarray（多维数组） | Series + DataFrame                    |
| 数据异构性   | 不支持（需同类型）  | 支持（不同列可不同类型）              |
| 索引方式     | 整数索引            | 标签索引 + 整数位置索引               |
| 缺失值处理   | 需手动处理（NaN）   | 内置 `isna()`、`fillna()`、`dropna()` |
| 数据类型     | 数值为主            | 数值 + 字符串 + 时间序列 + 分类数据   |
| 主要用途     | 数值计算、矩阵运算  | 数据分析、数据清洗、表格操作          |

**一句话总结**：NumPy 用于底层的数值计算，Pandas 用于更上层的结构化和表格化数据分析。

## 15. DataFrame 有哪些基本属性？

```python
df.shape          # 行数、列数
df.columns        # 列名列表
df.index          # 行索引
df.dtypes         # 各列数据类型
df.info()         # 整体信息（内存占用、非空值）
df.describe()     # 数值列的统计摘要（均值、标准差、四分位数）
```

---

# 五、Pandas 数据清洗与预处理

## 16. 如何处理缺失值？

| 方法       | 函数                  | 说明                       |
| ---------- | --------------------- | -------------------------- |
| 检测缺失值 | `isnull()` / `isna()` | 返回布尔型 DataFrame       |
| 删除缺失值 | `dropna()`            | 删除含缺失值的行或列       |
| 填充缺失值 | `fillna()`            | 用指定值/统计量/前后向填充 |

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'A': [1, np.nan, 3], 'B': [4, 5, np.nan], 'C': [7, 8, 9]})

# 检测缺失值
print(df.isnull())                # True 代表缺失

# 删除缺失值
df_clean = df.dropna()             # 删除含缺失值的行
df_clean_col = df.dropna(axis=1)   # 删除含缺失值的列

# 填充缺失值
df_filled = df.fillna(0)                       # 用 0 填充
df['A'] = df['A'].fillna(df['A'].mean())       # 用均值填充
df_ffill = df.fillna(method='ffill')           # 前向填充
```

## 17. 如何处理重复值？

| 方法       | 函数                | 说明                             |
| ---------- | ------------------- | -------------------------------- |
| 检测重复值 | `duplicated()`      | 返回布尔 Series，重复行返回 True |
| 删除重复值 | `drop_duplicates()` | 保留第一次出现的行（默认）       |

```python
df = pd.DataFrame({'A': [1, 2, 2, 3], 'B': [4, 5, 5, 6]})

print(df.duplicated())            # [False, False, True, False]

# 删除重复行
df_unique = df.drop_duplicates()               # 保留第一次出现的
df_unique_last = df.drop_duplicates(keep='last')  # 保留最后一次出现的
df_unique_all = df.drop_duplicates(keep=False)    # 删除所有重复行
```

## 18. 如何转换数据类型？

```python
df = pd.read_csv('data.csv')

# 查看数据类型
print(df.dtypes)

# 转换数据类型
df['age'] = df['age'].astype(float)
df['price'] = pd.to_numeric(df['price'], errors='coerce')  # 非数字转为 NaN

# 优化内存：将 object 类型转换为 category
df['category'] = df['category'].astype('category')
df['gender'] = df['gender'].astype('category')             # 适合低基数离散数据

# 日期解析
df['date'] = pd.to_datetime(df['date'])
```

## 19. 如何检测和处理异常值？

```python
# 使用 describe() 查看统计信息
print(df.describe())

# Z-score 方法检测异常值（3σ 原则）
from scipy import stats
z_scores = np.abs(stats.zscore(df['value']))
df_clean = df[(z_scores < 3)]

# IQR 方法（四分位距）
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1
df_clean = df[(df['value'] >= Q1 - 1.5*IQR) & (df['value'] <= Q3 + 1.5*IQR)]

# 条件替换异常值
df.loc[df['age'] > 120, 'age'] = df['age'].median()   # 用中位数替换
```

---

# 六、Pandas 数据筛选与查询

## 20. 如何选择 DataFrame 的数据？

| 方法         | 用法                               | 说明                         |
| ------------ | ---------------------------------- | ---------------------------- |
| `[]`         | `df['col']` / `df[df['A']>0]`      | 选择单列/布尔索引筛选行      |
| `loc`        | `df.loc['row_label', 'col_label']` | **标签索引**，包含终点       |
| `iloc`       | `df.iloc[0:3, 0:2]`                | **整数位置索引**，不包含终点 |
| `at` / `iat` | `df.at[row, col]` / `df.iat[0, 0]` | 快速访问单个元素             |

```python
# loc：通过标签名访问
df.loc[1:3, ['name', 'age']]        # 索引 1 到 3 行

# iloc：通过整数位置访问
df.iloc[0:3, 0:2]                   # 前 3 行前 2 列（不包含第 3 行）

# 推荐使用 .loc[] 避免链式赋值警告
df.loc[:, 'new_col'] = df['col1'] + df['col2']   # ✅ 正确做法
df['new_col'] = df['col1'] + df['col2']          # 也可以，但链式赋值时需注意
```

## 21. 如何按条件筛选数据？

```python
# 单条件
df_filtered = df[df['age'] > 30]

# 多条件（使用 & 和 |，注意括号）
df_filtered = df[(df['age'] > 30) & (df['city'] == '北京')]

# query() 方法：更接近 SQL 风格
df_filtered = df.query('age > 30 and city == "北京"')

# 使用 isin()：检查值是否在列表中
df_filtered = df[df['city'].isin(['北京', '上海', '广州'])]
```

**面试高频考点**：面试官最常考的 Pandas 操作是按条件筛选、分组聚合、关联合并三类，核心是还原 SQL 逻辑。

## 22. 如何排序数据？

```python
# 按单列排序
df_sorted = df.sort_values('age', ascending=False)   # 降序排列

# 按多列排序
df_sorted = df.sort_values(['department', 'salary'], ascending=[True, False])

# 按索引排序
df_sorted_index = df.sort_index()
```

---

# 七、Pandas 数据合并与连接

## 23. merge 和 join 和 concat 有什么区别？

| 函数          | 用途                        | 类比                 |
| ------------- | --------------------------- | -------------------- |
| `pd.merge()`  | 按列值合并（类似 SQL JOIN） | SQL 的 JOIN          |
| `df.join()`   | 按索引合并                  | DataFrame 的便捷方法 |
| `pd.concat()` | 单纯拼接（堆叠）            | SQL 的 UNION         |

## 24. 如何使用 merge？有哪些连接类型？

```python
# 准备数据
df1 = pd.DataFrame({'key': ['A', 'B', 'C'], 'value1': [1, 2, 3]})
df2 = pd.DataFrame({'key': ['B', 'C', 'D'], 'value2': [4, 5, 6]})

# 内连接（默认）：只保留两个 DataFrame 都有的 key
inner = pd.merge(df1, df2, on='key', how='inner')   # key: B, C

# 左连接：保留左表所有 key
left = pd.merge(df1, df2, on='key', how='left')     # key: A, B, C

# 右连接：保留右表所有 key
right = pd.merge(df1, df2, on='key', how='right')   # key: B, C, D

# 外连接：保留所有 key
outer = pd.merge(df1, df2, on='key', how='outer')   # key: A, B, C, D
```

**面试高频坑点**：
- merge 默认 `how='inner'`，但实际业务多是左连接，务必显式写 `how='left'`
- 当两表有同名列时，必须设置 `suffixes=('_left', '_right')`，否则 merge 后列名自动变成 `col_x` / `col_y`，后续代码可能崩溃
- merge 后出现重复行或丢失记录，通常是因为 `on` 和 `how` 参数理解不清

## 25. 如何使用 concat 拼接数据？

```python
# 纵向拼接（增加行）
df_concat = pd.concat([df1, df2], axis=0)

# 横向拼接（增加列）
df_concat = pd.concat([df1, df2], axis=1)

# 忽略原始索引，重新生成新索引
df_concat = pd.concat([df1, df2], axis=0, ignore_index=True)
```

---

# 八、Pandas 分组聚合与数据透视

## 26. groupby 分组聚合的核心操作是什么？

**精简回答**：groupby 遵循 **"split-apply-combine"** 模式：

```python
# 基础分组聚合
df = pd.DataFrame({
    'department': ['技术', '技术', '销售', '销售', '技术'],
    'salary': [10000, 12000, 8000, 9000, 13000]
})

# 单列聚合
df.groupby('department')['salary'].mean()
# 技术：11666.67，销售：8500

# 多聚合函数：使用 agg()
df.groupby('department')['salary'].agg(['mean', 'median', 'std', 'count'])
```

## 27. groupby 后的 agg、transform、apply 和 filter 有什么区别？

| 方法          | 输出形状       | 典型场景                   |
| ------------- | -------------- | -------------------------- |
| `agg()`       | **1 行/组**    | 计算统计量，每组输出汇总行 |
| `transform()` | **同原始行数** | 每组计算结果广播回原行     |
| `apply()`     | 灵活           | 自定义复杂操作             |
| `filter()`    | 按条件筛选组   | 过滤掉不符合条件的整个分组 |

```python
# agg：聚合，每组输出一行
summary = df.groupby('customer_id').agg(
    order_count=('order_id', 'count'),
    total_revenue=('revenue', 'sum')
).reset_index()

# transform：广播回原行，形状不变
df['customer_revenue'] = df.groupby('customer_id')['revenue'].transform('sum')
# 每一行都获得该客户的订单总额

# filter：筛选分组（相当于 SQL 的 HAVING）
# 筛选订单数 > 5 的客户组
filtered = df.groupby('customer_id').filter(lambda g: g['order_id'].count() > 5)
```

## 28. 透视表是什么？

```python
# pivot_table：创建交叉表格
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'date': ['2024-01', '2024-01', '2024-02', '2024-02'],
    'product': ['A', 'B', 'A', 'B'],
    'sales': [100, 150, 120, 180]
})

# 按 date 和 product 透视，聚合 sales 的均值
pivot = df.pivot_table(
    values='sales',
    index='date',      # 行
    columns='product', # 列
    aggfunc=np.mean    # 聚合方式
)

# melt：宽表转长表（逆向操作）
melted = pd.melt(df, id_vars=['date'], value_vars=['A', 'B'], var_name='product', value_name='sales')
```

---

# 九、Pandas 时间序列处理

## 29. 如何处理时间序列数据？

```python
# 转换字符串日期为 datetime 类型
df['date'] = pd.to_datetime(df['date'])
df['date'] = pd.to_datetime(df['date'], format='%Y-%m-%d')

# 设置时间列为索引
df.set_index('date', inplace=True)

# 按日期范围筛选
df_2024 = df['2024-01-01':'2024-12-31']

# 重采样：改变时间频率
df_monthly = df.resample('M').sum()           # 按月汇总
df_weekly = df.resample('W').mean()           # 按周汇总
df_daily = df.resample('D').ffill()           # 填充缺失日期

# 移动窗口计算
df['rolling_mean'] = df['value'].rolling(window=7).mean()   # 7 天滚动平均
```

---

# 十、性能优化与最佳实践

## 30. 如何写出高性能的 Pandas 代码？

**核心建议**：尽量避免使用显式循环。Pandas 和 NumPy 的优势在于向量化操作，利用底层的 C 实现批量处理。

| 优化方法               | 说明                              | 提效倍数   |
| ---------------------- | --------------------------------- | ---------- |
| **向量化运算**         | 使用 NumPy ufunc 替代 Python 循环 | 百倍以上   |
| **query() / eval()**   | 字符串表达式求值，底层向量化      | 数倍       |
| **优化数据类型**       | float64→float32，object→category  | 内存省 70% |
| **chunksize 分块读取** | 处理超过内存的大文件              | 避免 OOM   |

```python
# ❌ 避免
result = []
for i in range(len(df)):
    if df.loc[i, 'value'] > 0:
        result.append(df.loc[i, 'value'] * 2)

# ✅ 推荐：向量化
result = df.loc[df['value'] > 0, 'value'] * 2

# ❌ 避免逐行 apply 复杂函数
def calc(row):
    return row['a'] + row['b'] * 2
df['result'] = df.apply(calc, axis=1)

# ✅ 推荐：直接向量化
df['result'] = df['a'] + df['b'] * 2
```

## 31. 如何优化 DataFrame 的内存占用？

```python
# 查看内存占用
print(df.info(memory_usage='deep'))

# 优化方法：
# 1. 整数：int64 → int32 或 int16
df['col'] = df['col'].astype('int32')

# 2. 浮点数：float64 → float32
df['col'] = df['col'].astype('float32')

# 3. 字符串转 category（低基数）
df['gender'] = df['gender'].astype('category')

# 4. 删除不必要的列
df = df.drop(['unused_col'], axis=1)
```

---

# 十一、实战场景题

## 32. 手写代码：计算每个客户的订单数、总金额、首末订单日期

```python
customer_summary = (df
    .groupby('customer_id')
    .agg(
        order_count=('order_id', 'count'),
        total_amount=('amount', 'sum'),
        first_order=('order_date', 'min'),
        last_order=('order_date', 'max')
    )
    .reset_index()
)
```

## 33. 手写代码：删除某列中包含缺失值的行

```python
# 删除 age 列有缺失值的行
df_clean = df.dropna(subset=['age'])

# 删除全部列都有缺失值的行
df_clean = df.dropna(how='all')
```

## 34. 手写代码：将字符串列转换为分类类型

```python
# 面试官想看到 category 类型，而非简单的 rename
df['category'] = df['category'].astype('category')
df['category'] = df['category'].cat.set_categories(['低', '中', '高'], ordered=True)

print(df['category'].dtype)      # category
print(df['category'].cat.codes)  # 查看底层整数编码
```

## 35. 手写代码：合并多个 CSV 文件并去重

```python
import glob
import pandas as pd

path = './data/*.csv'
all_files = glob.glob(path)

# 读取并合并
df_list = (pd.read_csv(f) for f in all_files)
df_merged = pd.concat(df_list, ignore_index=True)

# 去重
df_unique = df_merged.drop_duplicates(subset=['id'], keep='first')
```

## 36. 手写代码：对数据进行标准化和归一化

```python
import numpy as np
import pandas as pd

# 标准化：(x - μ) / σ
def standardize(series):
    return (series - series.mean()) / series.std()

# 归一化：(x - min) / (max - min)
def normalize(series):
    return (series - series.min()) / (series.max() - series.min())

# 应用
df['col_standardized'] = standardize(df['col'])
df['col_normalized'] = normalize(df['col'])
```

---

