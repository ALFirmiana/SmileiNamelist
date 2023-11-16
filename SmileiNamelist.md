# 简介

Smilei的namelist是其输入文件，使用Python语言，基本组成是一系列Smilei定义的类（代码块）。在类之外，可以自由的进行模块导入和计算等操作。本文档根据Smilei的官方文档^[<https://smileipic.github.io/Smilei/Use/namelist.html>],主要介绍各个类与参数的意义与使用。

## Main

Main类是必须的，一个典型的Main如下

```python
Main(
    geometry = "1Dcartesian",
    interpolation_order = 2,
    interpolator = "momentum-conserving",
    grid_length  = [16. ],
    cell_length = [0.01],
    simulation_time    = 15.,
    timestep    = 0.005,
    number_of_patches = [64],
    cluster_width = 5,
    maxwell_solver = 'Yee',
    EM_boundary_conditions = [
        ["silver-muller", "silver-muller"],
#       ["silver-muller", "silver-muller"],
#       ["silver-muller", "silver-muller"],
    ],
    time_fields_frozen = 0.,
    reference_angular_frequency_SI = 0.,
    print_every = 100,
    random_seed = 0,
)
```

- `geometry`
  模拟维数，可选参数为
  - `1Dcartesian`：1维(x)
  - `2Dcartesion`：2维(x,y)
  - `3Dcartesion`：3维(x,y,z)
  - `AMcylindrical`：方位模分解的柱坐标(x,r)

- `interpolation_order`
  插值阶数，可选参数为
  - **`2`**^[加粗为默认值，下略]：2阶插值多项式，三点法
  - `4`：4阶插值多项式，五点法，向量化2维几何模拟不可用

- `interpolator`
  插值算法，可选参数为
  - **`monentum-conserving`**：
  - `wt`:一种随时间步长的插值算法^[https://doi.org/10.1016/j.jcp.2020.109388]
