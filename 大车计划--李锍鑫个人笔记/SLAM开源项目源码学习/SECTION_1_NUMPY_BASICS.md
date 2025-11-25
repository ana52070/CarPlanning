# 第一阶段：NumPy 基础速通 (针对本项目)

在阅读 `astar.py` 和 `map_pub.py` 之前，理解以下 NumPy 概念至关重要。本项目大量使用了 NumPy 数组操作来提高地图处理的速度，避免使用缓慢的 Python `for` 循环。

## 1. 数组形状 (Shape) 与 重塑 (Reshape)

**代码出处**: `astar.py` 第 38 行
```python
data = np.array(data).reshape(height, width)
```

**解释**:
ROS 的 `OccupancyGrid` 地图数据实际上是一维数组（一条长长的列表），但我们在处理时需要把它看作二维网格（行和列）。
*   **输入**: `[0, 0, 100, 0, 0, 100]` (假设这是一行数据)
*   **操作**: `reshape(2, 3)` (变身为 2 行 3 列)
*   **输出**:
    ```
    [[0, 0, 100],
     [0, 0, 100]]
    ```
这样我们就可以通过 `[row][col]` 的坐标方式来访问地图点了。

## 2. 布尔掩码 (Boolean Masking) - 核心技巧

**代码出处**: `astar.py` 第 41 行
```python
wall_mask = data == 100
```

**解释**:
这是 NumPy 最强大的功能之一。你可以直接对整个数组提问，它会返回一个同样形状的 `True/False` 数组。
*   **场景**: 找出地图里所有的墙（值为 100 的点）。
*   **示例**:
    ```python
    import numpy as np
    data = np.array([0, 100, 5, 100])
    wall_mask = (data == 100)
    # 结果: [False, True, False, True]
    ```
*   **应用**: 我们可以用这个 mask 直接修改数据，例如：`data[wall_mask] = 99` (把所有墙的值改成 99)。

## 3. 数组滚动 (Roll) 与 障碍物膨胀

**代码出处**: `astar.py` 第 46-47 行 (最难懂的一段)
```python
shifted_mask = np.roll(wall_mask, (i, j), axis=(0, 1))
data[shifted_mask] = 100
```

**解释**:
这段代码在做**障碍物膨胀**。为了让机器人不贴着墙走，我们需要把墙“变厚”。
*   **原理**:
    1.  找到所有墙的位置 (`wall_mask`)。
    2.  把这张“墙的掩码图”向右平移一格 (`np.roll`)。
    3.  把平移后覆盖到的地方也标记为墙。
    4.  重复上下左右各个方向平移，墙就“胖”了一圈。
*   **比喻**: 就像拿一张印章盖章，你手抖了一下，印章向右滑了一点，原来的位置有墨水，向右滑的位置也有了墨水。

## 4. 广播机制 (Broadcasting)

**代码出处**: `start_nav.py` 第 106 行
```python
interpolated_segment = start_point + (end_point - start_point)[np.newaxis, :] * t_values[:, np.newaxis]
```

**解释**:
允许不同形状的数组进行数学运算。
*   简单例子: `np.array([1, 2, 3]) * 2` 结果是 `[2, 4, 6]`。数组直接乘数字，会对每个元素都乘。
*   在代码中，这用于**插值**：计算从起点到终点中间的一系列点，通过矩阵乘法一次性算出所有坐标，而不是写个循环一个个算。

## 5. 欧几里得距离 (Euclidean Distance)

**代码出处**: `start_nav.py` 第 47 行
```python
distance_to_target = math.sqrt(dx**2 + dy**2)
```
或者 `np.linalg.norm`。
**解释**:
勾股定理 $c = \sqrt{a^2 + b^2}$。在导航中，这是计算机器人离目标点有多远的唯一真理。

---

**小结**:
当你在代码中看到 `data[...] = ...` 或者 `array * number` 时，记住这是在同时操作成百上千个数据。这正是 Python 能跑实时 SLAM 的关键——把繁重的计算交给底层的 C 语言优化过的 NumPy 库。