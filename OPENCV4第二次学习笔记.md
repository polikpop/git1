# OPENCV4





## 1. 认识Mat

### 1.1Mat类的基本概念

 		Mat类是类似于int, double的一种数据类型， 以矩阵的形式存储数据，可以有很多维，不一定是二维。



Mat类的结构如下：

<img src="/home/xushuo/图片/BA18F747E4D77E257DC74FBBD4F35D69.png" alt="BA18F747E4D77E257DC74FBBD4F35D69" style="zoom:50%;" />





Mat类存储的数据有好几种类型，但是因为不同平台对于int，float规定可能有变，所以做出如下对于数据类型的统一规定：

![02D2D39B4D28252B8E22D9F780F72A53](/home/xushuo/图片/02D2D39B4D28252B8E22D9F780F72A53.png)



<img src="/home/xushuo/图片/52EB6A91BE19269ECA4500BF8F28E0AA.png" alt="52EB6A91BE19269ECA4500BF8F28E0AA" style="zoom: 33%;" />

### 1.2 Mat类的创建与赋值



* 利用矩阵宽，高和类型参数创建Mat对象

  ```c++
  cv::Mat::Mat (int rows, int cols, int type)
  ```

  其中，前两个参数代表行数和列数，第三个代表**数据类型**，如：**CV_8UC1**,重要的是，C1代表这是一个单通道矩阵，我们可以在上面一小节中的数据类型后加**C(n)**表示这个对象有几个通道。

* 利用Size结构创建

  ```c++
  cv::Mat::Mat (Size size, int type)
      
  //如：Mat b(Size(3,3), CV_8UC1) ;   
  ```

* 利用已有的Mat对象创建

  ```c++
  //假如我们已经有了一个 a 矩阵
  
  //接着创建c 
  Mat c(a, Range(2,5), Range(2,5));
  
  //注意2-5不包括2，5 且用这种方法创建数据时a中必须含有数据
  ```



* 创建时赋值 ： 直接在type参数后面加Scalar参数，有几个通道就给几个数。

* 利用类方法赋值：用类方法创建特殊矩阵，如对角矩阵

* 用枚举法创建：上面两种都不能对每一个像素点分别赋值，所以我们可以使用枚举法：

  ```c++
  cv::Mat a = (cv::Mat_<int>(3,3)<<1, 2, 3, 4, 5, 6, 7, 8, 9);
  //对一个3*3的矩阵分别赋值，注意尖括号里面填的是double等数据类型，不是CV_8U
  ```

  ---

  

  ### 1.3 Mat 读取

* Mat类在数据中的存储形式如下：



<img src="/home/xushuo/图片/0EF4EA181E244294F17F0BF20884326E.png" alt="0EF4EA181E244294F17F0BF20884326E" style="zoom:50%;" />



* Mat类对象的常用属性（可以用对象直接调用）

<img src="/home/xushuo/图片/D6505EA21DC268A71E13DE3E437D9683.png" alt="D6505EA21DC268A71E13DE3E437D9683" style="zoom:67%;" />



* Mat元素的读取：

  使用**at**方法读取Mat矩阵元素， **at (int row, int col)**

  ```c++
  //单通道
  int value = (int)a.at<unchar>(0,0)
  
      
  //多通道，因为多通道元素的每个元素有多个值，所以不能用int存， 所幸OPENCV已经帮我们定义了一些类型
  cv::Vec3b vc3 = b.at<cv::Vec3b>(0,0);
  //Vec3b代表一个元素有三个通道 每个值是unchar类型
  int first = (int)vc3.val[0];  //读取第一个通道的值
  ```

  也可以使用统一转化的方法，这样就不需要知道对象中元素的类型。

  补充：

  ---

  ### 1.4 Mat的运算

