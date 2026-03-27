# 正确显示中文
### 方法一
```python
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = 'SimHei'
plt.rcParams['axes.unicode_minus'] = False   # 解决坐标轴负数的负号显示问题
```
### 方法二
```python
import matplotlib.pyplot as plt

font = {'family' : 'SimHei',
        'weight' : 'bold',
        'size'   : '16'}
plt.rc('font', **font)               
plt.rc('axes', unicode_minus=False)  
```
