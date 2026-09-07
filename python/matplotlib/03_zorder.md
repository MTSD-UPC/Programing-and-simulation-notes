# 图层调整

### 初始化

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 9, 10)
y = np.random.randint(0, 100, 10)
```
### 默认线图在上

```python
plt.plot(x, y, color='C0')
plt.scatter(x, y, color='C1')
plt.show()
```
### 设置 zorder 使散点图在上

```python
plt.plot(x, y, color='C0', zorder=1)
plt.scatter(x, y, color='C1', zorder=2)
plt.show()
```