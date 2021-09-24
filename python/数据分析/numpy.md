# 概述

数据分析绝对绕不过的三个包是numpy、scipy和pandas。

+ numpy是Python的数值计算扩展，专门用来处理矩阵，它的运算效率比列表更高效。

+ scipy是基于numpy的科学计算包，包括统计、线性代数等工具。
+ pandas是基于numpy的数据分析工具，能更方便的操作大型数据集。

---2017-06-29

# 基础知识：

### List

List is a non-homogeneous data structure which stores the elements in single row and multiple rows and columns.

List can be represented by [ ]，which represents you can create an empty list like this `data = []`

List allows duplicate elements.

```python
data = [[1, 2, 3], [4, 5, 6]]
print(data[0])
print(data[1])
print(data[0][2])
# [1, 2, 3]
# [4, 5, 6]
# 3
```

### Tuple

Tuple is also a non-homogeneous data structure which stores single row and multiple rows and columns.

Tuple can be represented by  ( ) `data = ()`

Tuple allows duplicate elements.

```python
data = ((1, 2, 3), (4, 5, 6))
print(data[0])
print(data[1])
print(data[0][2])
# (1, 2, 3)
# (4, 5, 6)
# 3
```

# 学习numpy

numpy的数据结构是n维的数组对象，叫做ndarray。Python的list虽然也能表示，但是不高效 **Why?**，随着列表数据的增加，效率会降低。

## ndarray

### 创建ndarray数组

+ 方法一 通过list和tuple创建

```python
import numpy as np

data = [[1, 2, 3], [4, 5, 6]]
# data = ((1, 2, 3), (4, 5, 6))
# 嵌套列表会被转换为一个多维数组，它也可以被称为矩阵
array = np.array(data)
# array = np.array(((1, 2, 3), (4, 5, 6)))
# array = np.array([[1, 2, 3], [4, 5, 6]])
print(array[0])
print(array[1])
print(array[0][2])
# [1 2 3]
# [4 5 6]
# 3
```
一些输出区别：

```python
import numpy as np

data = [[1, 2, 3], [4, 5, 6]]
array = np.array([[1, 2, 3], [4, 5, 6]])
print(array)
print(data)
# [[1 2 3]
#  [4 5 6]]
# [[1, 2, 3], [4, 5, 6]]
```

numpy一维数组用print输出的时候为 [1 2 3 4]，没有“,”这跟python的列表是有些差异的

+ 方法二  通过np.arange创建

```python
import numpy as np

arr = np.arange(5)
print(arr)
arr = np.array([np.arange(3), np.arange(3)])
print(arr)
# [0 1 2 3 4]
# [[0 1 2]
#  [0 1 2]]
```

- 方法三 基于arange以及reshape创建多维数组

```python
import numpy as np

arr = np.arange(24).reshape(2, 3, 4)
print(arr)
# [[[ 0  1  2  3]
#   [ 4  5  6  7]
#   [ 8  9 10 11]]

# [[12 13 14 15]
#  [16 17 18 19]
#  [20 21 22 23]]]
```

请注意：

- arange的长度与ndarray的维度的乘积要相等，即 24 = 2X3X4

- 几个参数就是几维数组

### ndarray数组的属性

- **dtype属性**

Numpy数组通常是由相同种类的元素组成的, 这样有一个好处---数组元素的类型相同，能快速确定存储数据所需空间的大小，可以用dtype查询其类型：

```python
array = np.array(data)
print(array.dtype)
# int64
```

优势之一哦

- **ndim属性**，数组维度的数量

```python
import numpy as np

array = np.array([[1, 2, 3], [7, 8, 9]])
print(array.ndim)
# 2
```

- **shape属性**，数组对象的尺度，对于矩阵，即n行m列,shape是一个元组（tuple）

```python
print(array.shape)
# (2, 3)
```

- **size属性**用来保存元素的数量，相当于shape中nXm的值

```python
print(array.size)
# 6
```

- **itemsize**属性返回数组中各个元素所占用的字节数大小

```
print(array.itemsize)
# 8
```

- **nbytes属性**，如果想知道整个数组所需的字节数量，可以使用nbytes属性。其值等于数组的size属性值乘以itemsize属性值。

```
print(array.nbytes)
# 48
```

- **T属性**，数组转置

### 运算优势

```python
import numpy as np

data = [[1, 2, 3]]
array = np.array(data)
print(array + 2)
print(array * array)
# [[3 4 5]]
# [[1 4 9]]
```

关于**numpy的向量化运算**和**python的循环遍历运算**效率对比，参考下面的例子可以看出差异：
+ 使用python的list进行循环遍历运算

```python
import time


def pySum():
    a = list(range(10000))
    b = list(range(10000))
    c = []
    for i in range(len(a)):
        c.append(a[i]**2 + b[i]**2)
    return c

best_time = 0.0
for i in range(10):
    start_time = time.time()
    pySum()
    end_time = time.time()
    if best_time == 0:
        best_time = end_time - start_time
        continue

    if best_time > (end_time - start_time):
        best_time = end_time - start_time

print("The best time in the 10 loop was %.3fms" % (best_time*1000))
# The best time in the 10 loop was 4.162ms
```
+ 使用numpy进行向量化运算

```python
def npSum():
    a = np.arange(10000)
    b = np.arange(10000)
    c = a**2 + b**2
    return c

# The best time in the 10 loop was 0.063ms
```

从上可以看出效率提高了两个数量级，**numpy的向量化运算**的效率要远远高于**python的循环遍历运算**。

### ndarray数组的切片和索引

- **一维数组**

一维数组的切片和索引与python的list索引类似

```python
import numpy as np

array = np.arange(6)
print(array[:4])
print(array[1:4])
print(array[:6:2])
print(array[1:6:2])
# [0 1 2 3]
# [1 2 3]
# [0 2 4]
# [1 3 5]
```

- **二维数组的切片和索引**

```
import numpy as np

array = np.arange(12).reshape(3, 4)
print(array)
print(array[:2, 1:4])
print(array[:1, 1:4])
print(array[:1, 2:4])
print(array[:3, 2:4])
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

#[[1 2 3]
# [5 6 7]]

# [[1 2 3]]

# [[2 3]]

# [[ 2  3]
#  [ 6  7]
#  [10 11]]
```

### numpy常用统计函数

常用的函数如下：

**请注意函数在使用时需要指定axis轴的方向**，若不指定，默认统计整个数组。

- np.sum()，返回求和
- np.mean()，返回均值
- np.max()，返回最大值
- np.min()，返回最小值
- np.ptp()，数组沿指定轴返回最大值减去最小值，即（max-min）
- np.std()，返回标准偏差（standard deviation）
- np.var()，返回方差（variance）
- np.cumsum()，返回累加值
- np.cumprod()，返回累乘积值

```python
import numpy as np

array = np.array([[0, 1, 20, 3, 4, 5],
                  [6, 7, 8, 9, 10, 11]])

print(np.max(array))
# 沿axis=1轴方向统计
print(np.max(array, axis=1))
# 沿axis=0轴方向统计
np.max(array, axis=0)

array = np.array([6, 7, 20, 9, 10, 11])
print(np.min(array))

# 20
# [20 11]
# 6
```