* 四则运算

  

  * 符号运算

    这种方法我们可以直接使用 **+  -  *  /** 进行运算。做运算时两个Mat内的数据类型要一致。

  * 两个矩阵相乘

    可以普通矩阵乘法，满足一般规律。

    可以点积，要求两个矩阵中元素个数相同，维度则不做要求。

    也可以对应位元素进行乘积，矩阵维度必须一样。

  * 也可以利用OPENCV自带函数。

  ---

  

  

  

  ## 2.  图像的加载与保存

  

  ### 2.1  图像读取 

- **imread**

  - 第一个参数为**filename** 写文件名称及其路经
  - 第二个参数为**flags**,为读取图像形式的方法，默认为**彩色读取**

  ###  2.2 图像显示

- **namedWindow**
  - 第一个参数为winname，可以设置窗口的名称。
  - 第二个参数为flags，设置窗口属性。

- **imshow**

  - 第一个参数为winname, 即上面函数的第一个参数
  - 第二个参数为mat，即要显示的图像对象

  ###  2.3  图像保存

- **imwrite**
  - 第一个参数为filename, 为保存图像的地址和格式。
  - 第二个参数为img, 为要保存的图像对象。
  - 第三个参数为params, 对保存图像的属性处理，如要不要压缩。



---





## 3.  视频加载与摄像头的使用



### 3.1  视频加载与摄像头调用

- **VideoCapture**

  - 第一个参数为filename, 为读取视频的名称。若调用摄像头为int类的参数, 为摄像头在电脑内的

    id。

  - 第二个参数为apiPreference, 为读取数据时的要设置的属性。一般不写。

  **P.S.**  VideoCapture是一个类， 他允许用户先定义一个空的对象， 后续用open()函数再设置参数。

  ​		可以用isopened()来判断是否读取成功。也可以用get()函数来获取属性，如下表：

  

  

  

### 3.2  视频保存 

- **VideoWriter** 

  - 第一个参数为filename, 为保存视频的地址和文件名。
  - 第二个参数为fourcc, 编解码器参数（大概），一般会选一些比较默认的格式。
  - 第三个参数为fps，为保存视频的帧率。
  - 第四个参数为framSize，为保存视频的尺寸，要和视频大小一样，不能随便填。
  - 第五个参数为isColor， 决定是否保存为彩色视频， 默认为true。

  

  ---

  



## 4. 图像颜色空间变换



### 4.1  图像颜色空间介绍

- **RGB模型**

为什么0-1，因为有可能出现小数点数据，所以可以把8U(0-255) 变为 float(0-1) 和 double(0-1),

<img src="/home/xushuo/图片/FD182F0952EA3377B8E464D0FD90113C.png" alt="FD182F0952EA3377B8E464D0FD90113C" style="zoom: 25%;" />

- **图像数据类型的相互转换（不是色彩空间）**

  - convertTo()

    - 第一个参数为m, 指输出的图像。
    - 第二个参数为rtype,为转换后的数据类型。
    - 第三个参数为alpha,为缩放系数。
    - 第四个参数为beta,为平移系数。

    ```c++
    //如：
    a.convertTo(b, CV_32F, 1 / 255.0, 0)
    //将a转换为b，从0-255变成0-1，类型从uchar变为float.    
    ```

    

- **HSV颜色模型**

<img src="/home/xushuo/图片/4DA24ADF4EA724D53FF4B1A89B8A117D.png" alt="4DA24ADF4EA724D53FF4B1A89B8A117D" style="zoom:25%;" />





- **GRAY颜色模型**

RGB可以推出GRAY，而反过来不能。



<img src="/home/xushuo/图片/D43CF8CA670F81129CEF9F03EE54B95D.png" alt="D43CF8CA670F81129CEF9F03EE54B95D" style="zoom:25%;" />



### 4.2  颜色转换



- **cvtColor()**
  - 第一个参数为src,  为待转换的原始图像。
  - dst,  转换后的目标图像。
  - code,  要从什么空间转到什么空间。
  - dstCn,  目标图像中的通道数， 如果参数为零，则从src和代码中自动导出通道数。

bgr 到 hsv 如果不先把0-255转化到0-1，结果会不一样。

bgr 到 gray 和 rgb 到 gray , 因为公式，效果会不一样。



---





## 5.  多通道的分离与合并



### 5.1  多通道分离

- **split()**
  - m, 待分离的图像。
  - mv, 分离后的单通道图像，为**vector向量**形式。也可以输出一个**Mat数组**。

### 5.2  多通道合并

- **merge()**
  - **mv**, 写**vector**,代表要合并的单通道，若写**数组**，则还要再加一个参数，声明数组的维度。
  - **dst**, 合并后的输出图像，通道数等于输入图像的总和。

---





## 6.  图像像素比较

- **min()**
  - src1, 第一个图像。通道数是任意的。
  - src2, 第二个图像。形式必须和第一个图像一致，所以要预处理。
  - dst, 输出的图像。

- **max()**
  - 同min()函数。

**它们可以做艺术字！**

- **minMaxLoc()**

  - src, 输入的**单通道**矩阵。
  - minVal, 指向最小值的指针， 如果不需要则使用NULL。
  - maxVal,  同上。
  - minLoc,  指向最小位置的指针，如不需要则使用NULL。类型为**Point**
  - maxLoc,  同上。
  - mask, 确定要寻找最大最小的范围。

  **这个函数找最小值只能找一个**。

  

---



## 7.  图像像素逻辑操作



### 7.1 两个像素逻辑运算规则：

**注意运算时因为像素是0-255，所以要转换为八位二进制数，然后再对每一位进行比较运算。**

**因为是八位比较，所以和白色做and运算出来还是它本身，八位和1取and转换回来是自己。**



<img src="/home/xushuo/图片/C755DA855E651E4529B87C12E8EC7711.png" alt="C755DA855E651E4529B87C12E8EC7711" style="zoom:33%;" />



### 7.2  逻辑运算函数

- **bitwise** + **下划线** + **and / not / or / xor /**  
  - src1, 输入的第一个图像。
  - src2, 输入的第二个图像。not无第二个参数。
  - dst, 输出的图像。
  - mask, 掩码矩阵

---



## 8.  图像二值化



<img src="/home/xushuo/图片/9CCF384B2F96F237D4257D9689ACFE5D.png" alt="9CCF384B2F96F237D4257D9689ACFE5D" style="zoom: 33%;" />





通过阈值，把图像进行滤波，可以把不想要的部分去掉。



- **threshold()**
  - src, 待二值化图像。必须CV_8U和CV_32F。
  - dst, 二值化后的图像。格式与src相同。
  - thresh, 二值化的阈值。如果用算法解阈值则没用。
  - maxval, 二值化过程的最大值。不一定起效果，如果用不到随便写。
  - type, 二值化的方法。





**但是，threshold法是固定阈值，像lena文字它就分不出来，所以我们可以把图像分割成许多小图片再二值化，于是出现了自适应阈值。**





- **adaptiveThreshold()**
  - src
  - dst
  - maxValue
  - adaptiveMethod,确定阈值的方法，只有均值法和高斯法。
  - thresholdType, 和上一个函数的type相同，但区别于adaptiveMethod。
  - blockSize, 分割小图片的大小（猜的）。
  - C, 减去常数，对整张图片的像素操作。



**自适应只能对灰度图进行处理。**

---



## 9. LUT查找表



**原理：**让像素按照用户规定进行， 如大于一百变255，小于一百变零，这种方法比较智能，也可以实现第七节中的单一二值化方法。。。



- **LUT()**

  - src, 输入的图像，类型必须是CV_8U。
  - lut, 上面src中256个值的对应映射表，可以单通道，也可以与src通道数相同。变量必须为Mat类型。
  - dst, 输出的图像矩阵， 尺寸和src相同， 数据类型和lut相同（因为输出的图像应该是变换后的）。

  

---



## 10.  图像尺寸变换



- 图像插值原理

   简单可以理解为用邻近的像素点来估计值未知的像素点的像素值。



<img src="/home/xushuo/图片/2F041D4099A181B2876214981C9FA2BF.png" alt="2F041D4099A181B2876214981C9FA2BF" style="zoom:25%;" />

- 图像缩放 -- **resize()**
  - src, 输入图像。
  - dsize, 输出图像的尺寸。
  - fx
  - fy, 第三，四个参数本质上和dsize一样，自己选一种方式就行。
  - interpolation, 插值方法。
- 图像翻转--**flip()**
  - src,输入图像。
  - dst,输出图像。
  - flipCode,翻转方法，小于0绕两个轴，等于0绕x轴，大于0绕y轴。

- 图像拼接--hconcat()  横向，  vconcat()  纵向
  - src1
  - src2
  - dst, 拼接后的图像。

---





## 11.  仿射变换



介绍：大概思路：已知三点 ->  算仿射矩阵->  将图像变换

<img src="/home/xushuo/图片/CEA8D39C64064535A2E6A6DBF9DD48E3.png" alt="CEA8D39C64064535A2E6A6DBF9DD48E3" style="zoom:25%;" />

- 仿射变换函数--**warpAffine()**
  - src, 输入图像。
  - dst, 输出的图像。与src数据相同，但是尺寸与dsize相同。
  - M：2x3的仿射矩阵。
  - dsize:输出图像的尺寸。
  - flags:插值方法。
  - boderMode:像素边界外推方式。
  - boderValue:填充边界所用的数值。类型为Scalar。
  - 扩充方法：



<img src="/home/xushuo/图片/2F4FE7DEC0FE8C2E7039172487771723.png" alt="2F4FE7DEC0FE8C2E7039172487771723" style="zoom: 33%;" />





- 图像旋转函数



![FA5266F4B3B6E94811E94255B82C5E12](/home/xushuo/图片/FA5266F4B3B6E94811E94255B82C5E12.png)





- 三点对应求仿射--**getAffineTransform()**
  - src[], **Point2f**类型的数组，为变换前的三个点。
  - dst[], 变换后的三个点，**要与src一一对应**。

---



## 12.  图像透视变换



- 介绍

<img src="/home/xushuo/图片/B4857807AA150D1A292901ADFF997FEB.png" alt="B4857807AA150D1A292901ADFF997FEB" style="zoom:33%;" />



- **getPerspectiveTransform()**
  - src, 原来的四个点。
  - dst,变换的四个点。
  - solveMethod, 计算变换矩阵的方法。

- 透视变换函数 





![02B32ADF6532EA6D88B236929CD90048](/home/xushuo/图片/02B32ADF6532EA6D88B236929CD90048.png)



---



## 13.  图像中绘制基础图形



- 画直线 -- **line()**
  - img,  要画线的图像。
  - pt1,  起点。
  - pt2, 终点。
  - color, 颜色 用Scalar来设置
  - thickness,  线宽，没啥事默认就行。
  - lineType,  线型，有FILLED,  LINE_4, LINE_8, LINE_AA。
  - shift, 偏移量（大概）
  
- 画圆 -- **circle()**
  - img, 图像
  - center, 中心， 
  - radius, 半径
  - 后面的参数和直线一样，要画实心圆的话线宽要写负数。不止是圆，别的图形画实心都是此方法。

- 椭圆 -- **ellipse()**

  - img
  - center,中心点
  - axes,  椭圆主轴大小的一半，为Size类型，包括长短轴。
  - angle, 整个椭圆旋转的角度。
  - startAngle, 画椭圆弧时的起始角度，要和上面一个参数区别开。
  - endAngle,终止角度。

  **P.S.**画点都是Point类型

- 矩形  --  **rectangle()**

  - img
  - pt1, 左上角顶点。
  - pt2, 右下角顶点。
  - 其他和直线一样

- 多边形 --  **fillPoly()**

  - img
  - pts, 二重数组， 第一层为绘制多边形的个数，第二层为每个多边形的顶点
  - npts, 是一个数组，里面是每个多边形的顶点个数，要对应
  - ncontours,  绘制多边形的个数。
  - 其余的参数和line一样， 最后一参offset为偏移量（不知意思）

- 绘制文字 -- **putText()**

  - img
  - text , 文字内容
  - org, 文字左下角的像素坐标
  - fontFace,  字体类型
  - fontScale,  字体大小
  - 后面和直线基本一样，最后一参为bottomLeftOrigin, 图像原点的位置，默认左上，改为true后变左下。

  ---





## 14.  ROI区域截取



- 图像截取 --  **Range()**

  - satrt, 起始，包括这个数
  - end, 结束， 不包括这个数

- **Rect_()**
  - _Tp,数据类型（不懂）
  - _x, 左上角x坐标
  - _y，左上角y坐标
  - _width，截取的宽
  - _height，截取的高

- 深拷贝和浅拷贝

  浅拷贝，只复制矩阵头， 深拷贝，全部复制， 但是费资源。

- 深拷贝函数 -- copyTo()
  - src
  - dst
  - mask
  - 注意这个函数有两个，一个是cv命名空间下，一个是Mat类中的函数。要和原来的对比要深拷贝。

---



## 15.  高斯图像金字塔



**原理：**对图像下采样组成序列。图像识别时克服物体尺寸变化的问题。

- 下采样函数 --  **pyrDown()**
  - src
  - dst,下采样输出，尺寸可以指定，但是数据类型和通道数与src相同。
  - dstsize, 输出图像尺寸，可以缺省。
  - borderType, 边界外推方法。

​			

---



## 16.  拉普拉斯图像金字塔



先求下一层高斯图像，再求上采样，再用原图减它，得到该层拉普拉斯图像。

- 上采样函数 --  **pyrUp()**

  参数和下采样函数一样。

  重点是如何手写一个拉普拉斯金字塔

---



## 17.  界面设计



### 17.1  创建滑动条

- **createTrackbar()**
  - trackbarname, 滑动条名称
  - winname, 窗口名称
  - value, 滑块初始位置，是一个指针
  - count, 滑块最大取值
  - onChange, 回调函数
  - userdata, 可选参数





### 17.2  鼠标事件响应

- **setMouseCallback()**
  - winname
  - onMouse , 鼠标响应回调函数
  - userdata



## 18. 图像直方图绘制



- 统计像素直方图 -- **calcHist()**
  - images,  待统计的图像组成的数组。（可以不止一张）
  - nimages, 输入图像的数量，和上面一个参数相关。
  - channels, 通道索引数组， 是双重数组，里面是每一个图像要统计的通道数。
  - mask, 掩码矩阵
  - hist，统计结果
  - dims， 结果直方图的维度。
  - histSize, 存放每个维度直方图的数组的尺寸。
  - ranges, 每个图像通道中灰度值的取值范围
  - uniform, 是否是均匀直方图，有默认参数
  - accumulate，是否累计起每个通道统计直方图。

**归一化方法画直方图应该是我们要使用的方法，比较好！详见ipad代码，后期可能会贴上来。**



## 19. 直方图均衡化和匹配

过明过暗都会导致图像纹理消失，因此我们可以使用图像直方图均衡化来处理图像。

- **像素距离拉伸**

  **。。。总结下再写，ddl为本周末**

- 直方图均衡化 --  **equalizeHist()**

  - src, 需要均衡化的图像， 类型需要为CV_8UC1。
  - dst，类型和src一样

  

- 直方图匹配

  把像素值的分布形式变成自己想要的。 累积概率要理解，**更接近映射就更合理**。

  可以总结为以下三个步骤：

  		1. 构建差矩阵
    		2. 寻找每一行的最小值，生成LUT
      		3. 通过LUT映射进行直方图匹配。

  **函数实现可以手写， ddl为本周末**

---





## 20.  模板匹配



**原理：**全局移动模板寻找系数最佳的位置。

- 模板匹配函数 -- **matchTemplate()**
  - image, 待匹配的图像，类型为CV_8U或CV_32F
  - templ, 模板图像，尺寸要小于image。
  - result,系数矩阵，类型为CV_32F。单通道，**能进minmaxval**
  - method, 匹配方法。
  - mask, 掩码。

**这个也最好自己实现，自己截一个图。**

---



## 21. 图像卷积

步骤见PPT：为什么要旋转？







- 卷积函数 -- **filter2D()**
  - src, 输入图像
  - dst, 输出图像，数据类型可能和输入不同，其他的相同。
  - ddepth, 输出图像的数据类型。
  - kernel, 卷积核， 类型为CV_32FC1
  - anchor, 内核基准点，默认为中心。写Point(-1, -1)。
  - delta，偏移量
  - borderType,像素外推法。
  - 还需要人为旋转180。

**具体可以看看代码，主要理解原理。**

---



## 22.  图像噪声的产生



- 产生椒盐噪声
  - 位置，种类，对rand取余数确定位置，除以二取余确定黑或白色。
  - 改像素值
  - **cvflann::rand_double()**
    - high,随机可能的最大值
    - low, 随机可能的最小值
  - **cvflann::rand_int()**
    - 参数和上面一个函数一样

- 产生高斯噪声

  - 创建一个大小与原图一样的Mat, 在里面产生符合高斯分布的随机数，再将其和原矩阵相加得到图像。
  - **fill()**
    - mat, 存放随机数的矩阵
    - distType, 随机数生成方法
    - a
    - b, a ， b都是分布的参数
    - sR, 预饱和，只有均匀用。

  **这个有机会还是实现一下，主要是要写出能产生椒盐噪声和高斯噪声的两个函数。**



---



## 23. 去噪



### 23.1线性滤波



- 均值滤波
  - 求区域内的和再求平均值，改中心。
  - **blur()**
    - src, 各种类型都可以
    - dst
    - ksize, 卷积核的尺寸，用**Size()**
    - anchor,锚点
    - borderType,

- 方框滤波，多了一个ddepth参数，确定输出图像类型，还有一个normalize,  是否要归一化，其它和均值滤波一样。



- 高斯滤波 -- 中间值大四周小
  - src
  - dst
  - ksize，必须奇数
  - sigmaX
  - sigmaY,标准偏差，默认写0
  - borderType





### 23.2  非线性滤波--中值滤波

滤波器内像素排序，选择中位数为替换结果，这个过程中没有线性计算。

**所以特别能打椒盐噪声。不太能打高斯噪声。**

- **medianBiur()**
  - src, 滤波器尺寸太大就要用CV_8U类型
  - dst
  - ksize, **因为要选取中位数，所以必须是大于1的整数**





### 23.3 可分离滤波

可以把滤波器分成行方向和列方向。加速运算。

- **sepFilter2D()**

  - src
  - dst
  - ddepth
  - kernelX, X方向滤波器
  - kernelY，Y方向滤波器，和上面有一个参数都输入行向量（大概）
  - anchor, 锚点
  - delta, 偏置
  - borderType, 像素外推的方法

  

---





## 24.  边缘检测

- 原理

  **对像素的变化求导，这样比较OK，后期理解后可以加注，有空可以看看算法解析。**

- 第一种 --  **Sobel()**

  - src
  - dst
  - ddepth
  - dx
  - dy, 这两个意思是要求几阶导。
  - ksize = 3,
  - scale = 1
  - delta = 0
  - borderType

- 第二种 -- **Scharr()**

  - 参数没有ksize罢了， 这个算子比上一个系数要大，缺点是边缘微弱也会被检测出来。

- 自定义算子 --  **getDerivKernels()**

  - kx, 行滤波器矩阵， 大小 **ksize x 1**

  - ky, 同上

  - dx

  - dy

  - ksize,  参数为 FILTER_SCHARR 或 1， 3， 5， 7

  - normalize

  - ktype,  滤波器系数类型， 默认CV_32F, 可设置为CV_64F

    

- 边缘检测可能得到负数，所以要用**convertScaleAbs(src, dst)**来取绝对值。

- 第三种 -- **Laplacian()**
  - 无方向性但是容易受噪声影响
  - src, 可以是灰度，也可以彩
  - dst
  - ddepth
  - ksize，正奇数
  - scale = 1
  - delta = 0
  - booserType

- **Canny()**
  - 去除异常点
  - 原理多看看(QAQ)
  - image,输入，可单可三
  - edges, 输出，但是为单通道
  - threshold1，阈值一
  - threshold2，阈值二
  - aSize，Sobel算子的直径，默认为3
  - L2gra， 算幅值，一般默认

---





## 25. 连通域分析

**理解两遍法分割连通域原理**

- 函数 -- **connectedComponents()**

  - image, 类型为CV_8UC1
  - labels， 输出图像
  - connectivity, 选择4邻域还是8邻域。
  - ltype，输出图像的数据类型。
  - 也可以使用 **connectedCompenentsWithStats()**   ,  多两个参数，记录不同连通域的信息。

   **这一课可以跟着代码走一遍。**

**可以做一些带连通的目标检测和分割，重点是要画矩形，**

-- -





## 26. 图像距离变换

理解欧氏距离，街区距离，棋盘距离。

- 距离变换函数 -- **distanceTransform()**
  - src 单通道，CV_8U
  - dst, CV_8U或CV_32F
  - distanceType, 计算距离的方法
  - maskSize,  距离变换掩码矩阵大小（不懂）
  - dstType, 输出图像的类型。

**这玩意有啥用？**



---





## 27. 图像形态学

### 腐蚀



- 去除微小物体，分开离得比较近的物体。
- 结构元素覆盖，**在新的图像下绘制0或1**  ，并行运算。
- 生成结构元素的函数 --  **getStructuringElement()**
  - shape, 结构元素的 种类
  - ksize， 大小
  - anchor, 锚点，结构元素几何中心
- 腐蚀函数 --  **erode()**
  - src, 通道任意
  - dst,输出
  - kernel, 结构元素
  - anchor, 结构元素中心
  - iterations, 腐蚀的次数
  - borderType
  - borderValue,外推法选定值时的大小。



### 膨胀

与腐蚀类似，结构元素可以用上面的生成。结构元素本质上还是一个矩阵，只不过部分为1，部分为0

- 膨胀函数 --  **dilate()**
  - src
  - dst
  - kernel
  - anchor
  - iterations
  - borderType
  - borderValue

上面这两个函数一般三个参数即可，即src，dst，kernel



### 形态学操作应用



- 开运算
  - 去除噪声，消除较小连通域，**保留较大连通域**。
  - **先腐蚀再膨胀，基本结构要相同**。
- 闭运算
  - 去除连通域中的小空洞， 连接连通域，平滑物体轮廓。
  - **先膨胀再腐蚀**
- 形态学梯度
  - 用膨胀的图像减腐蚀的图像
- 顶帽运算
  -  突出一些小区域
  - 先做开运算，再用原图像减去开运算。个人认为是开运算的逆
- 黑帽运算
  - 先做闭运算，图像被填充。
  - 再减去原图像，就可以留下原图中的暗区域。
- 击中击不中变换
  - 比腐蚀更加苛刻，即图像必须有和结构元素完美匹配的部分才叫击中，否则击不中。**可以认为是腐蚀的更厉害**   。

- 形态学操作函数 -- **morphologyEx()**
  - src
  - dst
  - op, 即操作类型
  - kernel
  - anchor
  - iterations, 操作次数
  - borderType
  - borderValue

**形态学常用于二值化图像**    ！！！



---





## 28. 图像细化



**原理：**图像细化就是将图像的线条从多像素宽度减少到单位像素宽度的过程，又称“骨架化”，一般对二值图像进行图像细化，要进行两次判断删除处理。

- 图像细化图像 --  **thinning()**
  - src, CV_8UC1，
  - dst
  - thinningType，细化算法的选择
  - 该函数存在cv::ximgproc::中
  - 实心圆会变成点，这种方法不适合实心的图形。

---





## 29. 轮廓



### 29.1 轮廓检测

- 轮廓就是一个闭合的交界处，描述一个轮廓用四个参数，分别为同层下一个轮廓，同层上一个轮廓，下一层第一个子轮廓，上层父轮廓。没有就写-1.

  **对二值化图像进行处理**。

- 轮廓检测函数 -- **findContours()**
  - image, 输入图像，CV_8U的单通道或二值化图像，一般为二值化图像
  - contours, 检测的轮廓，存放着每个轮廓的像素的坐标，是一个vector.
  - mode，轮廓检测的模式，即要哪些轮廓。
  - method，轮廓检测算法，要与上面的区分开。
  - offset，偏移量，在ROI区域中检测轮廓，又想知道它在整张图像的位置时要用到此参数。
- 绘制轮廓 --  **drawContours()**
  - image, 输入图像，可以是多张图像
  - contours, 要绘制的所有轮廓
  - contouridx, 绘制哪个轮廓，负数为绘制所有轮廓
  - color,Scalar三通道赋值
  - thickness
  - lineType
  - hierarchy,轮廓信息
  - maxLevel, 要绘制的层数，一般默认全绘制。
  - offset
  - 绘制所有层轮廓时，按回车绘制下一层轮廓。

**这段代码可以写写**



### 29.2  轮廓信息统计



- 轮廓面积  --  contourArea()
  - contour, 计算所需的轮廓像素点
  - oriented, 区域面积是否具有方向性，即是否有正负，一般为false
  - 图像面积会以double类型返回。
- 轮廓周长 --  arcLength
  - curve, 轮廓的2d像素点，可以不闭合
  - closed， 轮廓是否闭合，填true或false。即你是否要最后一个点到第一个点的距离。
  - 以double类型返回。





### 29.3  轮廓外接多边形



- 外接矩形 -- **boundingRect()**
  - array, 输入的灰度图或者2d点集，类型为vector<Point>或Mat

- 最小矩形 --  **minAreaRect()**
  - array,同上一个函数
  - 可以得到矩形的四个顶点和中心点， 分别用 **.points(Point2f[4] )** 和 .**center(Point2f)**  访问

- 外接多边形 --  **approxPolyDP()**
  - curve, 轮廓像素点，同上一节。
  - approxCurve, 逼近的结果，给出多边形顶点
  - eplision, 逼近的精度，即原始曲线和逼近曲线的最大距离
  - closed，是否闭合。
  - 圆形也可以用多边形拟合





## 30. 凸包检测



有的时候我们不是单单想把轮廓描出来，而是想把物体所在的区域圈出来，这时就需要凸包检测。

- 凸包检测函数 --  **convexHull()**
  - points, 输入的2d点集
  - hull,  输出凸包的顶点
  - clockwise,  输出点的顺序， 写true时为顺时针方向
  - returnPoints, 输出的东西， 为true时输出点的坐标，否则为点在输入点集中的索引。

## 31.  直线检测



因为我们可以把直线看作经过了很多点的图形，所以我们可以先转换坐标系，每一个点代表一条直线，每一条直线代表一个点，原空间中，此时交点含义为，有好多点都位于这条直线上。但是因为斜率可能不存在，所以我们采用霍夫变换，这样就能避免该问题



- 霍夫检测函数1 --  **HoughLines()**
  - images, 原图
  - lines, 检测结果
  - rho，长度单位
  - theta 单位
  - threshold，检测点数量的阈值
  - srn，多尺度，这两个不懂
  - stn
  - min_theta = 0,角度最大和最小，这两个不懂，一般默认。
  - max_theta = CV_PI

- 霍夫检测函数2 --  线段检测 --  **HoughLinesP()**
  - image， 单通道二值，最好是经过canny边缘检测后的，比较有意义。
  - lines，有四个参数，分别是两个端点的坐标
  - rho
  - theta
  - threshold
  - minLineLength, 检测直线的最小长度
  - minLineCap, 小的话斜率就小，不太懂



## 32. 点集拟合



允许点不都在图像上



- 点拟合直线函数 --  **fitLine()**
  - points, 待拟合的2d或3d点集
  - line，输出的直线参数， 2d输出Vec4f， 3d输出Vec6f
  - distType，计算距离的算法类型
  - param，距离算法中的某些常数参数，一般填默认的0，就会自动选择最佳值
  - reps，一般0.01，和下一个参数都为精度参数
  - aeps，一般0.01
- 三角形拟合函数 -- **minEnclosingTriangle()**
  - points
  - triangle, 三角形三个顶点
- 圆拟合函数  --  **minEnclosingCircle()**
  - points
  - center，圆心
  - radius，半径



## 33. 二维码检测

- 二维码定位函数--  ddetect
  - img,  待检测的灰度图像或者彩色图像
  - points，检测到二维码时得到的四个顶点坐标
  - 返回类型为bool，即是否检测到了
- 二维码读取函数 --  decode
  - img
  - points
  - straight_qrcode,  经过矫正和二值化的二维码
  - 返回string，即二维码信息
- detectAndDecode-- 既定位又读取
  - img
  - points
  - straight_qrcode
  - 返回信息



## 34 .  图像积分

有标准求和积分，平方求和积分，三角求和积分

- 积分函数 --  integral()
  - src, CV8U,CV32F,CV64F
  - sum, 标准求和， CV32S,CV32F,CV64F
  - sqsum，平方求和，CV32F,CV64F
  - tilted，倾斜求和，数据类型和sum相同
  - sdepth， sum和tilted的输出数据类型
  - sqdepth



## 35. 图像分割

待补充，比较麻烦





## 36.  角点检测

### 36.1  Harris角点检测

原理：  选取一个小方形区域，然后扫起来，按特定方式求和，变化大就是有角点。

- 检测Harris角点函数 --  **cornerHarris()**

  - src, CV8U, CV32F,单通道灰度图像
  - dst，存放Harris评价系数的R矩阵，CV32F的单通道图像
  - blockSize, 小方块的大小，一般2x2
  - ksize，Sobel算子的半径
  - k，R的权重系数,一般为0.02-0.04
  - borderType.

- 绘制角点函数 --  drawKeypoints()

  - image,输入图像
  - keypoints，要绘制的关键点
  - outimage，输出，一般和第一个参数一样
  - color,一般默认
  - flags,角点绘制类型，目前写默认，之后会介绍

  



### 36.2 Shi-Tomas角点检测

与Harris角点的检测方法相同，但是评价方法不同。

- Shi-Tomas角点检测函数 --  **goodFeaturesToTrack()**
  - image, 
  - corners,检测到的焦点输出。每一行是一个坐标
  - maxCorners,要寻找的焦点数目
  - qualityLevel，最佳值乘它等于角点检测阈值，与数目互相制衡。
  - minDistance,两角点最小欧氏距离
  - mask
  - blockSize，检测梯度矩阵的尺寸
  - useHarrisDetector，是否使用Harris角点
  - k， 若使用Harris，则为检测中的系数k

**检测角点时输出的是个Point函数，但是绘制角点时要求它是KeyPoint类型，所以要把x坐标和y坐标分别配给keyPoint对象**





## 37. 亚像素角点优化

通过这种方法，检测出的角点精度更高，也可以是小数的~

- 优化角点亚像素位置函数 --  cornerSubPix()
  - images, 输入图像 ，CV8U,CV32F,单通道灰度图。
  - corners, 角点坐标，既是输入也是输出
  - winSzie, 搜索窗口尺寸的一半，必须是整数，搜索窗口就是算梯度时的小窗口。
  - zeroZone, 搜索区域死区大小的一半，即不提取像素点的区域，(-1, -1)表示没有死区，一般我们也会选择默认。
  - criteria, 终止迭代的条件，一种是迭代次数，另一种是精度达到要求就停止迭代，我们一般会两种结合起来搞。



## 38.  ORB特征点提取

### 38.1  Feature2D类的了解

这个还要再多看，多写，背模板。这个类比较复杂，之后买书！







## 39.  特征点匹配

- 特征点匹配类 -- **DMatch()**
  - queryIdx, 查询子集合中的特征点索引
  - trainIdx, 训练子集合中的特征点索引
  - imgIdx, 训练子来自哪幅图，图片一多这个就比较重要了
  - distance，匹配成功的两个特征点的相似程度。
- 首先看一下一一匹配 -- **match()**
  - queryDescription，查询描述子集合
  - trainDescription，训练描述子集合
  - matches, 匹配结果，是一个向量，，里面都是DMatch
  - mask
- 一对多  --  knnMatch()
  - qD
  - tD
  - matches，二重向量
  - k， 要匹配几个
  - mask
  - compacResult，输出的结果中要不要把没匹配上的查询子也带上

之后还是要多了解为什么会出现半径法算匹配点

- 半径法 -- radiusMatch()
  - 只有第四个参数改成了距离
- 暴力匹配 -- BFMatcher()
  - normType, 距离比较方法，即用什么距离
  - crossCheck，是否交叉检测，默认
- 画出特征点匹配结果 --  drawMatches()
  - img1
  - keypoints1,因为需要位置信息，所以用关键点
  - img2
  - keypoints2，
  - matches1to2, 匹配结果，是DMatch向量
  - outImg,输出图像
  - 后面为颜色等参数，需要可以自己看看，一般默认

- 汉明距离优化
- RANSAC算法
  - 选取最佳规律
  - 计算单应矩阵函数 --  findHomography()
    - srcPoints,原始图像中特征点的**坐标**
    - dstPoints
    - method，计算单应矩阵的方法，写RANSAC
    - rRT，重投影的最大误差（即阈值范围）
    - mask，选RANSAC时输出满足条件的特征点索引
    - maxIters, RANSAC最大迭代次数
    - confidence，置信区间，0-1，越大越好

