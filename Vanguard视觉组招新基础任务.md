# Vanguard视觉组招新基础任务

[TOC]

下列任务请使用C++或Python实现，优先选择C++

练习题

## 任务一

请你利用话题实时传输当前时间

## 任务二

请你利用服务实现坐标转换

如：存在两个平面直角坐标系，且知道这两个坐标系的相对位置，给定一个点的坐标，求解该点在另外一个坐标系的位置，要求数据输入：点的坐标，两坐标系的相对位置

## 任务三

利用opencv识别某张图片里的物品

示例：

```python
import rclpy                            # ROS2 Python接口库
from rclpy.node import Node             # ROS2 节点类

import cv2                              # OpenCV图像处理库
import numpy as np                      # Python数值计算库

lower_red = np.array([0, 90, 128])     # 红色的HSV阈值下限
upper_red = np.array([180, 255, 255])  # 红色的HSV阈值上限

def object_detect(image):
    hsv_img = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)      # 图像从BGR颜色模型转换为HSV模型
    mask_red = cv2.inRange(hsv_img, lower_red, upper_red) # 图像二值化

    contours, hierarchy = cv2.findContours(mask_red, cv2.RETR_LIST, cv2.CHAIN_APPROX_NONE) # 图像中轮廓检测

    for cnt in contours:                                  # 去除一些轮廓面积太小的噪声
        if cnt.shape[0] < 150:
            continue

        (x, y, w, h) = cv2.boundingRect(cnt)              # 得到苹果所在轮廓的左上角xy像素坐标及轮廓范围的宽和高
        cv2.drawContours(image, [cnt], -1, (0, 255, 0), 2)# 将苹果的轮廓勾勒出来
        cv2.circle(image, (int(x+w/2), int(y+h/2)), 5, (0, 255, 0), -1)# 将苹果的图像中心点画出来

    cv2.imshow("object", image)                           # 使用OpenCV显示处理后的图像效果
    cv2.waitKey(0)
    cv2.destroyAllWindows()

def main(args=None):                                      # ROS2节点主入口main函数
    rclpy.init(args=args)                                 # ROS2 Python接口初始化
    node = Node("node_object")                            # 创建ROS2节点对象并进行初始化
    node.get_logger().info("ROS2节点示例：检测图片中的苹果")

    image = cv2.imread('/home/yang/dev_ws/src/learning_node/learning_node/apple.jpg')  # 读取图像
    object_detect(image)                                   # 苹果检测
    rclpy.spin(node)                                       # 循环等待ROS2退出
    node.destroy_node()                                    # 销毁节点对象
    rclpy.shutdown()                                       # 关闭ROS2 Python接口
```

要求：

1. 你可以对示例进行修改来达成识别目的，但必须理解每行代码的意义
2. 尝试给出更好的识别算法

## 任务四

将电脑摄像头连接到虚拟机，编写两个节点，节点之间通过话题进行通讯，节点一为图像获取并发送出去，节点二为接收图像并进行识别

## 任务五

在任务四的基础上，增加一个服务通讯，尝试获取物品的坐标点，并思考：在实时获取物品的坐标上，话题和服务哪个更为合适？



## 要求

- 在自己的个人账户中建立一个仓库，仓库命名方式：`姓名_RecruitTask`  姓名为英文
  在仓库中，提交你的代码。可以新建两个文件夹，分别放入基础任务和考核任务。

- 代码内包含README.md，内容为详细记录的实现过程、遇到的困难及解决办法

