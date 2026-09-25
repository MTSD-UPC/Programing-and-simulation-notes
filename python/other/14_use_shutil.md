# shutil 库

`shutil` 是 Python 的**高级文件操作模块**，可以把它理解为 `os` 模块的“高配版”或“增强版”。`shutil` 专门处理**复制、移动、删除整个目录树**以及**创建/解压归档文件（压缩包）**。

| 功能分类       | 核心函数                                               | 作用                       |
| ---------- | -------------------------------------------------- | ------------------------ |
| **复制文件**   | `shutil.copy()`, `shutil.copy2()`                  | 复制单个文件（`copy2` 会保留元数据）   |
| **复制目录**   | `shutil.copytree()`                                | 递归复制整个目录树（含子目录）          |
| **移动/重命名** | `shutil.move()`                                    | 移动文件或目录（**支持跨文件系统**）     |
| **删除目录**   | `shutil.rmtree()`                                  | 递归删除整个目录（**危险！不可恢复**）    |
| **压缩/归档**  | `shutil.make_archive()`, `shutil.unpack_archive()` | 创建或解压 `.zip`、`.tar` 等压缩包 |
| **磁盘信息**   | `shutil.disk_usage()`                              | 查看磁盘总容量、已用、剩余空间          |
| **查找命令**   | `shutil.which()`                                   | 在系统 PATH 中查找可执行文件路径      |

如果你需要**搬移整个目录、复制带权限的文件、做压缩包**，优先想到 `shutil` 而不是手写递归或纠结 `os` + `zipfile` 的组合拳。

```python
import shutil

# 1. 创建压缩包（支持 zip, tar, gztar, bztar, xztar）
# 参数：压缩包名（不带后缀）, 格式, 要压缩的源目录
archive_path = shutil.make_archive('backup_2026', 'zip', 'my_project')
print(f"压缩包已创建: {archive_path}")

# 2. 解压压缩包
shutil.unpack_archive('backup_2026.zip', 'extract_folder')
```

