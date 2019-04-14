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
  
  10. 这一过程进行了从三维坐标到二维坐标的转换，也即投影透视过程（用中心投影法将形体投射到投影面上，从而获得的一种较为接近视觉效果的单面投影图，也就是使我们人眼看到景物近大远小的一种成像方式）。
  
  11. 从理想图像坐标系到实际图像坐标系（考虑畸变）：  
  透镜的畸变主要分为径向畸变和切向畸变（还有薄透镜畸变等等，但都没有径向和切向畸变影响显著，所以我们在这里只考虑径向和切向畸变）。  
  径向畸变是由于透镜形状的制造工艺导致。且越向透镜边缘移动径向畸变越严重。实际情况中我们常用r=0处的泰勒级数展开的前几项来近似描述径向畸变。矫正径向畸变前后的坐标关系为：  
  xcorrected = x(1+k1r2+k2r4+k3r6)
  ycorrected = y(1+k1r2+k2r4+k3r6)
    
  由此可知对于径向畸变，我们有3个畸变参数需要求解。
  切向畸变是由于透镜和CMOS或者CCD的安装位置误差导致。切向畸变需要两个额外的畸变参数来描述，矫正前后的坐标关系为：  
  
  xcorrected = x + [ 2p1y + p2 (r2 + 2x2) ]
  ycorrected = y + [ 2p2x + p1 (r2 + 2y2) ]
  
  由此可知对于切向畸变，我们有2个畸变参数需要求解。  
  综上，我们一共需要5个畸变参数（k1、k2、k3、p1和p2 ）来描述透镜畸变。
  
  注意：  
  从实际图像坐标系到像素坐标系：  
  
  由于定义的像素坐标系原点与图像坐标系原点不重合，假设像素坐标系原点在图像坐标系下的坐标为（u0，v0），每个像素点在图像坐标系x轴、y轴方向的尺寸为：dx、dy，且像点在实际图像坐标系下的坐标为（xc，yc），于是可得到像点在像素坐标系下的坐标为：  
  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理6.png)
  
  化为齐次坐标表示形式可得：  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理7.png)
  
  若暂不考虑透镜畸变，则将式2与式5的转换矩阵相乘即为内参矩阵M：  
  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理8.png)
  
  之所以称之为内参矩阵可以理解为矩阵内各值只与相机内部参数有关，且不随物体位置变化而变化。 
  
  最后用一幅图来总结从世界坐标系到像素坐标系（不考虑畸变）的转换关系：  
  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理9.png)
  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原理10.png)
  
  
