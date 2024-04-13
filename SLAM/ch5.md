## 相机模型

### 针孔相机模型（单目相机）
- 相机坐标系:以相机平面为xoy平面，向右为x轴正方向，向下为y轴正方向,向前为正
- 成像平面:与相机坐标系距离相差焦距f，成像平面坐标系为$O'-x'-y'-z'$
- 像素坐标系:原点$o'$位于图像左上角,u轴向右与x轴正方向平行，v轴向下与y轴平行，**像素坐标系与成像平面间差一个缩放和原点平移**
  
在像素坐标系下坐标为$(u,v)$，在相机坐标系下对应点坐标为$(X,Y,Z)$，则满足关系:
$$
Z
\begin{bmatrix}
u \\ v \\ 1
\end{bmatrix}=
\begin{bmatrix}
f_x & 0 & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
X \\ Y \\ Z
\end{bmatrix}
$$
其中$f_x,f_y,c_x,c_y$单位为像素，中间矩阵为内参数矩阵记为$K$

相机位姿$R,t$称为相机外参数

由于单目相机无法测距，即Z值未知，故一般取Z=1得到对应**归一化平面坐标**

### 双目相机模型与RGB-D相机模型
相比于单目相机可以测得目标距离

原理：[双目、结构光、tof，三种深度相机的原理区别](https://www.oakchina.cn/2023/05/16/3_depth_cams/)

## 图像
OpenCv学习:
[OpenCv官方文档](https://docs.opencv.org/3.4.11/d9/df8/tutorial_root.html)
[OpenCv函数整理](https://blog.csdn.net/Python_0011/article/details/131871546?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171098198116777224486229%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171098198116777224486229&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-131871546-null-null.142^v99^pc_search_result_base4&utm_term=opencv&spm=1018.2226.3001.4449)

OpenCV访问点:
[访问Mat某个像素点](https://blog.csdn.net/qq_42731705/article/details/121522770)