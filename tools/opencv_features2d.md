# Topic: opencv_feature2d & opencv_xfeature2d

## list

* features2dΪ2D�������
  * ���ڷ�Ϊ��
    * features2d_main �������������
    * features2d_match ������ƥ��
    * features2d_draw ���ƹؼ�����ƥ���
    * features2d_category ģ�����
    * features2d_hal Ӳ�����ٵ�.
      * features2d_hal_interface�ӿ�

* opencv�е��㷨�̳в��
  * class Algorithm
  * class Features2D : public virtual Algorithm // 2άͼ��������ȡ��������ȡ���������
    * detect() ����ͼ�񣬼���ؼ���
    * compute() ����ͼ�񡢹ؼ��㼯��Ȼ�����ؼ���������ӣ����ڲ����������ӵĹؼ��㣬�����Ƴ�. ��ʱ�еĹؼ���Ҳ������. ���������
    * detectAndCompute() ����ͼ������ҵ��Ĺؼ���ͼ����������
    * �������Բ���
  * typedef FeatureDetector Features2D ���������
  * typedef DescirptorExractor Features2D �ؼ�����������ȡ��

  * class AffineFeature : public Feature2D ���䲻����������ȡ�� @YM11
    * create() ������Ҫʹ�õļ����/��ȡ�� backend
  * typedef AffineFeature AffineFeatureDetector
  * typedef AffineFeature AffineDescriptorExtractor

  * class SIFT : public Feature2D �߶Ȳ��������任�㷨@Lowe04
    * create() ����Ҫ��ȡ����������Octave�������Աȶ���ֵ����Ե��ֵ��sigma
  * typedef SIFT SiftFeatureDetector
  * typedef SIFT SiftDescriptorExtractor

  * class BRISK : public Feature2D BRISK�ؼ�����������������ȡ��@LSC11
    * create() AGAST�����ֵ�����������Octaves��ʹ�øó߶���ģʽ�Բ����ؼ�������
    * create() ��ͬ�߶Ȱ뾶�б�����Բ�ڲ�����������̶����������γɡ�������ֵ�������ж�
    * create() AGAST��������ֵ�������Octaves���뾶�嵥���������嵥���̶������������γɡ��������ڷ����ж�

  * class ORB : public Feature2D ����BRIEF��������������������ȡ�� @RRKB11 ���ڽ���������FAST����ȶ��ؼ��㣬����FAST��Harris��Ӧ������ǿ�������ҵ��䷽����һ�׾ز���BRIEF����������
    * create() �����������500���߶�����1.2���������㼶8����Ե��ֵ31�����������ڲ�0������ÿһ������BRIEF�������ĵ���-2/3/4����������-�л�harris��fast�����ڳ���BRIEF�����ӵĿ��С��������ֵ

  * class MSER : public Feature2D ����ȶ���ֵ������ȡ�� @nister2008linear, @forssen2007maimally ��������pythonʾ��.
    * create() delta���������ֵ���������ֵ���ü����������������ƴ�С�����򡢲�ͼ��׷�ټ�ȥ�бȴ�ֵС��mser?�����������������ֵ�������³�ʼ��������̫С��Ե����Եģ����϶��С
    * detectRegions() ����>= 3*3��8ucxͼ������һ�ѵ㼯������boundingbox

  * class FastFeatureDetector : public Feature2D ��װʹ��FAST������������ȡ
    * create() ��ֵ10���Ƿ�Ǽ���ֵ���ơ����������
  * void FAST(image, keypoints, threshold, nonmaxSuppression);
  * void FAST(image, keypoints, threshold, nonmaxSuppression, ���������);

  * class AgastFeatureDetector : public Feature2D ��װAGAST�����������
    * create() ��ֵ=10���Ƿ�Ǽ���ֵ���ơ����������
  * void AGAST(image, keypoints, threshold, nonmaxSuppression);
  * void AGAST(image, keypoints, threshold, nonmaxSuppression, ���������)

  * class GFTTDetector : public Feature2D ��װʹ��goodFeaturesToTrack�������������
    * create() ���ǵ㡢�����ȼ�����������blockSize���Ƿ�ʹ��harris�������k
    * create() ͬ��-4���ݶ�size���Ƿ�ʹ��harris�������k

  * class SimpleBlobDetector : public Feature2D ��ͼ����ȡblob
    * create() ����

  * class KAZE : public Feature2D KAZE�ؼ�����������������ȡ�� @ABD12
    * create() ʹ����ȡ��չ128�ֽ���������ʹ������������-����ת���䡢���ܵ�ļ������Ӧ��ֵ�����octave�ݱ䡢ÿ���߶Ȳ㼶��Ĭ���Ӽ���-�Ĳ�������ɢ��

  * class AKAZE : public Feature2D AKAZE�ؼ�����������������ȡ��@ANB13
    * create() ��ȡ������-4�֡�������size��������ͨ�����������������Ӧ��ֵ�����4������ȡ�ÿ�߶Ȳ��Ĭ���Ӳ㼶������ɢ��

