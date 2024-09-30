---
title: 科研日记(2024-1)
date: 2024-02-26 15:37:37
layout: hidden
mermaid: true
tags:
abbrlink: research-2024-1
---

{% timeline user:马鹏飞 api:https://memory.pengfeima.cn/api/v1/memo?creatorId=1&limit=10 type:memos %}
{% endtimeline %}


{% timeline %}
<!-- node 2024 年 2 月 26 日 15:37 -->
重新跑理想左心室的程序。

<!-- node 2024 年 2 月 26 日 21:22 -->
观察直杆弯曲的结果，增加不可压约束的惩罚系数会减小直杆的弯曲程度。因此，需要**增大惩罚系数，用隐格式，用多种参数再跑一遍**。
![image-20240226212442750](https://githubimages.pengfeima.cn/images/202402262124977.png)
<!-- node 2024 年 2 月 26 日 21:50 -->
在 `/home/kokkos/ssh/npuheart/build` 目录下运行了9个模拟直杆弯曲的程序。
```bash
taskset -c 5 nohup ./test/fsi_bar_bending --Nt 1000000 --Nb 64 --Ns 30 --kappa 100000 &
disown
taskset -c 6 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 96 --Ns 30 --kappa 100000 &
disown
taskset -c 7 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 128 --Ns 30 --kappa 100000 &
disown
taskset -c 8 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 64 --Ns 40 --kappa 100000 &
disown
taskset -c 9 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 96 --Ns 40 --kappa 100000 &
disown
taskset -c 10 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 128 --Ns 40 --kappa 100000 &
disown
taskset -c 11 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 64 --Ns 50 --kappa 100000 &
disown
taskset -c 12 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 96 --Ns 50 --kappa 100000 &
disown
taskset -c 13 nohup ./test/fsi_bar_bending_implicit_JFNK --Nt 1000000 --Nb 128 --Ns 50 --kappa 100000 &
disown
```
<!-- node 2024 年 2 月 26 日 21:50 -->
需要 8 个程序
参考
```
lid_driven_sphere_30_64_10000_10.0_with_1.00e-02_1.00e-01 成功
lid_driven_sphere_30_96_10000_10.0_with_1.00e-02_1.00e-01 失败
```

```bash
taskset -c 14 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 10000 -T 10 --Nb 64 --Ns 30 --mus 0.1 --muf 0.01 &
```

<!-- node 2024 年 2 月 26 日 23:30 -->

计算能量和体积。
```c++
auto kinematic_energy = ibm.fluid_solver->kinematic_energy(un, vn, wn);
LOG_F(WATCH, "The kinematic_energy is %.16e.\n", kinematic_energy);

auto potential_energy = ibm.solid_solver->energy_norm(ibm._solid_displacement_2);
LOG_F(WATCH, "The potential_energy is %.16e.\n", potential_energy);
// 输出固体当前构型的
auto volumes_sum = ibm.solid_mesh->current_area(ibm._solid_displacement);
LOG_F(WATCH, "The volume of the solid is %.16e.\n", volumes_sum);
```
<!-- node 2024 年 2 月 27 日 10:21 -->

运行有对流项的显隐格式共计8个程序，数据结果位于`/home/kokkos/ssh/npuheart/build_sphere`
``` bash
taskset -c 28 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 10000 -T 10 --Nb 64 --Ns 30 --mus 0.1 --muf 0.01 --lamb 100 &
disown
taskset -c 29 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 5000 -T 10 --Nb 64 --Ns 30 --mus 0.1 --muf 0.01 --lamb 100 &
disown
taskset -c 30 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 10000 -T 10 --Nb 64 --Ns 30 --mus 1.0 --muf 0.01 --lamb 100 &
disown
taskset -c 31 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 5000 -T 10 --Nb 64 --Ns 30 --mus 1.0 --muf 0.01 --lamb 100 &
disown
taskset -c 32 nohup ./test/fsi_lid_driven_sphere --Nt 10000 -T 10 --Nb 64 --Ns 30 --mus 0.1 --muf 0.01 --lamb 100 &
disown
taskset -c 33 nohup ./test/fsi_lid_driven_sphere --Nt 5000 -T 10 --Nb 64 --Ns 30 --mus 0.1 --muf 0.01 --lamb 100 &
disown
taskset -c 34 nohup ./test/fsi_lid_driven_sphere --Nt 10000 -T 10 --Nb 64 --Ns 30 --mus 1.0 --muf 0.01 --lamb 100 &
disown
taskset -c 35 nohup ./test/fsi_lid_driven_sphere --Nt 5000 -T 10 --Nb 64 --Ns 30 --mus 1.0 --muf 0.01 --lamb 100 &
disown
```
<!-- node 2024 年 2 月 27 日 15:43 -->
取消使用对流项，再运行8个程序。
```bash
rm -r build_sphere_noadvection
cmake -B build_sphere_noadvection -DNPUHEART_ADVECTION=false
cmake --build build_sphere_noadvection --target fsi_lid_driven_sphere_implicit_JFNK -j16
cmake --build build_sphere_noadvection --target fsi_lid_driven_sphere -j16
cd build_sphere_noadvection
```
<!-- node 2024 年 2 月 27 日 17:34 -->
计算理想左心室舒张的应变
```
cd /home/kokkos/ssh/npuheart/build_sphere_noadvection
make fsi_ideal_LV_diastole fsi_ideal_LV_diastole_implicit_JFNK -j32
taskset -c 36 nohup ./test/fsi_ideal_LV_diastole --Ns 30 --Nb 64 --Nt 400000 -T 10 --kappa 1000000 --beta 50000000 &
disown
taskset -c 38 nohup ./test/fsi_ideal_LV_diastole_implicit_JFNK --Ns 30 --Nb 64 --Nt 16000 -T 2 --kappa 1000000 --beta 50000000 &
disown
```
<!-- node 2024 年 2 月 27 日 18:48 -->
LV_diastole_3D_explicit_30_64_400000_10_0.04

<!-- node 2024 年 2 月 28 日 17:29 -->
### 重新跑真实左心室算例

<!-- node 2024 年 3 月 2 日 17:29 -->
``` bash
taskset -c 28 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 20000 -T 10 --Nb 64 --Ns 30 --mus 10.0 --muf 0.01 --lamb 100 &
disown
taskset -c 30 nohup ./test/fsi_lid_driven_sphere_implicit_JFNK --Nt 20000 -T 10 --Nb 64 --Ns 30 --mus 100.0 --muf 0.01 --lamb 100 &
disown
taskset -c 32 nohup ./test/fsi_lid_driven_sphere --Nt 20000 -T 10 --Nb 64 --Ns 30 --mus 10.0 --muf 0.01 --lamb 100 &
disown
taskset -c 34 nohup ./test/fsi_lid_driven_sphere --Nt 20000 -T 10 --Nb 64 --Ns 30 --mus 100.0 --muf 0.01 --lamb 100 &
disown
```

{% endtimeline %}

```mermaid
graph LR
A(Section A) -->|option 1| B(Section A)
B -->|option 2| C(Section C)
B -->|option 3| C(Section C)
```
