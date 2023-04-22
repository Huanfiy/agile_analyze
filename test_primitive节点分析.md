# test_primitive节点分析

### 分析

研究agile_autobomy节点，发现仿真运行时**perform_global_planning**这个bool开关是flase，结合该节点的C++文件中有这样一段代码：

```c++
 if (perform_global_planning_) {
      ROS_INFO(
          "Not yet switching state machine, waiting for global 			  reference to arrive...");
      setup_done_ = false;
      only_expert_ = only_expert;
      std_msgs::Float32 max_speed_msg;
      max_speed_msg.data = maneuver_velocity_;
      compute_global_path_pub_.publish(max_speed_msg);
    } else {
```

猜想仿真运行时test_primitive节点不会执行，不会对仿真时飞机避障造成影响。



### 参考资料

![image-20230418144225240](/home/huan/.config/Typora/typora-user-images/image-20230418144225240.png)

   GitHub上这段说明暗示该全局开关的作用是开启训练。

​	仿真所加载的参数文件中，该变量的值也为flase

![image-20230418145216450](/home/huan/.config/Typora/typora-user-images/image-20230418145216450.png)



### test_primitive节点（ellipsoid_planner_node.cpp）

分析该节点源码，逻辑汇总后可以认为该节点受**perform_global_planning**的控制，该节点对真机飞行无帮助，可以剥离。

### 该节点的运行逻辑



![image-20230418160243043](/home/huan/.config/Typora/typora-user-images/image-20230418160243043.png)

### 仿真验证

符合预期。

以前的节点图

![image-20230418162813218](/home/huan/.config/Typora/typora-user-images/image-20230418162813218.png)

现在的节点图

![image-20230418162243977](/home/huan/.config/Typora/typora-user-images/image-20230418162243977.png)

## 为什么要这样推进项目？

该开源工程所给源码中只给了仿真示例，无法一键真机部署，工程体量较大，无法自下而上做加法，而自上而下做减法在当前是一个可行的途径，也便于一步步深入探索。