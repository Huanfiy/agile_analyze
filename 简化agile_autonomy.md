# 简化agile_autonomy源码

### 概要

​	基于前两天的节点分析，现可对agile_autonomy.cpp进行一部分源码裁剪

### 改变

删减：

```C++
  compute_global_path_pub_ =
      pnh_.advertise<std_msgs::Float32>("compute_global_plan", 1);

  compute_global_path_pub_.publish(max_speed_msg);

  completed_global_plan_sub_ = nh_.subscribe("completed_global_plan", 1,
       &AgileAutonomy::completedGlobalPlanCallback, this);

//针对test_primitive的回调
 void AgileAutonomy::completedGlobalPlanCallback
```

仿真验证不受影响。



## test_trajectories.py

仿真运行时，运行**test_trajectories.py**以启动飞行计划，该文件主要执行了：

```python
    trainer.perform_testing()
```

此函数体在**dagger_training.py**中定义，在**test_settings.yaml**文件中**max_rollouts**定义了循环仿真执行次数

