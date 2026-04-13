# VASP 便捷命令
判断结构优化是否收敛：

```bash
grep accuracy OUTCAR
# 若收敛则出现下面这句
 reached required accuracy - stopping structural energy minimisation
```

提取运行时间：

```bash
# 总时间
grep User OUTCAR
                            User time (sec):       55.948
grep Elapsed OUTCAR
                         Elapsed time (sec):       59.198
# 查看每一个离子步花去的时间
grep LOOP+ OUTCAR  
# 查看每一个电子步以及离子步花去的时间
grep LOOP OUTCAR  
```

提取价电子数：

```bash
grep ZVAL OUTCAR
```

提取体积：

```bash
grep volume OUTCAR
  volume/ion in A,a.u.               =      85.36       576.01
  volume of cell :      256.07
# 第一行给出体系的体积分别以Å^3/atom, a.u.^3/atom为单位给出的。
# 第二行给出体系的体积是以Å^3/unit cell为单位给出的。
```

提取费米能级：

```bash
grep Fermi OUTCAR | tail -1
 Fermi energy:        -6.3439321217
```

提取K点个数：

```bash
grep irreducible OUTCAR
 Found     56 irreducible k-points:
```

提取频率：

```bash
grep cm-1 OUTCAR  # 查看频率
grep f/i OUTCAR  # 查看虚频
```

提取POTCAR元素种类：

```bash
grep TIT POTCAR
```

提取POTCAR截断能量：

```bash
grep ENMAX POTCAR
```

查看PBE-D2，PBE-D3范德华矫正的能量：

```bash
grep Edisp OUTCAR | tail -1
```

查看磁矩：

```bash
cat OSZICAR | grep mag
```

查看能量：

```bash
cat OSZICAR | grep F | awk '{printf("%.3f\n" ,$5)}' | tail -1 
```

查看体系受力情况：

```bash
vim OUTCAR
g/TOTAL-FORCE
```

查看体系倒格矢：

```bash
vim OUTCAR
g/reciprocal
```

