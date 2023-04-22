# 分析generate_label节点

### 分析：

​		simulation.launch启动八个generate_label相同功能节点，在该组节点中以**<u>1</u>**号节点为主，接受***"start_label"***话题的启动指令后，八个节点并行活动(执行内容细微差别)，加载里程计，点云，参考路径数据，全部加载完毕后发布话题***"labelling_completed"***，发现该话题供dagger_training节点订阅，用于训练，故推断这部分节点对实机飞行没有贡献，可以剥离。

### 验证：

​		修改simulation.launch文件，除去generate_label节点，进行仿真，仿真结果不受影响。该仿真运行为测试路径追踪，没有启动训练相关的节点，故一堆generate_label节点在该仿真中无作用。

### 今日总结：

剥离无用节点，减小工程复杂度.





