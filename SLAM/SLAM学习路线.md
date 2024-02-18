
# 坐标变换
*目标：已知坐标系A与坐标系B，如何将坐标系B中向量P转化为坐标系A中向量*
参考资料：[坐标变换讲解](https://www.zhihu.com/tardis/zm/art/78987582?source_id=1005)

# SLAM建图
## 常用地图结构
参考资料:
- [常用地图结构和基础知识](https://blog.csdn.net/Night___Raid/article/details/110917284)

# 路径规划
## 拓扑图的传统路径规划算法
1. A star算法： [Introduction to the A* Algorithm](https://www.redblobgames.com/pathfinding/a-star/introduction.html)
2. RRT star算法

### 传统算法为什么不应用于实际？
1. 平滑性太差，不利于实际机器运动---改进方法：将A star传统方法得到的路径进行插值得到平滑路径（这样做好处在于A star、RRT star能保证不撞到障碍物）
2. 传统算法得到路径间隙值太小，传统算法通常仅考虑距离最短而忽略机器本身体积在靠近障碍物时影响，实际引用中需要考虑这一因素
   

## 路径规划算法
*目标：找到一条安全、平滑且动力学可行的轨迹*
1. GTO(Gradient-Based Trajectory Optimizer 基于梯度的轨迹优化)
   参考资料:
    - [基于梯度的轨迹优化（GTO）和拓扑路径搜索的路径引导的无人机（UAV）轨迹再规划（PGO）](https://blog.csdn.net/weixin_44673253/article/details/126864377#:~:text=%E5%9F%BA%E4%BA%8E%E6%A2%AF%E5%BA%A6%E7%9A%84%E8%BD%A8%E8%BF%B9%E4%BC%98%E5%8C%96%EF%BC%88GTO%EF%BC%89%E5%92%8C%E6%8B%93%E6%89%91%E8%B7%AF%E5%BE%84%E6%90%9C%E7%B4%A2%E7%9A%84%E8%B7%AF%E5%BE%84%E5%BC%95%E5%AF%BC%E7%9A%84%E6%97%A0%E4%BA%BA%E6%9C%BA%EF%BC%88UAV%EF%BC%89%E8%BD%A8%E8%BF%B9%E5%86%8D%E8%A7%84%E5%88%92%EF%BC%88PGO%EF%BC%89%201%200%20%E6%A6%82%E8%A7%88%20%E5%9F%BA%E4%BA%8E%E6%A2%AF%E5%BA%A6%E7%9A%84%E8%BD%A8%E8%BF%B9%E4%BC%98%E5%8C%96%20GTO%20%E5%9C%A8%E5%9B%9B%E6%97%8B%E7%BF%BC%E9%A3%9E%E8%A1%8C%E5%99%A8%E7%9A%84%E8%BD%A8%E8%BF%B9%E9%87%8D%E6%96%B0%E8%A7%84%E5%88%92%E4%B8%AD%E5%BE%97%E5%88%B0%E4%BA%86%E5%B9%BF%E6%B3%9B%E7%9A%84%E5%BA%94%E7%94%A8%EF%BC%8C%E4%BD%86%E6%98%AF%E5%AE%83%E5%AD%98%E5%9C%A8%20%E5%B1%80%E9%83%A8%E6%9E%81%E5%B0%8F%E5%80%BC,2%20%E7%9B%B8%E5%85%B3%E5%B7%A5%E4%BD%9C%202.1%20%E5%9F%BA%E4%BA%8E%E6%A2%AF%E5%BA%A6%E7%9A%84%E8%BD%A8%E8%BF%B9%E4%BC%98%E5%8C%96%20GTO%20%E6%96%B9%E6%B3%95%EF%BC%8C%E6%98%AF%E4%B8%80%E7%A7%8D%E4%B8%BB%E8%A6%81%E7%9A%84%E8%B7%AF%E5%BE%84%E7%94%9F%E6%88%90%E7%AE%97%E6%B3%95%EF%BC%8C%E5%B9%B6%E6%8A%8A%E8%B7%AF%E5%BE%84%E7%94%9F%E6%88%90%E7%9C%8B%E4%BD%9C%E6%98%AF%E4%B8%80%E4%B8%AA%E6%9C%80%E5%B0%8F%E5%8C%96%E7%9B%AE%E6%A0%87%E5%87%BD%E6%95%B0%E7%9A%84%E9%9D%9E%E7%BA%BF%E6%80%A7%E4%BC%98%E5%8C%96%E9%97%AE%E9%A2%98%EF%BC%8C%E4%BD%86%E6%98%AF%E4%B9%9F%E6%9C%89%E4%B8%80%E4%BA%9B%E5%B0%8F%E9%97%AE%E9%A2%98%EF%BC%8C%E5%B0%B1%E6%98%AF%20%E5%B1%80%E9%83%A8%E6%9C%80%E5%B0%8F%E5%80%BC%20) 
    - [基于梯度的轨迹优化算法GTOP](https://f.daixianiu.cn/csdn/28817591038149415.html)

2. Minimum Snap
   参考资料:
   - [Minimum Snap轨迹生成](https://blog.csdn.net/u011341856/article/details/121861930)**（写的非常好的一篇）**
   - [基于硬约束和软约束的轨迹规划](https://blog.csdn.net/Travis_X/article/details/114905526?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168615911216782427445108%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=168615911216782427445108&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-1-114905526-null-null.268%5Ev1%5Ekoosearch&utm_term=%E7%BA%A6%E6%9D%9F&spm=1018.2226.3001.4450)