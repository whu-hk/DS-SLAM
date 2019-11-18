# **DS-SLAM**

DS-SLAM is a complete robust semantic SLAM system, which could reduce the influence of dynamic objects on pose estimation, such as walking people and other moving robots. Meanwhile, DS-SLAM could also provide semantic presentation of the octo-tree map．DS-SLAM is a optimized SLAM system based on the famous ORB-SLAM2 (from https://github.com/raulmur/ORB_SLAM2 and https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map, thanks for Raul's, Gao Xiang's and SegNet's great work!). It not only includes tracking, local mapping, loop closing threads, but also contains semantic segmentation, dense mapping threads. Currently, the system is integrated with Robot Operating System (ROS). In this Open Source project, we provide one example to run DS_SLAM in the TUM dataset with RGB-D sensors. The example of real-time implementation of DS-SLAM and hardware/software configurations would be coming soon.

As described in **DS-SLAM: A Semantic Visual SLAM towards Dynamic Environments **Chao Yu, Zuxin Liu, Xinjun Liu, Fugui Xie, Yi Yang, Qi Wei, Fei  Qiao, Published in the Proceedings of  the 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS 2018) 

Moreover, to download the pre-print version, you could be directed to [https://arxiv.org/abs/1809.08379v1].

DS-SLAM is developed by the joint research project of iVip Lab @ EE, THU (https://ivip-tsinghua.github.io/iViP-Homepage/) and Advanced Mechanism and Roboticized Equipment Lab.

If you have any questions or use DS_SLAM for commercial purposes, please contact: qiaofei@tsinghua.edu.cn

# 1. License

DS-SLAM is released under a  [GPLv3 license](https://github.com/zoeyuchao/DS-SLAM/blob/master/LICENSE).

DS-SLAM allows personal and research use only. For a commercial license please contact: qiaofei@tsinghua.edu.cn

If you use DS-SLAM in an academic work, please cite their publications as below:

Chao Yu, Zuxin Liu, Xinjun Liu, Fugui Xie, Yi Yang, Qi Wei, Fei Qiao "DS-SLAM: A Semantic Visual SLAM towards Dynamic Environments." Published in the Proceedings of the 2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS 2018). 

# 2. Prerequisites

We have tested the library in Ubuntu 14.04 and 16.04, but it should be easy to compile in other platforms. The experiment is performed on a computer with Intel i7 CPU, P4000 GPU, and 32GB memory.

### ORB_SLAM2 Prereguisites

DS-SLAM is a optimized SLAM system based on the famous ORB-SLAM2. In order to run DS_SLAM, you have to install environment needed by ORB_SLAM2(the section of 2. Prereguisites). We suggest that the path of the folder of Thirdparty and Vocabulary(provided at <https://pan.baidu.com/s/1-zWXlzOn-X0tjoEF9XI6qA>, extract code: 8t7x) to be DS-SLAM. Instructions can be found at: https://github.com/raulmur/ORB_SLAM2.

### ROS

We provide one example to process the TUM dataset as RGB-D image. A version Hydro or newer is needed. You should create a ROS catkin workspace(in our case, catkin_ws).

### SegNet

We adopt SegNet to provide pixel-wise semantic segmentation based on caffe in real-time. Download and decompress DS_SLAM library (provided at https://github.com/zoeyuchao/DS-SLAM) in catkin_ws/src. Then you can find the Example folder in the folder of DS-SLAM. We suggest the installation path of caffe-segnet to be /Examples/ROS/ORB_SLAM2_PointMap_SegNetM. 

The root of Download and install instructions can be found at: https://github.com/TimoSaemann/caffe-segnet-cudnn5.
caffe的安装过程需要OpenCV,与ROS自带OpenCV的兼容编译可以参考博客https://blog.csdn.net/abc039510/article/details/103032120

### OctoMap and RVIZ

稠密八叉树地图支持包安装：https://blog.csdn.net/weixin_39123145/article/details/82219968
额外操作：
1.打开头文件开关#define COLOR_OCTOMAP_SERVER，文件路径（/home/xxx/catkin_ws/src/octomap_mapping/octomap_server/include/octomap_server/octomapServer.h）
2.彩色八叉地图参数设置：<param name="colored_map" type="bool" value="True" />，文件路径
(/home/whu-hk/catkin_ws/src/DS-SLAM/Examples/ROS/ORB_SLAM2_PointMap_SegNetM/launch/Octomap.launch)

# 3. Building DS_SLAM library and the example

We provide a script DS_SLAM_BUILD.sh to build the third party libraries and DS-SLAM. Please make sure you have installed all previous required dependencies. Execute:

```c++
cd DS-SLAM
chmod +x DS_SLAM_BUILD.sh
./DS_SLAM_BUILD.sh
```
首先编译第三方库和ORB词典；第二步编译caffe，参考第二节；第三步编译SegNet包，实际为caffe-segnet-cudnn5/Examples中SegNet_with_C++的改写；第四步编译改写的ORB-SLAM2；最后编译出可执行文件TUM

# 4. TUM example

1. Add the path including Examples/ROS/ORB_SLAM2_PointMap_SegNetM to the ROS_PACKAGE_PATH            environment variable. Open .bashrc file and add at the end the following line. Replace PATH by the folder where you cloned ORB_SLAM2:

```
export  ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH/DS-SLAM/Examples/ROS/ORB_SLAM2_PointMap_SegNetM
```
或者将/DS_SLAM放置在/catkin_ws/src/下，执行rospack profile命令检查是否ROS能否找到对应包。

2. Download a sequence from http://vision.in.tum.de/data/datasets/rgbd-dataset/download and unzip it. We suggest you download rgbd_dataset_freiburg3_walking_xyz.
提供百度网盘下载链接: https://pan.baidu.com/s/1W5jwhrdEPmZSaQ2rtm2Ahw 提取码: cdqs 
原始的TUM RGB-D数据集额外使用tools实现几个功能，比如depth和rgb的时间戳对齐，轨迹精度评估及可视化，在网盘分享中均有。

3. We provide DS_SLAM_TUM.launch script to run TUM example. Change PATH_TO_SEQUENCE and  PATH_TO_SEQUENCE/associate.txt in the DS_SLAM_TUM.launch to the sequence directory that you download before, then  execute the following command in a new terminal. Execute:

```
cd DS-SLAM
roslaunch DS_SLAM_TUM.launch 
```
修改各个参数的实际路径，其包含的octo lanuch文件修改已在前文中提及。

#  5. Something about Folders

The function of folder in the catkin_ws/src/ORB_SLAM2_PointMap_SegNetM/Examples/ROS/ORB_SLAM2_PointMap_SegNetM.

1. segmentation: the section of segmentation including source code, header file and dynamic link library created by Cmake.
2. launch: used for showing octomap.
3. prototxts and tools: containing some parameters associated with caffe net relating to the semantic segmentation thread of DS_SLAM. There is the folder of models(provided at https://pan.baidu.com/s/1gkI7nAvijF5Fjj0DTR0rEg extract code: fpa3), please download and place the folder in the same path with the folder of prototxts and tools.

# 6. 本fork版本暂时不支持ORB词典的bin格式
需要在ORB的src/System.cc中删除二进制词典读取函数



