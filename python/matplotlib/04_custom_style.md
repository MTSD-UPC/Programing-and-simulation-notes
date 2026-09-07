# 自定义样式

### rc 设置

```python
import matplotlib.pyplot as plt
import numpy as np

plt.rcParams['lines.linewidth'] = 2
plt.rcParams['lines.linestyle'] = '--'
data = np.random.randn(50)
plt.plot(data)
```

### 样式表

创建样式表：`custom.mplstyle`

```
axes.titlesize : 24
axes.labelsize : 20
lines.linewidth : 3
lines.markersize : 10
xtick.labelsize : 16
ytick.labelsize : 16
```

使用样式表

```python
import matplotlib.pyplot as plt
plt.style.use('./custom.mplstyle')
```