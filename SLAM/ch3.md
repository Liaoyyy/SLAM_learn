## 三维空间刚体运动-坐标变换表示方法

### 变换矩阵（齐次变换矩阵）:
*变换矩阵$R_{12}$可表示向量从坐标系2到坐标系1的变换矩阵* 

1. 定义n维旋转矩阵集合定义为:
$$
SO(n)=\{R \in \mathbb{R}^{n\times n} | RR^T=I, det(R)=1\}
$$

2. 坐标变换公式($t_{12}$为平移向量)
$$
a_1=R_{12}a_2+t_{12} 
$$

3. 齐次矩阵
   齐次矩阵即将旋转加平移合并为一个矩阵进行表示
   $$
   \begin{bmatrix}
   a' \\ 1
   \end{bmatrix}=
   \begin{bmatrix}
   R & t \\
   0^T & 1
   \end{bmatrix}
   \begin{bmatrix}
   a \\ 1
   \end{bmatrix} 
   $$
   记$T=   \begin{bmatrix}
   R & t \\
   0^T & 1
   \end{bmatrix}$为齐次变换矩阵

problem:
1. 旋转矩阵都是正交矩阵？
   [验证旋转矩阵是正交矩阵](https://zhuanlan.zhihu.com/p/419854977)

### 旋转向量
*任意旋转均可以用一个旋转轴和旋转角来进行刻画，考虑用一个向量，其方向与旋转轴一致，长度等于旋转角--旋转向量*

1. 旋转向量:$\theta \boldsymbol{n}$

2. 旋转矩阵与旋转向量关系(罗德里格斯公式):
   $$
   \boldsymbol{R}=cos\theta \boldsymbol{I}+(1-cos\theta)\boldsymbol{n}\boldsymbol{n}^T+sin\theta \cdot tr(\hat{\boldsymbol{n}})
   $$
   其中$\hat{\boldsymbol{n}}$为向量转变为的反对称矩阵
   对应的：
   $$
   tr(\boldsymbol{R})=1+2cos\theta \\
   \theta=arccos\frac{tr(\boldsymbol{R})-1}{2}
   $$

3. 特性:
   对转轴$\boldsymbol{n}$，旋转轴上向量在宣州后不发生改变，即
   $$
   \boldsymbol{R} \boldsymbol{n} = \boldsymbol{n}
   $$
   故转轴$\boldsymbol{n}$为矩阵$\boldsymbol{R}$特征值为1对应的特征向量

### 欧拉角:
1. 欧拉角定义:
   绕物体Z轴旋转得到偏航角yaw,绕物体Y轴旋转得到偏航角pitch，绕物体Z轴旋转得到偏航角roll

2. 使用欧拉角描述旋转的问题:
   当多次旋转时，存在万向锁问题，系统丢失一个自由度   [万象锁问题](https://zhuanlan.zhihu.com/p/346718090)

problem:
1. 为什么使用欧拉角存在万向锁问题?
   描述一个旋转实际需要四个独立变量，如使用旋转向量需要旋转角度(1个变量)与旋转轴方向(3个变量)，而使用欧拉角想要充分描述旋转需要补充旋转顺序这一变量

### 四元数:
*二维旋转可以使用单位复数进行表示，三维旋转可以使用单位四元数表示*

1. 定义:
   $$
   \boldsymbol{q}=q_0+q_1i+q_2j+q_3k,\\
   \begin{cases}
   i^2 = j^2 = k^2 = -1 \\
    ij=k, ji=-k \\
    jk=i,kj=-i \\
    ki=j,ik=-j
   \end{cases}
   $$
   也可记四元数为
   $$
   \boldsymbol{q}=\begin{bmatrix} s & \boldsymbol{v}\end{bmatrix}^T
   $$

2. 四元数基本运算:
   参考:[四元数基本运算](https://blog.csdn.net/m0_37874102/article/details/114847374)

3. **四元数描述旋转:**
   空间中有一个三维点$\boldsymbol{p}$(但为四维向量[0,x,y,z])，旋转后为$\boldsymbol{p}'$
   则满足
   $$
   \boldsymbol{p}'=\boldsymbol{q} \boldsymbol{p} \boldsymbol{q} ^{-1}
   $$