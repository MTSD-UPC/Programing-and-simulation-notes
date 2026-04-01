# 查看 AIMD 的能量变化

+ read 函数来自 ASE 库，用于读取 VASP 结果文件
+ get_total_energy 方法来自 ASE 库，用于读取 Atoms 对象的总能量

```python
from ase.io import read
import matplotlib.pyplot as plt

input_dir = "/path/to/your/directory/"

structures = read(f"{input_dir}/vasprun.xml", index="::", format="vasp-xml")
print("Frames:", len(structures))

f = lambda x: x.get_total_energy()
energy_list = list(map(f, structures))

fig, ax = plt.subplots()
ax.plot(range(1, len(structures) + 1), energy_list, color="orange")
ax.set_xlabel("Structure")
ax.set_ylabel("Energy (eV)")
plt.tight_layout()
plt.show()
fig.savefig("result.png", dpi=900)
