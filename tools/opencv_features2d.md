# Topic: opencv_feature2d & opencv_xfeature2d

## list

* features2d为2D特征框架
  * 其内分为：
    * features2d_main 特征检测与描述
    * features2d_match 描述子匹配
    * features2d_draw 绘制关键点与匹配对
    * features2d_category 模板归类
    * features2d_hal 硬件加速等.
      * features2d_hal_interface接口

* opencv中的算法继承层次
  * class Algorithm
  * class Features2D : public virtual Algorithm // 2维图像特征提取和描述提取器抽象基类
    * detect() 输入图像，计算关键点
    * compute() 输入图像、关键点集，然后计算关键点的描述子，对于不存在描述子的关键点，将其移除. 有时有的关键点也会新增. 输出描述子
    * detectAndCompute() 输入图像，输出找到的关键点和计算的描述子
    * 其他属性操作
  * typedef FeatureDetector Features2D 特征检测器
  * typedef DescirptorExractor Features2D 关键点描述子提取器

  * class AffineFeature : public Feature2D 仿射不变检测器与提取器 @YM11
    * create() 输入你要使用的检测器/提取器 backend
  * typedef AffineFeature AffineFeatureDetector
  * typedef AffineFeature AffineDescriptorExtractor

  * class SIFT : public Feature2D 尺度不变特征变换算法@Lowe04
    * create() 输入要提取的特征数、Octave层数、对比度阈值、边缘阈值、sigma
  * typedef SIFT SiftFeatureDetector
  * typedef SIFT SiftDescriptorExtractor

  * class BRISK : public Feature2D BRISK关键点检测器和描述子提取器@LSC11
    * create() AGAST检测阈值分数、检测器Octaves、使用该尺度至模式以采样关键点邻域
    * create() 不同尺度半径列表、采样圆内采样点个数、短对作描述器形成、长对阈值作方向判断
    * create() AGAST检测分数阈值、检测器Octaves、半径清单、采样数清单、短对用于描述器形成、长对用于方向判断

  * class ORB : public Feature2D 方向BRIEF特征点检测器和描述符提取器 @RRKB11 先在金字塔中用FAST检测稳定关键点，利用FAST或Harris响应计算最强特征，找到其方向作一阶矩并用BRIEF计算描述符
    * create() 最大特征个数500、尺度因子1.2、金字塔层级8、边缘阈值31、金字塔所在层0、产生每一个朝向BRIEF描述器的点数-2/3/4、分数类型-切换harris或fast、用于朝向BRIEF描述子的块大小、快速阈值

  * class MSER : public Feature2D 最大稳定极值区域提取器 @nister2008linear, @forssen2007maimally 本算子有python示例.
    * create() delta、面积低阈值、面积高阈值、裁剪与其子区域有相似大小的区域、彩图中追踪减去有比此值小的mser?、进化步数、面积阈值触发重新初始化、忽略太小边缘、边缘模糊缝隙大小
    * detectRegions() 输入>= 3*3的8ucx图、产生一堆点集、产生boundingbox

  * class FastFeatureDetector : public Feature2D 包装使用FAST方法的特征提取
    * create() 阈值10、是否非极大值抑制、检测器类型
  * void FAST(image, keypoints, threshold, nonmaxSuppression);
  * void FAST(image, keypoints, threshold, nonmaxSuppression, 检测器类型);

  * class AgastFeatureDetector : public Feature2D 封装AGAST方法特征检测
    * create() 阈值=10、是否非极大值抑制、检测器类型
  * void AGAST(image, keypoints, threshold, nonmaxSuppression);
  * void AGAST(image, keypoints, threshold, nonmaxSuppression, 检测器类型)

  * class GFTTDetector : public Feature2D 包装使用goodFeaturesToTrack函数的特征检测
    * create() 最大角点、质量等级、最大分数、blockSize、是否使用harris检测器、k
    * create() 同上-4，梯度size、是否使用harris检测器、k

  * class SimpleBlobDetector : public Feature2D 从图中提取blob
    * create() 参数

  * class KAZE : public Feature2D KAZE关键点检测器和描述子提取器 @ABD12
    * create() 使能提取扩展128字节描述符、使能右上描述符-非旋转不变、接受点的检测器响应阈值、最大octave演变、每个尺度层级的默认子集数-四叉树吗、扩散率

  * class AKAZE : public Feature2D AKAZE关键点检测器和描述符提取器@ANB13
    * create() 提取器类型-4种、描述符size、描述符通道数、检测器接受响应阈值、最大4叉树深度、每尺度层的默认子层级数、扩散率

* 距离相关
  * template<class T> struct SL2
  * template<class T> struct L2
  * template<class T> struct L1

