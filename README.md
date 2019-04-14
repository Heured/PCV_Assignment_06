# PCV_Assignment_06
chessboard Square
## 相机标定所得结果
  1. 相机内参矩阵A(dx,dy,r,u,v,f), 外参矩阵[R|T]、畸变系数[k1,k2,k3...,p1,p2,...]。 
  
  2.内参矩阵各元素：一个像素的物理尺寸dx和dy，焦距f，图像物理坐标的扭曲因子r，图像原点相对于光心成像点的纵横偏移量u和v(像素为单位)。
  
  3.外参矩阵：世界坐标系转换到相机坐标系的旋转R和平移T矩阵。
  
  畸变系数：包括相机的径向畸变参数k1，k2，k3，...，和相机的切向畸变参数p1，p2，...。
  
  4.![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理.png)
  
## 相机标定原理

