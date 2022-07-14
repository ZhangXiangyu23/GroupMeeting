# Nuscenes数据集总结

## 一. 简介

nuScenes 数据集是由Motional（前身为 nuTonomy）团队开发的自动驾驶公共大规模数据集。在波士顿和新加坡收集了 1000 个驾驶场景.

nuScenes 数据集的灵感来自开创性的KITTI数据集。nuScenes 是第一个提供来自自动驾驶汽车的整个传感器套件（6 个摄像头、1 个激光雷达、5 个雷达、GPS、IMU）的数据的大规模数据集。与 KITTI 相比，nuScenes 包含 7 倍以上的对象注释。

## 二.数据采集

### 1.场景规划

在波士顿和新加坡收集了大约 15 小时的驾驶数据。对于完整的 nuScenes 数据集，发布了来自波士顿海港和新加坡 One North、皇后镇和荷兰村地区的数据。

![image-20220713154348253](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713154348253.png)



![image-20220713154424614](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713154424614.png)

### 2.汽车设置

使用两辆具有相同传感器布局的雷诺 Zoe汽车在波士顿和新加坡行驶。数据是从研究平台收集.

![image-20220713154636624](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220713154636624.png)

相机（CAM）有六个，分别分布在前方（Front）、右前方（Front Right）、左前方（Front Left）、后方（Back）、右后方（Back Right）、左后方（Back Left）；激光雷达（LIDAR）有1个，放置在车顶（TOP）；毫米波雷达有五个，分别放置在前方（Front）、右前方（Front Right）、左前方（Front Left）、右后方（Back Right）、左后方（Back Left）。

## 三.数据集的使用

### 1.导入nuscenes-devkit库

```python
pip install nuscenes-devkit
```

### 2.加载数据集信息

```python
from nuscenes.nuscenes import NuScenes
nusc = NuScenes(version='v1.0-mini', dataroot='数据集的具体路径', verbose=True)
```

![image-20220714150733230](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714150733230.png)

### 3.场景scene

使用如下代码，查看当前数据集中所有的场景

```python
nusc.list_scenes()
```

![image-20220714151735762](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714151735762.png)

使用如下代码查看具体某个场景的信息

```python
my_scene = nusc.scene[0]
print(my_scene)
```

![image-20220714152204280](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714152204280.png)

### 4.样本sample

每个scene大约持续20s，那sample就是每0.5秒进行一次采样。也可以这样理解sample和scene，sence相当于20s的视频，sample就是每0.5s取一帧的图像。

使用如下代码，获取具体场景的具体的一个sample的token值

```python
first_sample_token = my_scene['first_sample_token']  #获取第一个sample的token值
print(first_sample_token)
```

![image-20220714153301265](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714153301265.png)

使用如下代码，通过sample的token值获取sample的具体信息

```python
my_sample = nusc.get('sample', first_sample_token)
print(my_sample)
```

![image-20220714153503166](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714153503166.png)

### 5.样本数据 sample_data

#### 可视化前方的毫米波雷达传感器

通过如下代码，获取sample的data数据

```python
print(my_sample['data'])
```

![image-20220714154104732](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714154104732.png)

(1)获取具体sample中的具体传感器的信息

```python
sensor_radar = 'RADAR_FRONT'  #这里选择的传感器为前方的毫米波雷达传感器
radar_front_data = nusc.get('sample_data',my_sample['data'][sensor_radar])
print(radar_front_data)
```

(2)通过传感器信息的token值进行可视化

```python
nusc.render_sample_data(radar_front_data['token'])
```

![image-20220714154648984](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714154648984.png)

#### 可视化前方的相机

```python
# 可视化前方的相机
sensor_CAM_FRONT = 'CAM_FRONT'  #这里选择的传感器为前方的毫米波雷达传感器
CAM_FRONT_data = nusc.get('sample_data', my_sample['data'][sensor_CAM_FRONT])
print(CAM_FRONT_data)
nusc.render_sample_data(CAM_FRONT_data['token'])
```

![image-20220714155411802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714155411802.png)

#### 可视化顶部激光雷达

```python
# 可视化顶部激光雷达
sensor_LIDAR_TOP = 'LIDAR_TOP'  #这里选择的传感器为前方的毫米波雷达传感器
LIDAR_TOP_data = nusc.get('sample_data', my_sample['data'][sensor_LIDAR_TOP])
print(LIDAR_TOP_data)
nusc.render_sample_data(LIDAR_TOP_data['token'])
```

![image-20220714155708178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714155708178.png)

### 6.样本标注 sample_annotation

在sample_data中已经展示了传感器采集到的信息，这一部分将展示样本标注的信息，方法与之前是类似的。

(1)获取sample的标注数据，然后输出相关信息

```python
my_annotation_token = my_sample['anns'][18]
my_annotation_metadata = nusc.get('sample_annotation',my_annotation_token)
my_annotation_metadata
```

(2)通过sample标注信息的token值，进行可视化

```python
# 获取特定sample的标注信息
my_annotation_token = my_sample['anns'][18]
my_annotation_metadata = nusc.get('sample_annotation', my_annotation_token)
print(my_annotation_metadata)
# 可视化
nusc.render_annotation(my_annotation_metadata['token'])
time.sleep(3)
```

![image-20220714160510718](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714160510718.png)

### 7.实例 instance

通过nusc.instance[0]获取具体的实例，instance表示某个实例对象，例如一辆汽车

```python
# 获取某个实例对象，并输出其信息
my_instance = nusc.instance[5]
print(my_instance)
# 通过实例的token值，进行可视化
instance_token = my_instance['token']
nusc.render_instance(instance_token)
time.sleep(3)
```

![image-20220714161126558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714161126558.png)

### 8.类别categories

使用如下代码展示数据集中所有的种类

```python
nusc.list_categories()
```

![image-20220714161427099](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714161427099.png)

### 9.属性attributes

通过如下代码展示数据集的具体属性

```python
nusc.list_attributes()
```

![image-20220714161723151](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714161723151.png)

### 10.可视化 visibility

```python
# 选取当前sample标注信息中的一个token值
anntoken = my_sample['anns'][9]
nusc.render_annotation(anntoken)
time.sleep(3)
```

![image-20220714163002640](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714163002640.png)

### 11.传感器sensor

通过下面代码展示数据集中传感器信息

```python
print(nusc.sensor)
```

![image-20220714163421019](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714163421019.png)

### 12.校准传感器 calibrated_sensor

通过下面代码获取传感器的校准信息

```python
# 展示传感器的校准信息
sensor_token = nusc.calibrated_sensor[0]
print(sensor_token)
```

![image-20220714163600190](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714163600190.png)

### 13.车辆姿态 ego_pose

通过下面代码获取车辆姿态信息

```python
# 车辆姿态ego_pose信息
print(nusc.ego_pose[0])
```

![image-20220714163858730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714163858730.png)

### 14.日志 log

通过下面代码获取日志信息

```python
# 日志信息
print(nusc.log[0])
```

![image-20220714164010632](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714164010632.png)

### 15.地图 map

```python
# 地图信息
print(nusc.map[0])
```

![image-20220714164127302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220714164127302.png)
