# subprocess 库

`subprocess` 是 Python 官方推荐的**强大、安全的进程管理和命令执行模块**。它能让你从 Python 内部启动新的操作系统进程（命令行程序），连接它们的标准输入/输出/错误管道，并获取返回码。

### 执行命令并获取输出

```python
import subprocess

# 最基础用法：运行命令并等待结束
result = subprocess.run(["ls", "-l"])

# 获取标准输出和错误（常用）
result = subprocess.run(
    ["ping", "-c", "3", "google.com"],
    capture_output=True,   # 捕获 stdout 和 stderr
    text=True              # 解码为字符串（而非 bytes）
)
print(f"返回码: {result.returncode}")  # 0 表示成功
print(f"输出: {result.stdout}")
print(f"错误: {result.stderr}")
```
