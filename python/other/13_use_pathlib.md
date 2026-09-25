# pathlib 库

`pathlib` 是 **Python 3.4** 引入的**面向对象的文件系统路径库**。它将路径视为一个对象（`Path` 对象），这个对象封装了对路径的所有操作（拼接、判断类型、查找、读写、属性获取）。

### 简单的路径拼接

```python
from pathlib import Path

# 无需 os.path.join，直接用 "/" 运算符
p = Path("docs") / "projects" / "readme.txt"  
```

### 创建目录

```python
from pathlib import Path

# 创建单个目录
Path('new_folder').mkdir()

# 自动创建父目录 + 避免已存在错误
Path('parent/child/grandchild').mkdir(parents=True, exist_ok=True)
```

### 删除文件

```python
from pathlib import Path

# 创建 Path 对象
file_path = Path('temp.txt')

# 检查存在性后删除
if file_path.exists() and file_path.is_file():
    file_path.unlink()  # unlink() 等同于 os.remove()
    print("文件已删除")
else:
    print("文件不存在")
```

### 移动和重命名

```python
from pathlib import Path

# 创建 Path 对象
p = Path('old_name.txt')

# 重命名
p.rename('new_name.txt')

# 移动并重命名
p.rename('backup/new_name.txt')

# 使用 replace（会覆盖已存在文件）
p.replace('existing_file.txt')
```

### 遍历所有指定文件的内容

```python
from pathlib import Path

for file_path in Path('.').glob('*.txt'):
    content = file_path.read_text(encoding='utf-8')
```

