# Ovito 输出高清图片
链接 1：[lammps教程：ovito免费输出高清图片方法 - 知乎](https://zhuanlan.zhihu.com/p/620843703)  
链接 2：[Ovito高清图渲染参数设置 - 知乎](https://zhuanlan.zhihu.com/p/663051302)

### 参考 python 代码

```python
from ovito.io import import_file
from ovito.vis import Viewport,TachyonRenderer

pipeline = import_file('test.data')
pipeline.add_to_scene()
vp = Viewport()
vp = Viewport(type = Viewport.Type.Perspective, camera_dir = (0, 1, 0))
vp.zoom_all()
vp.render_image(filename='simulation.png', 
                size=(3200, 2400), 
                renderer=TachyonRenderer(shadows=False, direct_light_intensity=1.1))
```

### 具体流程

**读入轨迹文件**

```python
pipeline = import_file('dump.xyz')
```

**将显示对象插入到场景中**

```python
pipeline.add_to_scene()
```

**设置视角**

```python
vp = Viewport(type = Viewport.Type.Perspective, camera_dir = (1, 1, 1))
```

**全局缩放**

```python
vp.zoom_all()
```

**配置渲染类型并输出图片**

```python
vp.render_image(filename='simulation.png', 
                size=(3200, 2400), 
                renderer=TachyonRenderer(shadows=False, direct_light_intensity=1.1))
```
