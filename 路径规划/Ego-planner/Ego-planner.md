# Ego-planner论文阅读
*一个中文版的论文框架讲解:*
- [EGO-Planner论文阅读笔记](https://zhuanlan.zhihu.com/p/366372048?utm_id=0)

# Ego-planner代码阅读

## 目录
- src目录下:
  - planner
    - bspline-opt:B样条部分函数
    - drone-detect:多机检测部分，单机不用管 
    - path_searching:A star轨迹规划部分
    - plan_env:地图部分
    - plan_manage:主体部分，main函数入口(包括launch文件)
    - rosmsg_tcp_bridge:多机通信不用管
    - traj_utils:可视化、minimum_snap部分