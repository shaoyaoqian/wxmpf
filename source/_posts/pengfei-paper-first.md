---
abbrlink: pengfei-paper-first
title: An Unconditionally Stable Scheme for the IB Method
layout: hidden
date: 2024-06-27 16:15
---


<div class="font-local-title">
    An Unconditionally Stable Scheme for the Immersed Boundary Method with Application in Cardiac Mechanics
</div>

<div class="font-local-author">
    Pengfei Ma, Cai Li, Xuan Wang,<br> Yongheng Wang, Xiaoyu Luo, Hao Gao
</div>



<div class="font-local-content">
  The stability of the immersed boundary (IB) method is a challenge in simulating fluid-structure interaction (FSI) problems, where time step constraints are significantly stricter than in pure fluid simulations. We propose a novel unconditionally stable scheme for 
  the immersed boundary finite element (IBFE) method. The structure is handled implicitly and characterized by strain energy functions, rather than being modeled as fibers or membranes. 
  Through energy estimate, we prove the unconditional stability of the fully discrete approximation in the absence of the convective term. 
  In real simulations of cardiac mechanics problems, the time step is much larger, only limited by the Courant–Friedrichs–Lewy (CFL) condition of the fluid. 
  The novelty of this work lies in the combination of dual interpolation and distribution operators, 
  the Jacobian-free Newton-Krylov (JFNK) method for solving nonlinear algebraic systems, 
  and the semi-Lagrangian method for handling the convective term. 
  To validate the effectiveness and accuracy of our approach, 
  we present various benchmarks and conduct a quasi-static simulation 
  of a three-dimensional real left ventricular model. We have shown that 
  the numerical stability of our scheme is very robust even with much 
  larger time step compared to conventional explicit IB methods. 
  Our work paves the way for further works on efficient solvers of the IBFE method.
</div>

以后复制一个像这样的网站嘿嘿嘿
https://ipc-sim.github.io

<style>
    .font-local-title {
        font-family: 'Times New Roman', Times, serif; font-size: 25px !important;text-align: center;font-weight: bold;
    }
    .font-local-author {
        font-family: 'Times New Roman', Times, serif; font-size: 20px !important;text-align: center;
    }
    .font-local-content {
        font-family: 'Times New Roman', Times, serif !important;
        font-size: 15px !important; /* 使用 !important 确保此样式优先级最高 */
        text-align: justify; /* 两端对齐 */
    }
</style>