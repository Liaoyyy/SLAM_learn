# 后端
## 概述
后端主要负责数据的再次处理(滤波等)以最大化估计的当前状态的概率，目前主要有两种方法：
1. 非线性卡尔曼滤波（基于马尔科夫性假设）
2. 非线性优化（认为当前状态与之前所有状态均有关）--SLAM主流

## 线性系统和KF
线性高斯系统可以用运动方程+观测方程描述
$$
\begin{cases}
x_k= A_k x_{k-1}+u_k+w_k\\
z_k = C_k x_k + v_k
\end{cases}
$$
其中$x_k$为$k$时刻状态向量，$u_k$为输入，$z_k$为观测器观测值，$w_k,v_k$为服从零均值高斯分布的随机噪声

## BA与图优化
可认为像素坐标系下目标特征点的坐标为特征点在世界坐标系下坐标的观测值，记观测值$z=h(T,p)$，其中$T$为特征点从世界坐标系到相机坐标系的齐次变换矩阵，$p$为特征点在空间中坐标。
定义观测误差
$$
e=z-h(T,p)
$$
代价函数为
$$
\frac{1}{2}\sum^m_{i=1}\sum^n_{j=1}||e_{ij}||^2=\frac{1}{2}\sum^m_{i=1}\sum^n_{j=1}||z_{ij}-h(T_i,p_j)||^2
$$
其中，$z_{ij}$表示在位姿$T_i$处观察路(特征点)$p_j$的观测值
将自变量定义为所有待优化的变量
$$
x=[T_1,...,T_m,p_1,....,p_n]^T
$$
雅可比矩阵可分块为
$$
J=[F,E] \\
J=\begin{bmatrix} J_{11} \\ J{12} \\... \\ J_{ij}\end{bmatrix}
$$
其中，$F$为代价函数对相机姿态导数，$E$为代价函数对路标的偏导
由高斯牛顿法可得到
$$
H=J^TJ=
\begin{bmatrix}
F^TF & F^TE \\
E^TF & E^TE
\end{bmatrix}
$$
对多个姿态、目标点优化情况可以将$H$记为
$$
H=\sum_{i,j}J_{ij}^TJ_{ij}
$$
**矩阵H是具有稀疏性质的矩阵**
将$H$记为
$$
H=
\begin{bmatrix}
B & E \\
E^T & C
\end{bmatrix}
$$
其中，$B$只和相机位姿有关，$C$仅和路标点有关
增量方程为
$$
\begin{bmatrix}
B & E \\
E^T & C
\end{bmatrix}
\begin{bmatrix}
\Delta x_c \\ \Delta x_p
\end{bmatrix}=
\begin{bmatrix}
v \\ w
\end{bmatrix}
$$
**舒尔消元(Schur消元或Marginalization)：**
对上式方程左乘矩阵
$$
\begin{bmatrix}
I & -EC^{-1} \\
0 & I
\end{bmatrix}
$$
整理得到
$$
\begin{bmatrix}
B-EC^{-1}E^T & 0 \\
E^T & C
\end{bmatrix}
\begin{bmatrix}
\Delta x_c \\ \Delta x_p
\end{bmatrix}=
\begin{bmatrix}
v-EC^{-1}w \\ w
\end{bmatrix}
$$

*对舒尔消元理解:*
舒尔消元可视为一种边缘化，即求$\Delta x_c， \Delta x_p$问题化为先固定$\Delta x_p$，求解$\Delta x_c$，再求$\Delta x_p$

## 滑动窗口滤波与优化
*单纯BA随着时间推移规模会越来越大，路标点越来越多，考虑仅保留离当前时刻最近的N个关键帧的数据或类似思路的有限个关键帧*


- 新增一个关键帧或路标点：
  滑动窗口已经建立了N个关键帧，新增1个关键帧获得N+1个关键帧和更多路标点的几何，可按照正常BA流程获得N+1个关键帧的结果
- 再删除一个旧的关键帧
  **对一个多元分布，如果我们想去除一个元的影响，可对这个元固定得到剩下变量的条件分布（即对这个元进行边缘化）**
  同样的，我们可以先求得N+1的Hessian矩阵，再进行Schur消元得到除去不想要的关键帧，得到N阶Hessian阵，进一步迭代优化

## 位姿图 
*为什么提出位姿图的优化问题？--按BA方法，多次迭代后其实特征点的空间坐标已经收敛，其实不需要持续将特征点坐标纳入优化函数，带来额外的开销。此外，我们还可以利用一些测量位姿的传感器，融合得到位姿变换（位姿图）*
\
**位姿图优化的节点**：相机位姿
**位姿图优化的边**：两个位姿节点之间相对运动的**估计**--*可以来源于特征点匹配、直接法、GPS、IMU积分*
利用李群描述得到
$$
T_{ij}=T_i^{-1}T_j
$$
由于上式不会精准成立，为构造最小二乘问题，定义误差得到
$$
e_{ij}=\ln(T_{ij}^{-1}T_i^{-1}T_j)^{\nabla}
$$
给$i,j$处位姿左乘一个微扰得到
$$
\begin{align}
\hat{e_{ij}} &=\ln(T_{ij}^{-1}T_i^{-1}\exp{(-\delta \xi_i)}  \exp{(\delta \xi_j)} T_j)^{\nabla}  \nonumber \\
& \approx e_{ij}+\frac{\partial e_{ij}}{\partial \delta \xi_i} \delta \xi_i + \frac{\partial e_{ij}}{\partial \delta \xi_j} \delta \xi_j\nonumber
\end{align}
$$
总体目标函数为
$$
\min \frac{1}{2} \sum_{i,j \in \epsilon} e_{ij}^T \Sigma_{ij}^{-1} e_{ij}
$$

**位姿图的优化实际上是已知两个相机位姿，且利用某种方法知道两个相机之间的位姿变化（例如PnP等方法），在后端进行优化得到更精准的位姿变换关系**