* 描述符匹配器
  * class DescriptorMatcher : public Algorithm 匹配关键点描述子的基类
    * train() 训练描述符匹配器，如flann. bf无
    * match() qDescriptors、tDescriptors-已在类对象中存储、matches-DMatch集-可能比q小、mask掩膜-允许哪些输入q和t矩阵的描述符匹配
    * knnMatch() qDes、tDes、matches-每个matches[i]为k或更少的相同的qDes、k-每个qDes的最佳匹配个数-或比k少、compactResult-mask有用时可用-允许压缩mask中排除的行
    * radiusMatch() qDes、tDes、matches、compactResult、maxDistance-匹配描述子间距离-如Hamming距离而非坐标距离、mask
    * match() qDes、matches、masks 无tDes的接口 knnMatch()\radiusMatch()同理.
  * class BFMather : public DescriptorMatcher 暴力描述子匹配
    * create() 范数类型、交叉检测-fasle为默认最近邻行为之找每个qDes的最近邻-true为k=1时返回pairs(i,j)=对于iqDec其j匹配集为最近邻\反之亦然-只返回一致的匹配对. 可选ratio测试-见与sift论文
  * class FlannBasedMatcher : public DescriptorMatcher flann加速的近似最近邻匹配
    * create() 无函数

* 绘制函数
  * enum struct DrawMatchesFlags 绘制类型
  * void drawKeypoints(image, keppooints, outImage, color, flags);
  * drawMatches(img1, kpts1, img2, kpts2, matches1to2, outImg, color, singlePointColor, matchesMask, flags)
  * drawMatches() 加入细度

* 评估
  * void evaluateFeatureDetector(img1, img2, H1to2, kpt1, kpt2, repeatability, correspCount, 某特征检测器指针);
  * void computeRecallPrecisionCurve(matches1to2, correctMatches1to2Mask, recallPrecisionCurve);
  * float getRecall(recallPrecisionCurve, 1_precision);
  * int getNearestPoint(recallPresisionCurve, float 1_precision);

* 视觉词袋 Bag of visual words
  * class BOWTrainer
    * add() ...
    * virtual Mat cluster() const = 0
    * virtual Mat cluster(const des) const = 0
  * class BOWKMeansTrainer : public BOWTrainer
    * cluster()重载
  * class BOWImgDescriptorExtractor 利用视觉词袋计算图像描述符
    * compute() image, kpts, imgDes-输出图像描述符，属于簇的关键点集指引，描述符
    * compute() 去掉了描述符

* 扩展--opencv-contrib库中的xfeaures2d
  * class FREAK : public Feature2D 快速Retina关键点 @AOV12
    * create() 使能方向归一化、使能尺度归一化、尺度化描述符、四叉树数目、用户自定义选择对
  * class StarDetector : public Feature2D
    * create() 最大size、响应阈值、线投影阈值、线二值化阈值、非极大值抑制大小
  * class BriefDescriptorExtractor : public Feature2D BRIEF描述子 @calon2010
    * create() 字节32、使能方向
  * class LUCID : public Feature2D 局部一致比较描述符
    * create() 描述子构建核、模糊图像先验核以用于描述子构建
  * class LATCH : public Feature2D 从树块码中学习的排列 @2015
    * create() 字节数、旋转一致性、半ssd尺寸、sigma
  * class BEBLID : public Feature2D @Suarez2020BEBLID 提升高效二值局部图像描述符
    * create() 尺度因子、描述子bits
  * class TEBLID : public Feature2D @Suarez2021TEBLID 基于三元组的高效二值局部图像描述符
    * create() 尺度因子、描述子bits
  * class DAISY : public Feature2D @Tola10 DAISY描述子
    * create() 初始尺度下的半径、q半径、qtheta、qhist、范数、H矩阵、插值、用户定义朝向
    * compute() image, keypoints, des, 
  * class MSDDetector : public Feature2D 最大自异性关键点检测器@Tombari14
    * create()
  * class VGG : public Feature2D
    * create()
  * class BoostDesc : public Feature2D
    * create()
  
  * class PCTSignatures : public Algorithm
    * create()
    * computeSignature()
    * drawSignature()

  * class PCTSignaturesSQFD : public Algorithm
    * create()
    * computeQuadraticFormDistance()

  * class Elliptic_KeyPoint : public KeyPoint

  * class HarrisLaplaceFeatureDetector : public Feature2D
    * create()

  * class AffineFeature2D : public Feature2D
    * create()
    * detect()
    * detectAndCompute()
  * class TBMR : public AffineFeature2D
    * create()
  * void FASTForPointSet()

* 匹配
  * void matchGMS()
  * void matchLOGOS()
  


* 所需必备
  * class KeyPointsFilter 对于关键点进行过滤的类滤波器
  * 