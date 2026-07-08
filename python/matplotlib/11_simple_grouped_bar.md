# 简单的分组柱状图

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker

# 自定义绘图函数
def myplot(df, filename, ylabel, legend):
    colors = ['#FF9999', '#66B3FF', '#99FF99', '#FFCC99']
    # 绘制分组柱状图
    ax = df.plot(kind='bar', figsize=(12, 6), width=0.8, color=colors, linewidth=0)

    # 设置刻度字体大小和标签字体大小
    ax.tick_params(labelsize=22, width=3, length=8)  # 刻度字体大小
    plt.xlabel('', fontsize=22)  # X轴标签字体大小
    plt.ylabel(ylabel, fontsize=18)  # Y轴标签字体大小
    plt.ylim([0, 100])

    # 设置 spines 的宽度
    for spine in ax.spines.values():
        spine.set_linewidth(3)
    ax.yaxis.set_minor_locator(ticker.AutoMinorLocator())  # 自动添加次刻度
    ax.tick_params(axis='y', which='minor', length=4, width=3, color='gray')  # 设置次刻度的样式

    # 其他设置
    plt.xticks(rotation=0)  
    ax.legend(legend, frameon=False, fontsize=18, loc="upper left", bbox_to_anchor=(1, 1))
    
    plt.tight_layout()
    plt.show()
    # plt.savefig(filename, dpi=600)

# 主程序
data = [
    [56.18, 24.5, 10.74, 5.28],
    [59.77, 40.13, 12.27, 10.17],
    [75.59, 51.22, 19.43, 18.15],
    [79.29, 62.38, 30.64, 23.54],
    [85.12, 84.03, 45.31, 35.32]
]
filename = "grouped_bar.png"
index = [f"Method {i}" for i in range(1, len(data) + 1)]
ylabel = 'Proportion(%)'
legend = [f"Type {i}" for i in range(1, len(data) + 1)]

df = pd.DataFrame(data)
df.index = index
myplot(df, filename, ylabel, legend)
```

