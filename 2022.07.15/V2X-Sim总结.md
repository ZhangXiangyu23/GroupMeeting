# V2X-Sim总结

## 一、简介

V2X-Sim，Vehicle-to-Everything 模拟的简称，是由纽约大学 AI4CE 实验室和上海交通大学 MediaBrain团队开发的第一个合成的 V2X 辅助自动驾驶协同感知数据集，旨在激发多智能体多模态多任务感知研究。由于 V2X 的不成熟以及同时操作多辆自动驾驶汽车的成本，为研究社区构建这样一个真实世界的数据集非常昂贵和费力。因此，使用高度逼真的 CARLA-SUMO 联合仿真来确保我们的数据集与现实世界的驾驶场景相比具有代表性。

## 二、传感器设置和数据收集

### 1.传感器

![img](https://ai4ce.github.io/V2X-Sim/img/SensorSetupNew.PNG)

（1）每辆车配备6个RGB /深度/语义分割摄像头、1个BEV语义分割 摄像头、1个LiDAR和语义LiDAR传感器、1个IMU和1个GNSS传感器。

（2）在路边，我们将 4 个RGB摄像头、一个BEV 语义分割摄像头、一个LiDAR和一个语义 LiDAR连接到基础设施。车辆和路边基础设施上的摄像头都能够覆盖 360°，以确保全方位感知。

### 2.地图

数据集中考虑了 CARLA 的 Town 3、Town 4 和 Town 5。使用上述 CARLA-SUMO 联合仿真来生成多种类型的车辆，例如汽车、自行车、摩托车和紧急车辆，以从记录文件中检索传感器数据。

![image-20220713150634102](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713150634102.png)

## 三、数据集的使用

### 1.前置条件：需要下载nuScenes devkit包

### 2. 加载数据集

```python
from  nuscenes.nuscenes  import  NuScenes 
import  matplotlib.pyplot  as  plt 
import  numpy  as  np 
import  os 
from  IPython.display  import  Image 
import  collections

in_path = 'V2X-Sim数据集路径'

nusc = NuScenes(version='v1.0-mini', dataroot=in_path, verbose=True)
```

![image-20220713151057886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713151057886.png)

### 3.场景信息

```python
# To investigate a single scene

my_scene = nusc.scene[0]
my_scene

```

![image-20220713151200399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713151200399.png)

### 4.sample信息

```python
my_scene = nusc.scene[0]
first_sample_token = my_scene['first_sample_token']
my_sample = nusc.get('sample', first_sample_token)

my_sample
```

![image-20220713151239856](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713151239856.png)

### 5.传感器数据可视化