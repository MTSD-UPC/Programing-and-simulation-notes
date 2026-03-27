# 绘图设置

### 导入 matplotlib
```python
import matplotlib.pyplot as plt
```

### 正确显示中文
**方法一**
```python
plt.rcParams['font.sans-serif'] = 'SimHei'
plt.rcParams['axes.unicode_minus'] = False   # 解决坐标轴负数的负号显示问题
```

**方法二**
```python
font = {'family' : 'SimHei',
        'weight' : 'bold',
        'size'   : '16'}
plt.rc('font', **font)               
plt.rc('axes', unicode_minus=False)  
```

### 刻度线方向朝内

```python
plt.rcParams['xtick.direction'] = 'in'
plt.rcParams['ytick.direction'] = 'in'
```
