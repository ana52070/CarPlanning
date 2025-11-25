
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
np.arange()

```