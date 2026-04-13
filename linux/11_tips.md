# Linux 技巧

### 查看 CPU 核数
**方法一**
```bash
cat /proc/cpuinfo | grep "cpu cores" | wc -l
```
**方法二**
```bash
lscpu | grep "^CPU(s):"
```

### 按列合并不同文件

```bash
paste file1.txt file2.txt > file.txt
```

### 指定 GPU

```bash
export CUDA_VISIBLE_DEVICES=0  # 指定使用0号GPU
