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
  
## 实现方法
  棋盘是一块由黑白方块间隔组成的标定板，我们用它来作为相机标定的标定物（从真实世界映射到数字图像内的对象）。之所以我们用棋盘作为标定物是因为平面棋盘模式更容易处理（相对于复杂的三维物体），但与此同时，二维物体相对于三维物体会缺少一部分信息，于是我们会多次改变棋盘的方位来捕捉图像，以求获得更丰富的坐标信息。  

  下面将依次对刚体进行一系列变换，使之从世界坐标系进行仿射变换、投影透射，最终得到像素坐标系下的离散图像点，过程中会逐步引入各参数矩阵。
  
  标定图片需要使用标定板在不同位置、不同角度、不同姿态下拍摄，最少需要3张，以10~20张为宜。标定板需要是黑白相间的矩形构成的棋盘图，制作精度要求较高，如下图所示：  
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/棋盘.png)
  
  原图为：
  ![emmmm](https://github.com/Heured/PCV_Assignment_06/blob/master/imgToShow/原图.PNG)
  
  拍摄设备参数： 焦距3.5mm、光圈F/2.2
  
  代码如下:  
  ```python
  # -*- coding: utf-8 -*-
import cv2
import numpy as np
import glob

# 设置寻找亚像素角点的参数，采用的停止准则是最大循环次数30和最大误差容限0.001
criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 30, 0.001)

# 获取标定板角点的位置
objp = np.zeros((4*6, 3), np.float32)
objp[:, :2] = np.mgrid[0:6, 0:4].T.reshape(-1, 2)  # 将世界坐标系建在标定板上，所有点的Z坐标全部为0，所以只需要赋值x和y

obj_points = []  # 存储3D点
img_points = []  # 存储2D点

images = glob.glob("./data/*.jpg")
i = 0;
for fname in images:
    img = cv2.imread(fname)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    size = gray.shape[::-1]
    ret, corners = cv2.findChessboardCorners(gray, (6, 4), None)
    #print(corners)

    if ret:

        obj_points.append(objp)

        corners2 = cv2.cornerSubPix(gray, corners, (5, 5), (-1, -1), criteria)  # 在原角点的基础上寻找亚像素角点
        #print(corners2)
        if [corners2]:
            img_points.append(corners2)
        else:
            img_points.append(corners)

        cv2.drawChessboardCorners(img, (6, 4), corners, ret)  # 记住，OpenCV的绘制函数一般无返回值
        i += 1;
        cv2.imwrite('conimg'+str(i)+'.jpg', img)
        cv2.waitKey(4000)

print(len(img_points))
cv2.destroyAllWindows()

# 标定
ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(obj_points, img_points, size, None, None)

print("ret:", ret)
print("mtx:\n", mtx) # 内参数矩阵
print("dist:\n", dist)  # 畸变系数   distortion cofficients = (k_1,k_2,p_1,p_2,k_3)
print("rvecs:\n", rvecs)  # 旋转向量  # 外参数
print("tvecs:\n", tvecs ) # 平移向量  # 外参数

print("-----------------------------------------------------")

img = cv2.imread(images[2])
h, w = img.shape[:2]
newcameramtx, roi = cv2.getOptimalNewCameraMatrix(mtx,dist,(w,h),1,(w,h))#显示更大范围的图片（正常重映射之后会删掉一部分图像）
print(newcameramtx)
print("------------------使用undistort函数-------------------")
dst = cv2.undistort(img,mtx,dist,None,newcameramtx)
x, y, w, h = roi
dst1 = dst[y:y+h, x:x+w]
cv2.imwrite('calibresult3.jpg', dst1)
print("方法一:dst的大小为:", dst1.shape)
  ```
  
结果：  

  
  
