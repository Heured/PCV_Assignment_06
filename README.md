# PCV_Assignment_06
chessboard Square
## 相机标定所得结果
  1. 相机内参矩阵A(dx,dy,r,u,v,f), 外参矩阵[R|T]、畸变系数[k1,k2,k3...,p1,p2,...]。 
  
  2.内参矩阵各元素：一个像素的物理尺寸dx和dy，焦距f，图像物理坐标的扭曲因子r，图像原点相对于光心成像点的纵横偏移量u和v(像素为单位)。
  
  3.外参矩阵：世界坐标系转换到相机坐标系的旋转R和平移T矩阵。
  
  畸变系数：包括相机的径向畸变参数k1，k2，k3，...，和相机的切向畸变参数p1，p2，...。
  
  4.  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理.png)
  
  
## 相机标定原理
  1.四个坐标系：
    世界坐标系：用户定义的三维世界的坐标系，为了描述目标物在真实世界里的位置而被引入，单位为m。
    
    相机坐标系：在相机上建立的坐标系，为了从相机的角度描述物体位置而定义，作为沟通世界坐标系和图像/像素坐标系的中间一环。单位为m。
    
    图像坐标系：为了描述成像过程中物体从相机坐标系到图像坐标系的投影透射关系而引入，方便进一步得到像素坐标系下的坐标。 单位为m。
    
    像素坐标系(pixel coordinate system)：为了描述物体成像后的像点在数字图像上（相片）的坐标而引入，是我们真正从相机内读取到的信息所在的坐标系。单位为个（像素数目）。
    
  2.  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理2.png)
  
  3.从世界坐标系到相机坐标系3D->3D  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理3.png)
  
  4.  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理3_1.png)
  
  5. 其中，R为旋转矩阵，t为平移向量，因为假定在世界坐标系中物点所在平面过世界坐标系原点且与Zw轴垂直（也即棋盘平面与Xw-Yw平面重合，目的在于方便后续计算），所以zw=0，可直接转换成式1的形式。其中变换矩阵
  
  6.  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理3_2.png)
  
  7. 即为前文提到的外参矩阵，之所称之为外参矩阵可以理解为只与相机外部参数有关，且外参矩阵随刚体位置的变化而变化。
  
  8. 从相机坐标系到理想图像坐标系（不考虑畸变） 3D->2D  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理4.png)
  
  9.  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理5.png)
  
  