* �������
  * template<class T> struct SL2
  * template<class T> struct L2
  * template<class T> struct L1

* ������ƥ����
  * class DescriptorMatcher : public Algorithm ƥ��ؼ��������ӵĻ���
    * train() ѵ��������ƥ��������flann. bf��
    * match() qDescriptors��tDescriptors-����������д洢��matches-DMatch��-���ܱ�qС��mask��Ĥ-������Щ����q��t�����������ƥ��
    * knnMatch() qDes��tDes��matches-ÿ��matches[i]Ϊk����ٵ���ͬ��qDes��k-ÿ��qDes�����ƥ�����-���k�١�compactResult-mask����ʱ����-����ѹ��mask���ų�����
    * radiusMatch() qDes��tDes��matches��compactResult��maxDistance-ƥ�������Ӽ����-��Hamming�������������롢mask
    * match() qDes��matches��masks ��tDes�Ľӿ� knnMatch()\radiusMatch()ͬ��.
  * class BFMather : public DescriptorMatcher ����������ƥ��
    * create() �������͡�������-fasleΪĬ���������Ϊ֮��ÿ��qDes�������-trueΪk=1ʱ����pairs(i,j)=����iqDec��jƥ�伯Ϊ�����\��֮��Ȼ-ֻ����һ�µ�ƥ���. ��ѡratio����-����sift����
  * class FlannBasedMatcher : public DescriptorMatcher flann���ٵĽ��������ƥ��
    * create() �޺���

* ���ƺ���
  * enum struct DrawMatchesFlags ��������
  * void drawKeypoints(image, keppooints, outImage, color, flags);
  * drawMatches(img1, kpts1, img2, kpts2, matches1to2, outImg, color, singlePointColor, matchesMask, flags)
  * drawMatches() ����ϸ��

* ����
  * void evaluateFeatureDetector(img1, img2, H1to2, kpt1, kpt2, repeatability, correspCount, ĳ���������ָ��);
  * void computeRecallPrecisionCurve(matches1to2, correctMatches1to2Mask, recallPrecisionCurve);
  * float getRecall(recallPrecisionCurve, 1_precision);
  * int getNearestPoint(recallPresisionCurve, float 1_precision);

* �Ӿ��ʴ� Bag of visual words
  * class BOWTrainer
    * add() ...
    * virtual Mat cluster() const = 0
    * virtual Mat cluster(const des) const = 0
  * class BOWKMeansTrainer : public BOWTrainer
    * cluster()����
  * class BOWImgDescriptorExtractor �����Ӿ��ʴ�����ͼ��������
    * compute() image, kpts, imgDes-���ͼ�������������ڴصĹؼ��㼯ָ����������
    * compute() ȥ����������

* ��չ--opencv-contrib���е�xfeaures2d
  * class FREAK : public Feature2D ����Retina�ؼ��� @AOV12
    * create() ʹ�ܷ����һ����ʹ�ܳ߶ȹ�һ�����߶Ȼ����������Ĳ�����Ŀ���û��Զ���ѡ���
  * class StarDetector : public Feature2D
    * create() ���size����Ӧ��ֵ����ͶӰ��ֵ���߶�ֵ����ֵ���Ǽ���ֵ���ƴ�С
  * class BriefDescriptorExtractor : public Feature2D BRIEF������ @calon2010
    * create() �ֽ�32��ʹ�ܷ���
  * class LUCID : public Feature2D �ֲ�һ�±Ƚ�������
    * create() �����ӹ����ˡ�ģ��ͼ������������������ӹ���
  * class LATCH : public Feature2D ����������ѧϰ������ @2015
    * create() �ֽ�������תһ���ԡ���ssd�ߴ硢sigma
  * class BEBLID : public Feature2D @Suarez2020BEBLID ������Ч��ֵ�ֲ�ͼ��������
    * create() �߶����ӡ�������bits
  * class TEBLID : public Feature2D @Suarez2021TEBLID ������Ԫ��ĸ�Ч��ֵ�ֲ�ͼ��������
    * create() �߶����ӡ�������bits
  * class DAISY : public Feature2D @Tola10 DAISY������
    * create() ��ʼ�߶��µİ뾶��q�뾶��qtheta��qhist��������H���󡢲�ֵ���û����峯��
    * compute() image, keypoints, des, 
  * class MSDDetector : public Feature2D ��������Թؼ�������@Tombari14
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

* ƥ��
  * void matchGMS()
  * void matchLOGOS()
  


* ����ر�
  * class KeyPointsFilter ���ڹؼ�����й��˵����˲���
  * 