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
  模拟维数，可选参量为
  1. `1Dcartesian`：1维(x)
  1. `2Dcartesion`：2维(x,y)
  1. `3Dcartesion`：3维(x,y,z)
  1. `AMcylindrical`：方位模分解的柱坐标(x,r)

- `interpolation_order`
  插值阶数，可选参量为
  1. **`2`**^[加粗为默认值，下略]：2阶插值多项式，三点法
  1. `4`：4阶插值多项式，五点法，向量化2维几何模拟不可用

- `interpolator`
  插值算法，可选参量为
  1. **`monentum-conserving`**：不知道
  1. `wt`:一种随时间步长的插值算法^[<https://doi.org/10.1016/j.jcp.2020.109388>]

- `grid_length` or `number_of_cells`
  参量为长度等同于维数的列表，代表模拟空间的大小，可选两个参数：
  - `grid_length`：每个维度方向的长度^[使用的归一化单位参考<https://smileipic.github.io/Smilei/Understand/units.html>，下略]
  - `number_of_cells`：每个维度方向的网格数
  
- `cell_length`
  参量为长度等同于维数的浮点数列表，代表每个网格的大小

- `simulation_time` or `number_of_steps`
  模拟时长，可选两个参数：
  - `simulation_time`：模拟时长
  - `number_of_steps`：模拟步数

- `timestep` or `timestep_over_CFL`
  时间步长，可选两个参数：
  - `timestep`：时间步长，归一化单位
  - `timestep_over_CFL`：时间不长，*Courant-Friedrichs-Lewy*(CFL)时间单位

- `gpu_computing`
  是否使用GPU加速，可选参量为
  1. **`False`**：不启用
  2. **`True`**：启用

- `number_of_patches`
  参量为长度等同于维数的整形列表，代表每个方向上划分的patches数。举例来说，对于二维模拟，参量为`[nx,ny]`，要求nx、ny均为2的整数次幂，且划分总数$nx*ny$大于等于并行线程数
