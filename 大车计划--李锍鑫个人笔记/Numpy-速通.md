
numpy.array()
可以用它表示普通的一维数组，或者二维矩阵，甚至任意维数据
![](assets/Numpy-速通/file-20251125095350759.png)

```python
import numpy as np

#创建一个numpy数组对象
np.array([1,2,3,4,5])

#也可以用zeros创建全0数组
np.zeros((3,2)) #3行2列

#获取数组尺寸
np.shape


#创建一个全部是一的数组
np.ones((2,4))


#创建一个递增或递减的数列
np.arange(3,7) #左开右闭 3,4,5,6

#返回某个区间等间距分布的数 
np.linspace(0,1,5) # 前俩参数是区间范围，后面参数是输出样本总数
>>>array([0 , 0.25 , 0.5 , 0.75 , 1])

#生成一个随机的数组
np.random.rand(2,4) #2行4列


#在Numpy中，数组默认的数据类型是64位浮点数
#但在创建数组时，可以通过dtype指定其他数据类型
a = np.zeros((4,2) , dtype = np.int32)
"""
整形        np.int8/16/32/64
无符号整型   np.uint8/16/32/64
浮点数      np.float32/64
布尔值      bool
字符串      str
"""
#对于现有的数组可以通过astype来转换数据类型
a = np.zeros((2,4))
b = a.astype(int)


```


基本运算
```python

#两个相同尺寸的数组可以直接进行四则运算,它会将数组同位置的元素进行加减乘除
a = np.array([1,2,3])
b = np.array([4,5,6])
a + b


a / b

#在乘法运算中还有一个dot，它会将两个向量进行点乘运算
np.dot(a,b)
>>>


#另一个与乘法相关的是@符号,它会进行矩阵的乘法运算
a @ b
>>>



```