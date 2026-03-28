# 技巧

### 查看 CPU 核数
**方法一**
```bash
cat /proc/cpuinfo | grep "cpu cores" | wc -l
```
**方法二**
```bash
lscpu | grep "^CPU(s):"
```
