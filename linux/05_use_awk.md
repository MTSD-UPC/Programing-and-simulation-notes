# awk 命令

### 简单用法

获取第 3 列的数据

```bash
awk '{print $3}' data.txt
```

获取第 3 列最小值

```bash
awk '{print $3}' data.txt | sort -n | head -1
```
