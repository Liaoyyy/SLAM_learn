# 创建工作区
```shell
mkdir <workspace_name>
cd <workspace_name>
catkin_init_workspace #初始化
catkin_make 
source devel/setup.bash #加入ROS路径
```

# 创建catkin Package
```shell
catkin_creat_pkg <package_name> [depend1] [depend2] [depend3] #创建包，depend为依赖的包，为可选项
```

## rospack语法
```shell
rospack find <package_name>
rospack depends <package_name> #找到该包所有依赖包，包括间接依赖包
rospack depends1 <package_name> #找到该包直接依赖包
```

## 其他
```shell
catkin_make [make_targets] #等价于make
```

# ROS节点
```shell
rosnode list #查看当前node列表
rosnode info <node_name> #查看某个节点信息
rosrun [package_name] [node_name] #运行某个包中某个节点
rosrun [package_name] [node_name] __name:=[my_node_name] #重命名某个节点
```

# ROS topic
```shell
rostopic echo [topic] # 打印该话题内容
rostopic list （-v） # -v可现实
rostopic type [topic]
rostopic pub [topic] [msg_type] [args]
rostopic hz [topic] # 测话题信息频率
```

# ROS service
```shell
rosservice list #查看运行节点的service
rosservice type [service]
rosservice call [service] [args] #调用当前某个service
```

# roslaunch
*作用：类似于脚本启动多个node*

.launch文件内容:
```xml
<launch>

  <group ns="turtlesim1">
    <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
  </group>

  <group ns="turtlesim2">
    <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
  </group>

  <node pkg="turtlesim" name="mimic" type="mimic">
    <remap from="input" to="turtlesim1/turtle1"/>
    <remap from="output" to="turtlesim2/turtle1"/>
  </node>

</launch>
```
如何写：[Launch文件写法](https://zhuanlan.zhihu.com/p/367357844)

格式
```xml
<launch>
    <node .../>
    <param .../>
    <rosparam .../>
    <include .../>
    <remap .../>
    <arg .../>
    <group>  </group>
</launch>
```

调用:
```shell
roslaunch [package-name] [launch-file-name]
```