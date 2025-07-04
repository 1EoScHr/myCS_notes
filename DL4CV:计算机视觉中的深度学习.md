# UMich EECS 498-007 / 598-005: Deep Learning for Computer Vision

**计算机视觉中的深度学习**  
计算机视觉教机器去看，深度学习教机器去学习  

***

# Lec01: Introduction 介绍

## 开始之前

### 解开术语

**Computer Vision / CV**:  
*Building artificial systems that process, perceive, and reason about visual data.*  
*建立能够处理、感知和推理视觉数据的人工系统*  

+ **process**、**perceive**、**reason**  
+ **visual data**:可能是videos、medical scans医学扫描、（几乎）任何类型的连续值信号continuously valued signal，相当广泛  
	
### 为什么CV重要

**它无处不在！**  
人们分享、讨论的**视觉数据**，如Ins、ytb等产生**海量**的照片、视频，人是无法处理、分析的 ，所以研究感知、推理和处理图像信息的算法，构建出能够**处理这些数据的自动化系统**，就是计算机视觉  
视觉传感器数量激增，自动驾驶汽车、VR、无人机等，CV只会**越来越重要**。  

### CV使用的技术：Deep Learning 深度学习

+ **What‘s Learning？**  
**学习**是*构建可以从数据和经验中学习的人工智能系统的过程*  
不同于CV只想要理解视觉数据，不关心怎么实现  
后来人们发现构建基于学习的系统在构建cv、ai甚至整个cs都很重要  

+ **Deep Learning**：
*Hierarchical learning algorithms with many "layers", (very) loosely inspired by the brain*  
*非常粗略的受大脑启发的多层分层学习算法*，（loosely说明只是大体上受到人脑结构，但实际并不一样）  

### 课程内容
**深度学习**是机器学习的**子集**，CV、机器学习和其他研究**都是AI的子集**，韦氏图如下：  
![](pictures/CV/vmap.png)  
这门课程就关于**计算机视觉**与**深度学习**的交集。  

## 视觉与深度学习简史

### 计算机视觉

+ **1959, H&W**：两个神经生物学家研究猫脑神经元与视觉的关系，发现**定向边缘oriented edges**是最基本的视觉处理，进而得到了**视觉系统的层次表示**（从简单单元构建起越来越复杂的组合与响应），这两点意味着**计算机视觉的开端**  
+ **1963,R**：发表计算机视觉博士论文，基于“发现边缘是基本的视觉处理”，建立了一个可以**检测图片边缘**、**提取特征点**来理解物体与图像的**几何结构**的系统。值得一提的是，后来他成为了万维网的创始人。~~~视觉与计网大神~~  
+ **1970s,D**：提出**视觉的全过程**，输入图像-->获取图像边缘-->根据边缘提取深度信息->根据深度信息区分不同对象->考虑不同对象相对深度->推理三维场景  
+ **1979,G**：提出**部分认知**(**Recognition via Parts**)，要根据物体的不同部分，而不再只聚焦与边缘感知与简单几何物体。譬如通过识别不同圆柱体的组合来识别人  
+ ***以上部分受限于数码相机、算力等，都只算是没有实践的理论研究的“玩具”，从下面的80年代后，性能更好的相机与算力之下才研究更真实的图像，算是进入实践***  
+ **1986,C**：通过**边缘检测**来识别物体和图像，提出高效边缘算法与模板匹配算法  
+ ***AI winter***
+ **1997,N**：提出**分组认知**(**Recognition via Grouping**)，通过分组识别对象，对输入图像进行分割，得到语义上有意义的块chunks，在此之上进行识别  
+ **1999,SIFT**：提出**匹配认知**(**Recognition via Matching**)，检测输入图像中所有独特的不变特征关键点，然后匹配、对应另一张图像上的点  
+ **2001,V&J**：开发出一个强大的图像**人脸识别算法**，称为**增强决策树 boosted decision trees**，这是深度学习用于视觉的最早成功案例之一，很快实现从学术界到工业界的转变  
+ ***V&J打开了使用深度学习、应用大量数据来提高视觉表现的“魔盒”，从此机器学习有越来越多的用途、使用越来越多的数据来改进视觉识别系统***
+ **2001,PASCAL**：**pascal Visual Object Challenge**，互联网上下载更多图片，构建更大数据集更方便，让研究生打上标签，进行训练，从而性能在05-11年逐渐提高  
+ **2009,ImageNet**：**large scale visual recognition challenge**，采取众包打标签，发给所有人而不是牛马烟酒僧，因而过程中建立了非常大规模的数据集，已经成为CV的主要基准之一；还举办比赛，识别错误率逐年降低  
+ **2012,alex kachafski**：alex开创性的论文，提出**深度卷积神经网络**，错误率粉碎其他算法，视觉新时代。  

### 深度学习

+ **1958,Perceptron**：一项算不上算法的研究，叫做**感知器**，是最早的可以进行学习的计算机系统之一，当时作为**硬件**实现，十分复杂，但确实可以以某种方式从数据中学习，能够在20x20画幅中识别字母。在**现代视角**看，称其为**线性分类器 linear classifier**  
+ **1969,M&P**：出版《**Perceptrons/感知器**》一书，提出感知器**不是**一种神奇的**硬件设备**，**而是**一种特殊的**学习算法**，有一些特定的函数能学习表示，而其他类型的函数无法学习表示，预示了学习不是万能的，人们失去兴趣；但书中还说明了算法有一个**潜在版本**，称为**多层感知器**，可以实现学习许多类型的函数，却无人问津  
+ **1980,N**：论文提出名为"**neocogmatron**"的系统，计算实现视觉到感知的过程，其中提到的两个操作，类似现代术语中的**卷积convolution**与**池化pooling**，这与2012年alex的整体结构**十分类似**，只是遗憾的**没有实用的训练算法**  
+ **1985,B**：论文提出**反向传播 Backprop 算法**，用于训练多层感知器，是**第一次**通过多层计算成功且有效地训练更深层次的模型  
+ ***AI Winter***  
+ **1998,L**：提出**卷积神经网络 Convolutional Networks**，使用了卷积、池化、视觉类似的多层系统、反向传播等，也获得了学术界与工业界的成功  
+ **2006,D**：**Deep Learning**出现，deep指神经网络算法是多层，蓬勃发展  
+ **2012,alexNet**：ALEX！自此以后，无处不在，各种识别、匹配、分类、预测  

### 2012的AlexNet

**历史上**要五十年以后才能做出定论  
但老师个人理解是，其**伟大**在将三个重要组成部分一起组合：  
	1. 在深度学习、神经网络、机器学习的潮流中开发的**强大学习算法集**，**Algorithms**  
	2. 随着相机、互联网兴起，得到了空前**庞大的数据量**，用于训练系统，**Data**  
	3. 贯穿整个历史的**算力提升**，GPU计算能力呈指数增长，**Computation**  
2018年图灵奖，2024年诺贝尔物理学奖，建立真正的计算机视觉，未来还有很长的路要走。  

***

# Lec02: Image Classification：KNN

## 综述

**图像分类**，是本课第一个学习算法，是计算机视觉的**核心任务 core task**。  
**流程**：输入图像-->图像分类算法-->输出类别标签，因此算法需要预先知道有哪些标签  
**用途**：可以用来处理医学影像、天文分类、珍稀动物识别等，应用十分广泛且强大  
	还有一个更有趣、容易被忽略的角度：**图像分类本身也是计算机视觉不同算法内部的基本组成部分**，譬如对象检测、图像字幕(image captioning)、围棋算法等。  
**思想**：也许针对特定的物体识别，利用人类思想，硬编码出所有边界条件，可一旦切换需要识别的物体，算法的一切努力将白费。所以更需要的是**更健壮、更有拓展性、不用依靠人类知识硬编码的算法**，这就是**机器学习**

## 遇到的问题 Challenges

语义鸿沟是算法有无的问题，剩余的是算法是否具有鲁棒性的问题  

+ **语义鸿沟 semantic gap**  
人可以一眼辨认出图像的类别：光子打在视网膜，神经传到大脑，一系列的复杂处理，最后产生了直觉；但机器得到的只有巨大的数字网格，没有这种直觉，这个问题称为**语义鸿沟 semantic gap**。
+ **类内变异 intraclass variation**  
以判断猫猫为例，不同品种的猫形状一致，但毛色、细节不同，因此图像上像素颜色也不同，需要构建一个足够健壮（**鲁棒robust**）的系统，能够处理同一个类别中巨大的**个体差异**  
+ **精细分类 fine-grained categories**  
进一步的，我们不只想判断猫，更想判断猫的具体品种，尽管它们彼此相似，仍然要有足够健壮的算法来完成。  
+ **背景杂波 background clutter**  
有时需要识别的主体会以某种方式融入背景，因此算法对背景杂波也要有良好的鲁棒性，足够健壮，抗干扰  
+ **光照变化 illumination changes**  
当场景中的光源发生变化时，图像像素的颜色发生巨大变化，但图像的基本语义不会变化。因此算法对光照变化应该具有鲁棒性  
+ **变形 deformation**  
有时我们要判断的对象可能以不同的姿势出现，就像猫能变成任意形状，因此算法要能处理变形  
+ **遮挡 occlusion**  
算法需要能够处理遮挡，也就是对象可能只有部分能够看到  

## Machine Learning:Data-Driven Approach 数据驱动

真的是完全不同的编程方式。  
以往的算法都是基于自己的人类知识，告诉电脑具体应该如何做才能产生理想的输出；  
而数据驱动的机器学习，只是通过输入的数据集来对计算机进行编程。  

### 实现方法

1. 首先收集大量的**图像数据集**(dataset)，并用想要识别的分类标签**标记**(label)它们  
2. 然后部署一种机器学习算法，用收集到的数据集与分类标签训练，算法会提取**统计依赖性**(statistical dependencies)  
3. 在新的图像上评估得到的分类器  

### 编码方式

**不是**编写一个单一的、名为"classified_image()"的整体函数，  
**而是**两个API："train(images, labels)"与"predict(model, test_images)"，  
	前者输入数据集与标签进行训练，返回训练好的模型；  
	后者输入训练好的模型与新数据集，返回其对新数据集分类的标签。  

### 数据集来源

**MNIST**：一个灰度图手写数字数据集，比较轻量化，就像生物学家的果蝇，有任何机器学习的新想法都在其上试验  
**CIFAR10**：彩色10类数据集，相比真正的大数据集是很小的，但依然很有挑战，作业基本使用此数据集  
**CIFAR100**：彩色100类数据集，cifar10的兄弟  
**ImageNet**：1000类，非常庞大，图像分类数据集的**黄金标准**，是benchmark。  
**MIT Places**：偏向各类场景的数据集  
**Omniglot**：不同于越多训练数据越好，每个分类的训练用例只有20个，力求效率最高、鲁棒性最好  

## First classifier：Nearest Neighbor

最简单的图像分类算法，甚至都不能称为"算法"，其能算得上“学习”的部分只有记忆了所有训练数据。  

### 思路与复杂度

+ 对于**train()**，不对输入的数据集与标签进行处理，而是单纯的**记住所有的训练数据**  
+ 对于**predict()**，使用某种比较或相似函数，将需要判断的新图像与训练集中所有图片进行**对比**，追踪训练集中**最接近的相似图像**，返回其标签  

因此为了实现这个算法，需要实现可以计算两个图像相似度的函数，称为**Distance Metric 距离度量**。   
+ **L1距离**（**曼哈顿距离**），即为**将两个图像逐像素相减，取绝对值，再求和**;  
+ **L2距离**（**欧几里得距离**），即为**将两个图像逐像素相减，求平方和，再开根**。  

在有N个训练用例的情况下，**训练**的时间复杂度（这里只关心测试用例数量，不深入到像素）：  
	**O**(**1**)，因为**只需要存储**输入的图像与标签就能完成训练  
**测试/预测**的时间复杂度：  
	A：**O**(**N**)，因为认为匹配两图像的大小、并计算其L1距离的过程耗费为常数 

这实在是**极其不好**的，因为*我们想让机器尽可能从训练数据中提取某种特征，哪怕花费很长时间在训练上，但最终用来预测时运行得很快*——这是**“学习”的意义所在**，但nearest neighbor与其**背道而驰**（尽管其还有升级过后的加速算法）。  

### 可视化分析与L1、L2距离对比

**可视化表示**：    
	![](pictures/CV/neighbor.png)  
	**坐标空间**是某种方法表示的所有训练图像  
	**每一个点**都是一个训练图像，其颜色为其类别标签  
	**底色**是类别标签，根据训练图像扩大，若预测的图像落在某处，即可按照对应的颜色判断标签（其实就是**nearest neighbor**的字面意思，很直观了）  
	**决策边界 Decision Boundary**是不同底色的交界处  
	在决策边界上，可以发现这些决策区域非常嘈杂，互相交错，还有异常值成为的"飞地"，可能会对图像分类**造成干扰**。  

**K-nearest neighbor**：  
	为了消除决策边界上的锯齿，得到一个更健壮的分类器，可以用K-nearest neighbors进行优化，如下图：  
	![](pictures/CV/neighbor_k.png)  
	原理是**不再只根据最近的“邻居”**进行判断，而是采用**获取多数票**的思路，找到离其最近的k个邻居，根据邻居最多的种类来判断。
	**作用**使得决策边界变得更平滑，同时减少异常值（飞地）对分类的影响；但可能出现一些无法判断的区域，需要加上额外规则来打破。  

采用L2距离、K=1时：  
![](pictures/CV/neighbor_L2.png)  
会发现决策边界可以向任意角度延伸，而L1的边界只能横、竖、45度。  

### 超参数hyperparameters

由此可见**距离度量**与**k值**的选取十分重要，那么应该如何选择？  
这个问题就属于**hyperparameters 超参数**：指一些算法中某些参数有多种可选，但我们**无法直接**根据训练数据做出最优选择，只能在训练程序开始时**手动设置**，多次尝试，观察结果来判断。下面是超参数的设置方法：  

**错误思想**：  
+ ~~选择在整个数据集上表现最好的超参数~~  
**错因**：其可能导致过拟合，让机器在新的数据上不再有估计与想法  	
+ ~~将训练数据集分成train与test两部分，用train训练，选在test上表现最好的超参数~~  
   **错因**：会退化到整个数据集上，因为相当于查看了test的答案，测试集不再是看不见的数据，**这是机器学习模型中一个根本性错误** 
   

**正确方法**：  
将数据集分为三部分：**train训练集**，**validation验证集**，**test测试集**  
+ train：用于训练算法模型  
+ validation：用于修改超参数  
+ test：验证集上的超参数设置完全后，在最后的最后使用一次测试集来评估效果  
这样实现了验证算法在真正看不见的数据上的表现，尽管**恐怖**（最终测试机上的表现可能让项目功亏一篑），但是唯一正确的机器学习数据处理方式。  

**更优化方法：交叉验证**：  
继续细分数据集：除**test测试集**以外，划分为**若干fold集**  
然后开始训练，令每一个fold都当一次验证集，其余fold为训练集。最后将每一次的超参数平均起来获得结果。  
这是**最稳健**的机器学习训练方法，但也很**昂贵**，要反复训练多次，所以在实践中通常不这么做，尤其是deep learning。如果数据集小，算力充足，还是推荐这样训练。  

### 一些性质

+ nearest neighbor几乎可以适用于任何不同的数据类型，只要选择好距离度量，其相当健壮。  
+ 当数据集趋于无穷大时可以表示任意函数，但是有**维度的诅咒 curse of dimensionality**，我们若想用它来实现高维数据的精确预测，所需要的数据永远无法收集完全。  

### 总结

**K-Nearest Neighbor**因为足够简单，很容易理解，因此对于任何数据类型都很鲁棒；  
但因为运行缓慢、要求大量训练数据、没有真正的学习过程导致其“距离度量”在语义上没有很大意义与在图像上不直观（譬如背景由黑到白，距离度量会极大变化，而图片语义并没有变化），所以在**原始图像分类处理上很少使用**  
但是在**特征向量计算的深度卷积神经网络上**有着奇效，在后面会学习。  
（*在这里我根据CG中对卷积、信号等的了解，在此猜测原因：原始图像上只有原始数据，不包含语义，最邻近的也只是“形似”，所以没用；但卷积神经网络处理后的图像变成了语义数据，这时利用最邻近就相当于在语义上寻找，是“神似”了，希望我的想法是正确的*）  

***

# Lec03：Liner Classifiers

学完总结：03、04节从**线性分类器**出发，提到**损失函数**，最后再到**梯度下降**一整套，如图：  
![](pictures/CV/linearclasssum.png)  

**线性分类器**：听起来很简单，但对构建**神经网络**很重要，有很多相似之处。  

## 简单的参数化方法：线性

其利用了**参数化方法**，即构建一个函数，有变量**x**与**W**，其中x为输入的图像数据，W为一些用于在训练中学习的权重，最后输出对于属于每种类别的概率大小。  
**线性分类器**的构造十分简单，为**f(x,W) = Wx**的形式，有时还会加上**偏移量b**以抵消误差，十分**线性**。  

根据其**线性特征**，可以有**f(cx,W) = W(cx) = c\*f(x,W)**，这说明如果将原图像进行某些**放缩**后（比如将每个像素的值减半），分类器仍然能够正常工作，“*so this is maybe a bug maybe a feature*”，从某种意义上，**与人类相似**，尽管图像整体变化，也不影响判断。  

## 理解角度

有三种理解线性分类器的角度，如下图：  
![](pictures/CV/LinearClassifier.png)  

### 代数角度

**具体**的，如果图像采用**CIFAR10**数据(32x32x3)，则有：  
+ **x**为(3072,)，就像作业1中，将图像的多维张量数据**矢量化、扁平化**  
+ **W**为(10,3072)，为**权重矩阵**  
+ **b**为(10,)，用来消误差  
+ **f(x,W)**为(10,)  

**Bias Trick**（但并不常用）：  
在构建出的**f(x,W) = Wx + b**中，为了使输入与运算更简洁，可以将**偏移项b**整合在W与x中，具体的，以CIFAR10为例，可以将b放在W后面，成为新的**W'**(10,3072+1)，同时在x底部增加一维，值为1，变为**x'**(3072+1,)，这样就有了**f(x,W) = W'x'**，结果是一样的。  
（**这里有点图形学里面的齐次坐标的感觉**）  
**BUT**在计算机视觉的实践上**并不经常使用**，因为不便于后续的**卷积操作**，同时在模型训练中，将偏移与权重分开**更利于初始化等操作**。

![](pictures/CV/liner.png)  

### 视觉视角

还可以采用**将权重矩阵按类别拆开**，重塑为原图像的形状，进行一个点乘（对应元素相乘求和）再加上拆开的偏移，称为**视觉视角**，见上图右。  

之所以称为“**视觉视角**”，是因为我们如果将重塑的每一类权重矩阵**可视化**表示出来，就得到了**一个类别的图像模板template**，求内积实际是判断模板向量与图像向量的接近程度，也就是**权重**（*图形学学到的*），是一种**模板匹配**，说明了其更依赖于**图像上下文context**，即识别飞机不光飞机，还有蓝天；识别鹿不光是鹿，还有森林。所以，若**物体不寻常的出现在情景**，会导致严重的**误判**，比如森林里的汽车。  

此外，其还有**缺陷**，是其会用一个模板覆盖所有可能的模式，做不到类内细分。  

### 几何视角

比较**抽象**。  
在一个二维空间中，x轴为图像某个像素的取值（假设其他像素值不变），y轴为不同分类的分数，由于**线性性质**，不同分类器的分数与像素取值的关系也是**线性**的，因此能够根据像素值来对应分数。  
![](pictures/CV/geoview1.png)  

在二维基础上**拓展**，增加一个像素，想象x轴为像素1的取值，y轴为像素2的取值，z轴为在对应点上不同分类的分数值，我们可知不同的分类值会**形成一个平面**（两个方向都是线性的，组合起来也是一个线性平面），根据高低来对应分数。  
或者先找到某条直线使得某类得分为定值，沿正交的方向即变化方向，类似等高线  

尽管人类的想象力到此，但也不妨继续抽象。设在由图像所有像素不同取值组成的**高维空间**中，存在着不同的**超平面**，就像三维中的那样，切割整个高维空间，根据某种映射关系，就可以从几何上得出各个类别的分数。  
![](pictures/CV/geoview2.png)  
这也可以用于解释线性分类器无法识别一个类别的多个模式，因为这多个模式可能实际对应不同的独立区域，无法用线性来分开。  

### 感知器即线性分类器

在机器学习历史中，提到过的**感知器Perceptron**就是一个线性分类器，提到有的函数能够学习，而有的不能学习，就是因为有的函数无法通过一条直线来分开。  
譬如**异或函数XOR**，x、y、f(x,y)无法用一条直线分开。  

## 训练思路：损失/目标/成本(loss/objective/cost)函数

在实际训练中，需要使用**损失函数 loss function**，使用令损失函数最小的W。  

损失函数是一个用于展现当前分类器在已有数据上的表现的函数，高损失意味分类器性能不好，低损失为性能好。  
在数学的表示上，为下图所示：  
![](pictures/CV/lossfunction.png)  
依次是**数据集**、**单个类型的损失**、**整个数据集的损失**。  

### Multiclass SVM Loss 多类SVM损失

**核心思想**为**正确类别的得分**应该比其他类别的得分**更高**，基于此来分布**损失函数**。  

需要追踪**正确类别的得分**与**其他类别中的最高得分**，使得正确类别得分对应的**损失函数为0**，而从正确类别到其他类别最高分的**损失函数**开始**线性增长**。  
在损失--得分图像上，会形成一个**线性区域**与**0区域**，由于形状像门上的铰链，因此称为**铰链损失 Hinge Loss**。  
![](pictures/CV/hingeloss.png)  

在**数学描述**上，也可以表示出这个损失函数，见上图右。  
**某一类损失**可以通过求所有max(其他类别得分与真正类别得分之**差**再加一，0)之和来计算（也就是说，当真正类别得分**大于**其他类别时，损失就为0）  
则整个**数据集总损失**为所有类损失的均值。  

一些点：  
+ 如果某一类别的损失为0，其中图像改变一点，仍然会0损失。这是**多类SVM损失**的一个特性，当已经正确分类后，对数据进行很小的修改，不会影响损失。  
+ 单个类别的损失**最小值**为0，**最大值**为无限，如果正确类别得分十分低。  
+ 当刚初始化权重矩阵时，并**没有进行训练**，其值为随机的小值，期望的**某一类损失**大约为**类别-1**，由于高斯分布等原因，这有助于调试  
+ 如果某一类损失求和时**算上该类**，只会为损失增加1，但并**不会改变**权重矩阵，因为绝对的大小值没有发生变化  
+ 如果某一类的损失使用**平均值**而不是总和，**不会改变**，因为绝对大小依然不变  
+ 使用**其他方式**来求得某一类的损失（譬如求和改成平方和），这会以**非线性**方式改变所有分数，导致权重矩阵发生变化，**不再是多类SVM损失**  
+ 如果存在某个权重矩阵，使得数据集总损失为0，其**并不唯一**，因为权重矩阵的倍数也有相同效果  
这就需要我们**引入**损失函数之外的**额外机制**，来表达对不同分类器的偏好，这就是**正则化 Regularization**的想法。  

### Cross-Entropy Loss / Multinomial Logistic Regression 交叉熵损失/多元逻辑回归

是训练神经网络时最常使用的损失。  
上面提到的多类SVM损失并没有对损失做出**实际的解释**，而单纯是出于“正确类型得分应大于其他类型”的目的而构建，而**交叉熵损失**要做的就是尝试找到一种方法来**解释模型预测的分数**，即将**分数向量**解释为**概率分布**。  

使用**Softmax**函数，如下图：  
![](pictures/CV/softmax.png)  
之所以是叫softmax，是因为其实现了最大函数的可微逼近（*？*）  

进行损失计算的过程是：  
1. 先获取初始的预测分数，是分类器对输入样例属于不同类型的得分，称其为“**未正则化对数概率分布**”，由于概率分布需要为正数，所以需要进行指数化处理(e^x)  
2. 获得指数化后的分数，称为“**未正则化概率分布**”，接下来则对其进行正则化，即分配这些数使得其和为1  
3. 获得正则化后的分数，称为“**概率分布**”，这样获得了一个向量，每个元素非零且总和为1，这个向量就解释为我们想识别的各类的概率分布。以上就实现了**softmax**  
4. 然后我们取输入样例的**正确类型的负对数(-log)作为损失**（这实际实现了对观测数据权重概率的最大似然估计，证明略）  
	其正确性可以通过**信息论**来证明，现在得到的是**实际概率分布向量**，但实际上**理想概率分布**应该为一个只在正确分类维度上为1、其余维度为0，可以通过信息论来做（关于P、Q，信息论中将P看作真实分布，Q看作非真实分布，即我们的理想分布与真实分布，此真实非彼真实）：  
	+ **Kullback-Leibler KL散度**，用来衡量两概率分布之间的差异，如下图：  
	![](pictures/CV/kullback.png)  
	最后化简后得到的“差异”就是正确类别概率的负对数。  
	+ **Cross Entropy 交叉熵**，是另一种测量概率分布之间差距的方式，如下图：  
	![](pictures/CV/cross.png)  
	**在定义上**，概率分布差距与交叉熵单调相关，而交叉熵的**代数定义**可以推导得到上方所示与熵和KL散度的关系（推导过程如下图）,实际得到的还是负对数  
	![](pictures/CV/cross_prove.jpg)  

一些点：  
+ 损失的**最大、最小值**：最小值仍然为0，最大值仍然为无穷，但这里最小值不同于多类SVM可以真的到0，但交叉熵损失只有在纯粹的理想情况下才能到0。  
+ 当刚初始化，所有值都是随机的小值，某一类的损失期望为**log(类数)**，因为可以认为随机的初始值大小都相近，因此最终的概率分布每一维都是**1/c**，所以损失应为-log(1/c)=log(c)（这里也捉视频里的虫，没有加负号！！！）。  
这在同样**有助于我们的调试**，当刚初始化时的损失大于预测，说明模型有一个错误以至于优化的还不如随机值，同样也适用于多类SVM。  

### Regularization 正则化

在上面完成损失函数后，我们还可以在模型总损失后加上一个**正则化**的部分来拓展损失函数，如下:  
![](pictures/CV/regularization.png)  
第二项就称为**正则化项**，其**不涉及**训练数据，是为为了控制模型而增加的额外参数。  
其往往带有一个**超参数 hyperparameter**，即*lambda*项，用来**控制**总损失与人为偏好的**比重**。  

**正则化目的**：  
1. 能够让模型除“最小化训练错误/模型损失”**之外**，还能表达对**不同类型模型的偏好**(出于主观目的、人类先验知识、想要学习的分类器类型、想要把训练重心放在某个功能中)  
2. 避免**过拟合 overfitting**，让模型牺牲一些在当前数据的表现，换取对未知数据的更合适的表现  
3. 添加**额外的曲率**，这有助于优化过程（**？**）  

**线性分类器**常用正则化项类型为：  
+ **l2正则化**：**R(W)=∑∑W^2**(权重矩阵各项平方和)；  
+ **l1距离**：**R(W)=∑∑|W|**；  
+ **Elastic net(L1+L2)**：**∑∑(βW^2+|W|)**，通常用于神经网络(但通常神经网络还用其他正则化)  

### 总结：两类损失（SVM、交叉熵）与正则化

**多类SVM损失**的最小单元是**一个类别**，其性质（损失可以达到0，并且此后数据点微调不再会产生影响）说明了其模型效果到某一点后就不再会变化；  
而**交叉熵损失**的最小单元是**每个样本**，性质（损失不可能达到0，数据点微调也会持续产生影响）则注定了**有交叉熵，就可以永远训练**。  

两类损失都可以**对权重集施加不同的偏好**，正则化也能实现这一点，从而两者组成更加泛化的**损失函数**来训练模型。  

***

# Lec04：Optimization 优化

前面提到的线性分类器，如今已经有了数据集、损失函数等部件，就剩下最重要的部分：**如何确定、优化权重矩阵W**的值。  

可以理解成在高维空间中，蒙着眼睛，去寻找最低（L最小）的位置。  

## Random Search（bad！）

采用完全随机的方法，**随机生成**权重矩阵，然后分别评估损失值，追踪最低损失值。  

这在**算法**层面，**随机搜索**无疑是**愚蠢**的；但实际测试效果还说得过去，**not bad**。

实际上我们还是需要用梯度下降法来解决。  

## 梯度计算方法

而**梯度gradient**告诉我们函数**最大增加**的方向，所以应该向着**负梯度**的方向前进。  

1. **Numeric数值**:  
	+ 认为权重矩阵是一个高维向量，最终梯度与其同维度。在当前W下使用**损失函数**求出**损失Loss**
	+ 略微改变W中一维度，获取整体新的Loss，继续用导数定义求得**梯度dL/dW**中对应维度的值  
	+ 重复，从而得到在当前权重矩阵所对应损失的梯度  
	+ 因此，这个方法的**复杂度为O(ndim)**，还是比较**慢**的，当深度学习采用**维度很高**时。并且其精度不高，因为是近似替代。  
2. **Analytic解析**：  
	利用解析方法求出具体表达式，准确、快速、无误  
	一般会在实际数据上用**解析**进行计算，但用低维度上的**数值**进行debug工具来验证。称为**gradient check**，在实际工作上也会常用  
3. **backprppagation反向传播**  
	在**实践**中，我们更多的用其来计算任意梯度，见***第六节***内容。  

### (Full) Batch Gradient Descent (BGD)

名为**批量梯度下降**，**算法原理**很简单：  
**初始化**权重矩阵各值-->**迭代n次**(在**整个**数据集上根据损失函数等**计算当前梯度**-->根据**学习率**更新梯度)，其会自然的在接近最优解时放缓  

其中有三个**Hyperparameters超参数**：  
+ 权重矩阵**初始化**方法  
+ **迭代次数**：程序需要在有限时间内完成，受限于性能也要进行精度控制  
+ **学习率**：我们对计算出的梯度要有一定的信任度，因此其走的方向大小也要有一定的选择  

之所以为**批量batch**，甚至**full**，是因为在计算总损失时**遍历了整个数据集**，譬如SVM损失是以类为单位，交叉熵是以每个样本为单位，计算出了**模型总损失**，当样本量太大时**开销会非常大**；而计算梯度又是另一层大开销。这样**每一步**都会非常**缓慢**，并不可行。  

### Stochastic Gradient Descent (SGD/mini batch)

名为**随机梯度下降**，在实践中比较**常用**。这里的**思路**发生了变化：  
+ 用整个训练集的**子集**来计算**近似**损失函数与梯度，而不是计算**整个训练集**上的损失，这个子集的大小**通常为32/64/128**
+ **初始化**权重矩阵各值-->**迭代n次**(从训练集选择**minibatch**-->在minibatch上根据损失函数等**计算当前梯度**-->根据**学习率**更新梯度)  

**超参数**依然还是那些之前的，只是相应的增加了**minibatch**的两项：  
+ **mb的size**，但是效果其实并不敏感，因此尽可能的选择大的size直至显存用光  
+ **mb的采样方法**，在分类器作业中采用洗牌打乱-->按序读取-->再次打乱的方式，在其他应用上就需要仔细考虑研究了。  

名为**stochastic随机**是因为相当于完整的数据集进行蒙特卡罗样本估计。  

尽管SGD很有效，但也有一些潜在的问题：  
+ 当模型下降速度的极值相差太大（大值太大，小值太小）时，会因为学习率步长太大而产生**zig zag曲折**，或者学习率步长太短而**收敛缓慢**，这个问题称为**high condition number**，与任意点的哈希矩阵奇异值有关，不讨论。  
+ 当存在**local minimum局部最优解**或者**saddle point鞍点**（一个方向增加，另一个方向减小，在**高维优化**中常见），都是因为梯度为0,我们可能会卡在那里。
+ 随机的部分噪声太大，以至于梯度方向性很弱，效率很低  

### SGD + Momentum（动量随机梯度下降）

为了解决上述SGD的问题，采取一种更为**智能化**的方法：Momentum。其是一种结合**物理直觉**的优化方法，之前的仅仅是沿着梯度的方向前进，而这个方法是赋予了**“速度”**。  

**具体做法**：  
引入一个**速度**向量v，每次计算先计算**v**的大小，通过`v_t+1 = rho*v_t + dw`来结合**上一位置速度**与当前位置**梯度**（rho通常取0.9或0.99来模拟**摩擦**），从而得到当前位置速度，然后再`x_t+1 = x_t - α*v_t+1`来计算。  
在实际应用上，还有多种实现方式，但道理都一样。  

这样在**梯度下降**的过程中，在局部最优点、鞍点都可以继续进行了。  
并且由于下降过程中的**动量**，噪声实现了一定程度的消除，比较好的解决了问题。  

另一种动量思路：**Nesterov Momentum**：  
速度不止考虑当前，更考虑下一时刻，更加平滑：`v_t+1 = rho*v_t - α*d(w + rho*v_t)`，如此便可以同步考虑到未来。  
但由于在计算时更倾向于使用当前所在位置的梯度，所以可以进行**变量替换**，进而重新组织成便于计算的形式：  
![](pictures/CV/nesterov.png)  

### AdaGrad

**不同于**之前BGD、SGD那“持续跟踪**梯度**的历史**平均值**”的思路，AdaGrad将跟踪历史**梯度平方**，然后依靠平方来限制步长（方法为`w -= learningrate * dw / (squaresum.sqrt() + 1e-7)`），从而步长可以随着实际情况递减，最终在底部平稳降落，称作**二阶动量**。  

但是缺点为平方和只会越积越大，遇到特殊情况下容易卡死不动。  

### RMSProp：leak AdaGrad

是为了解决AdaGrad的问题而改进的，类似于动量下降中的**摩擦**，让平方和不断**泄漏**，以防卡死。  

**具体实现**：  
`squresum = decay_rate*grad_squared + (1 - decay_rate)*dw*dw`，控制衰减率即可  

### 梯度下降综合：Adam

以上几种方法实际是两类方法：SGD与Ada，两种各有**微妙特点**，如图：  
![](pictures/CV/gradcmp.png)  
**SGD类**(blue)会“横冲直撞”，下降过头，然后再调头，伴有轻微震荡（超调）；  
**AdaGrad类**(red)则十分流畅的下降到最低点，不会有不断的调整过程，一路向着正确的方向弯曲  
（尽管真实工作中，会接触到高阶得多的数据，但是基于二维投影的直观表示也有些用）  

**那么，为什么不把这两类综合起来？**  
于是有了**Adam：RMSProp + Momentum**的方法：  
![](pictures/CV/adam.png)  
完美的综合了两种方法，在梯度下降的过程中，既有累积的动量，又有自适应的步长。  

**注意**，这里还有**偏差矫正**的部分，这一部分是由于RMSProp部分引入的：  
当**t=1**，也就是在**起点**时，如果**beta**的值都很近似1(0.999)，就会出现由于值过小而阻塞在起点无法开始。  
因此，就引入如图所示的**偏差矫正**项，在起点时可以避免走不动；在步数多了之后也会自行消失。  

在**实践**中，也总结了Adam的常用参数组合，有很不错的鲁棒性，几乎对所有模型很不错：  
+ beta1 = 0.9, beta2 = 0.999
+ learning_rate = 1e-3 - 5e-4 - 1e-4的范围内

***Adam是老师推荐的最常用的默认方法***  

### 二阶优化：

前面所提到的方法都是**一阶优化**，因为都是用**梯度**来获取函数的线性近似，再走向最小值；   
那么可以拓展到**二阶优化**，使用梯度与Hessian获取函数的局部二阶近似，再走向局部最小值，这能够帮助自己精确选择合适的步长，当曲率大时，自然会走小步；曲率小时走大步。  

其具体实现较为麻烦，要对**损失函数**求二阶泰勒展开（应该是线代或计方讲过带矩阵的），然后求解。解很复杂，尤其是二阶Hession矩阵开销为O(N^2)，再加上转置开销O(N^3)，在数据量大时难以接受。  
同时二阶优化也不能处理**随机**梯度下降，只能Full Batch。  
**因此在高维度数据与随机梯度并不适用**，但是当数据低维与可以负担BGD时可以考虑。  

***

# Lec05：Neural Networks 神经网络

终于到了课程的**重点**，将离开性能比较弱的**线性分类器**（几何角度上要通过一个超平面分隔开不同类型的点，视觉角度上一个类别只能对应一种模式），开始探索基于**神经网络**的**模型**。  
性能将会更强大，分类精度将会更高。  

## before networks：feature transforms 特征变换

在进入神经网络前，先针对线性分类器**几何角度**上的缺点进行优化，方法就是**特征变换**。  

以**二维**为例，原**线性不可分**的空间通过特定的变化，得到一个**线性可分**的空间，如下图，通过转换为极坐标从而可分：  
![](pictures/CV/featuretrans.png)  
所以对于**线性分类**，如果能巧妙选择一个特征变换与合理的数据结构，就可以适当克服线性分类器的缺点。  

**事实上**，特征变换的思想比较广泛的被应用，尤其是计算机视觉，其能够提取出图像中的某些特征，如下方：  

### 几种特征变换

+ **Color Histogram 颜色直方图**  
原理是其**丢弃**图像的**空间**信息，只保留**颜色**信息，形成颜色直方图作为图像的特征。  
其意义在于突破了训练用例中的空间限制，譬如一辆特定颜色的汽车在图片的不同位置，以往的线性分类器无法解决，但是利用颜色直方图就可以绕过。  
+ **Histogram of Oriented Gradients(HoG) 方向梯度直方图**  
其与所有空间信息、只保留颜色信息的颜色直方图丢弃**相反**，其**丢弃**所有**颜色**信息，只**关注空间**信息，如边缘的局部方向与强度。  
+ **Bag of Words 词袋**  
	不同于以上两种特征变换都需要**人为指定**变换的形式，BoW是一种**Data driven 数据驱动**的特征变换。  
	其思想是：  
	在**训练阶段**，从巨大的训练图像集中**随机**提取许多**patches**，就像句子里的单词，图像语义其实是由其中的patches组成的。通过基于patches的大量训练，形成**codebook**或者**visual words**。  
	在**编码阶段**，将自然的从输入图像中提取出codebook中的特征，以此来指定某一类别的特征，并用于分类判断。  
+ **综合特征**  
不必只纠结于使用某一种特征变换提取出的图像特征，我们可以将各种方式得到的特征**连接**，形成一个**长特征向量**，包含了图像各种方面上的特征值，因而能使分类器更健壮。  
2011年的imageNet挑战冠军就使用这个思路，压缩了许多特征。  

### 承上启下

![](pictures/CV/lvsn.png)  

可以看到，线性分类器的pipeline可以理解为两个部分：**特征提取器**与**训练出的模型**。  
前者输入图像的像素，输出提取出的图像特征；后者基于这些图像特征进行模型的训练。  

可以发现，线性分类器**只能**在给定的特征上进行训练，而**不能**对自动特征本身以及系统的其他部分进行优化处理，**不能**达到理想的最佳效果。这是一个**缺陷**。  
而作为对比，**神经网络**则可以对**整个系统**的每一个部分进行优化处理，从输入像素到输出结果的全过程，从而**最大化**分类效果。  

所以我们需要走向**Neural Networks 神经网络**。  

## 神经网络的形式

神经网络有些类似于线性分类器的**推广**；或者说，线性分类器是一个**单层**神经网络。  

+ 从**线性**分类器，也就是**单层**神经网络出发：***f = Wx***  
+ 2-layer：***f = W_2 max(0, W_1 x)***，其中x为(D,1),W_1为(H,D),W_2为(C,H)  
+ 3-layer：***f = W_3 max(0, W_2 max(0, W_1 x))***，其中x为(D,1),W_1为(H1,D),W_2为(H2,H1),W_3为(C,H2)  
+ 以此类推更多层的形式  

**注**：在实践中，每一层通常还带有一个**bias term 偏差项**，也可以在学习中修正。但是偏差项完整写出来会使表达很杂乱，所以一般**隐藏**，但确实在实践中会使用偏差项。  

其损失与线性分类器类似，为多类SVM损失+每层权重矩阵的正则化系数。  

**可视化**如下：  
![](pictures/CV/vnetwork.png)  
H是神经网络内部的**Hidden layer隐藏层**，通过层间的权重矩阵W沟通，其预示上一层的元素数量以及上下层的关系。  
权重矩阵任意一项W(i,j)意味着**上一层**的j号元素对**下一层**的i号元素的影响。  

另外的，由于神经网络密集的连接方式，若每一层所有的元素都彼此连接，则可以称为**Fully-connected neural network**（全连接神经网络），或者**Muliti layer perceptron**（多层感知器），简写为**MLP**。在这种情况下，模型就回到了之前研究的**perceptron 感知器**模型。详细原理与后面的ReLU等有关。  
也就是说如果层间元素都连接，那么其还是相当于多个线性的叠加，**仍然还是线性**。  

## 神经网络视觉角度

之前提到线性分类器的**视觉**角度，实际上是图像匹配不同类别的模板，**缺点**是无法解决一个类别中不同的样式，泛用型差。  

而在将线性分类器**拓展**到神经网络后，能够解决**视觉**角度的这一问题。  
以**二层**网络为例，其第一层是许多**模板**组成的库，第二层是对不同模板的**重新组合**，这称为**distributed representation**（分布式表示），这样就实现了一个类别多种样式的匹配，能够解决之前“双头马”的问题。  

按视觉角度的方法获取模板，可以发现第一层中的模板库中的内容大多已经无法分析具体含义，或许是**模板**匹配，也或许是图像**边缘**，就像是CV开始时视觉先被定向边缘刺激（当然其中也有训练出的冗余项，后面会学习修剪冗余的方法）。  
从这里开始，神经网络就已经超脱了人类的理解范围了，不知道它会学习出什么，但是它确实能够最大限度的提高精度。  

## Deep Neural Networks 深度神经网络

如图，是一个六层网络：  
![](pictures/CV/deepnetwork.png)  

**深度网络参数**：  
+ **深度**：神经网络的**层数**，也就是网络中可学习的权重矩阵数量
+ **宽度**：每一隐藏层的大小，按理说每一层的大小可能都不一样，**但是**实践中约定俗成的是设置每一层大小都一样。 

## Activation Functions 激活函数

在神经网络的形式部分，发现不同层的权重矩阵之间并**不是直接相乘**，而是要经过一个max的处理，这个max其实是神经网络中**最关键**的一部分，称为**激活函数**，名为**ReLU**(Rectified Linear Unit)。  

假设一个二层网络，若没有这个激活函数，那么网络*f = W1 W2 x*实际上由于**线性**性质，等效于*f = W3 x*，**退化**成为了线性分类器。所以我们需要一个方式来使神经网络真正的起作用，引入**激活函数**，如其名，通过非线性的变换破坏线性关系，才是激活了神经网络。  

事实上，除了ReLU，激活函数其实还有许多形式：  
![](pictures/CV/activation.png)  
每一种选择都有比较特定的应用场景，但是在现在的神经网络中，**ReLU**是一个十分优良的激活函数，是一个默认的好选择。  

## 纠正：神经网络与生物神经

神经网络与神经的确有一些**抽象上**的相似，因此得名：  
在基本单元上，生物神经是由神经元接受其他神经元刺激-->判断-->发出/抑制刺激，而神经网络也是接受上一层输入-->通过激励函数-->输出或置0；  
在整体构成上，生物神经彼此间组成复杂网络，而神经网络不同层级之间组成复杂关系。  

**但是**其实也有诸多区别之处：  
譬如神经元对刺激的接受与处理要比权重与激励函数这样的非线性函数复杂得多，神经元的种类也很多；  
形成的网络有复杂的拓扑结构，包括回环等，而神经网络只能按层分开来方便矩阵向量运算（尽管有随机连接网络的研究）；  

**总而言之**，应该将**神经**这一词视为与真实神经完全不同的概念，回到数学，回到工程。  

## Space Warping 空间扭曲角度

理解神经网络的**强大**之处，可以通过**空间扭曲**的角度研究。  

使用线性分类器时，通过**线性变化**得到的新空间仍然和原空间一致，**线性不可分**的性质仍不会改变：  
![](pictures/CV/linearwarp.png)  
因此使用线性特征变换无法提高模型的能力。  

而使用神经网络时，由于ReLU函数，会将空间进行扭曲，负半轴的数据点都挤在数轴上，拥有了**线性可分**的性质：  
![](pictures/CV/nertualwarp.png)  
对应回原数据集，相当于拥有了非线性的决策边界，因此能力更强。  

**由上可知**，随着神经网络层数的增加，模型会变得越发复杂，决策边界将会十分混乱，这可能会带来**过拟合**，而之前提到的**正则化**这一控制模型复杂度的方法。  
尽管**减少**神经网络层数也可以控制模型复杂度，但一般并不这么做，而是加大**正则化强度**。  

## Universal Approximation 性质：普遍近似

神经网络理论上可以学习**逼近**任何连续函数。  

以一个二层神经网络为例，可以根据原理将其output重新写成**多个**位移、缩放后的**ReLU函数**的和，然后将其**四个一组**，可以组合成一个**bump函数**（就像信号与系统中的开关门函数的构成）：  
![](pictures/CV/bumpfunc.png)  
其斜率、高度等参数都可以通过改变神经网络参数来改变。  
然后再将多个bump函数组合起来，就可以拟合曲线了：  
![](pictures/CV/approximate.png)  

但还留下一些问题，如权重值的学习方式、学习过程、需求数据量等。

## 神经网络最优化：Convex Function 凸函数

**凸函数**：  
数学定义为在区间[0,1]上，f((1-t)\*x1 + t\*x2) <= f((1-t)\*x1) + f(t\*x2)  
而直观上，其实就是像y=x^2这样的，还可以拓展到更高维，**bowl碗形**的函数，就叫做凸函数。  

凸函数可以理论证明局部最优解就为全局最优解；  
**但是**在实践中遇到的损失函数一般都为**nonconvex function 非凸函数**，因此需要在**非凸优化**领域中进行研究。因此非常火热。  

***

# Lec06：Backpropagation 反向传播

对于一些简单的损失函数（如我们的线性模型），我们可以从其形式上解析推导出总损失关于权重矩阵的梯度；  
但是对于其他种类的神经网络或函数，由于过于复杂，**导出解析梯度**十分乏味且**困难**，甚至得不出；同时**拓展性差**，推导一次只能针对这一种情况的梯度，一旦条件之间相互组合、拓展，就需要重新计算。  

## Computational Graphs 计算图

相比于解析推导，作为计算机科学家，设计出更加**泛用**且**模块化**的方法来解决梯度计算问题才是更好的方法，而将计算梯度的过程进行**计算图化**，并使计算机能够自动有效解决这一问题。

### 以简单计算为例

对于计算**f(x,y,z)=(x+y)z**的梯度：  
+ **列出**每一个计算节点，譬如这里的+与x  
+ **Forward pass 前向传播**，用来计算输出值。为x、y、z取随机的值，然后记下每个节点计算后的值  
+ **Backward pass 反向传播**，用来计算梯度。列出想要计算的导数，一般是最后输出与输入的偏导，如df/dx，df/dy，df/dz，然后从最后输出一步一步往前计算局部梯度，要用到**链式法则**  

这样就可以绘制出**计算图**了，如图所示：  
![](pictures/CV/simplegraph.png)  

关于**链式法则**，更科学的表述是如上图解释的“上下游”，例如当计算df/dx时，可以通过节点p，则df/dx称为下游梯度，dp/dx称为局部梯度，df/dp称为上游梯度，**梯度就如此传播**起来了。  

### 单节点角度：感知、封装

沿袭上面关于链式法则的理解，思考对于一个并不很具体的单个原子节点，其自然的由这几部分组成：  
+ input 输入数据：x、y
+ output 输出数据：z
+ function 处理函数：f

试想在一个巨大的网络里，每一个节点对最终梯度的组成其实是**不可感知**的，其只能够接收远处传来的**上游梯度**，在**局部**进行处理，然后回传给**下游梯度**，如下图：  
![](pictures/CV/singleplot.png)  

由此可以获得其绝妙的性质：  
任意一节点并不能对最终的整体梯度有感知，其只能处理上游、局部、下游的梯度，那么由所有这些任意节点组成的网络就是**模块化封装**的，可以任意组合、变换，并且计算效率仍旧很高。  

我们可以自定义地将一些原子节点组合在一起，成为一个模块，从而实现整体简化。  
譬如激励函数Sigmoid，对于输入的x要经过\*-1、exp、+1、1/x的操作，但由于sigmoid经常性的使用，所以将其整个视为一个**模块**，能模块化精简运算  

### 角度：看作“gate 门”

思考在正向传播与反向传播的过程中，数值正向流动，梯度反向传播，信息就在其中双向流动。  
那么其实可以思考电路中的一个个“门”，这些基本单元也可以看作是这样的结构，并能找到其中的一些相互关系，如下图所示：  
![](pictures/CV/gates.png)  
+ **add gate 加法门**：在梯度的反向传播中，可以发现加法节点回传的梯度与自己相同，则可以称其为**梯度distributor**，复制自己，回传  
+ **copy gate 复制门**：有时候一个数值要在整个传播流中多次使用，在计算图中可以用一个copy节点来复制自己，则在梯度回传中，下游梯度将会是两个上游梯度的和。  
从**对称性**的角度思考，会发现加法门与复制门有着精妙的对称性，相互的数值传播与梯度传播道理相同。  
+ **mul gate 乘法门**：观察z=xy，会发现局部梯度为dz/dx=y，dz/dy=x，正好起到了“交换”的作用，所以称为**交换乘法**  
+ **max gate 最大门**：很显然的，使用max函数会使两个下游梯度一个为0，这在规模大时会使大面积的梯度变为0，所以对于不使用max有种偏好。  

通过这种抽象角度理解计算图，会有一种新体验。  

## 实际编码

### Flat Backprop 平面反向传播

我们在编码计算梯度时，完全可以使用反向传播，倒着写下推导loss的过程就可以。  
当这种方式熟练后，完全可以不再手动推导梯度。  

但这种方式其实在实现单元测试等方面上很**失败**，因为与原整体耦合性很强，我们一旦略微更改了模型，或者正则化系数时，就需要重新构造，没能体现反向传播的模块化。  

### API Backprop 调用API进行反向传播

可以自定义一些标准化的运算模块，成为便于我们利用的API。定义过程会有一些标准化方式。  
通常会在一个类中定义成对的前向传播与后向传播。  

## 向量的反向传播

### 求导

从*x改变一点（或x的每一个元素）会对y改变多少*角度思考
+ **常数y**对**常数x**求导，是常数
+ **常数y**对**N维向量x**求导，是向量，形状与向量相同，称为**梯度 gradient**
+ **M维向量y**对**N维向量x**求导，是NxM维矩阵，称为**Jacobian**，雅可比有一个特性是只有对角元素是非负的，其他都是0。

### 每个计算节点

正如前面提到的，不应该将计算图视为一个整体，将其拆解为一个个gate来理解。  
如下图，其实就是在原标量的基础上改成了向量形式，含义更为丰富，在torch中基本都是如此。  
![](pictures/CV/singleplotV.png)  
+ 损失L仍然是一个标量scalar；
+ 输入x是Dx维向量，y是Dy维向量；
+ 输出z是Dz维向量；
+ 上游回传回来的dL/dz是Dz维；
+ 局部变量dz/dx是Dx\*Dz矩阵，dz/dy是Dy\*Dz矩阵；
+ 下游梯度自然。  

### 关于Local Jacobian的优化

由雅可比矩阵的性质，可以发现它是**sparse 稀疏**的，所以在计算时按照矩阵进行计算是十分**浪费**的。  
所以有一些**定制化的方法**将其优化，不按照矩阵来算，能够极大的节省资源。  

譬如**ReLU**，可以由其性质发现，输入与回传向量是同形状的，而下游梯度中为0的项是由输入中小于零决定，所以直接对应，置零即可。（事实上，在完成作业时，为使用掩码mask）  

## 张量（矩阵）的反向传播

与向量的类似，只是本地梯度会有些微妙：  
![](pictures/CV/singleplotT.png)  
直接的形状是一个**矩阵x矩阵**的矩阵，显然只是一个抽象的概念，在实际上应该是将原矩阵展平成为一个维度。  

### 实际优化：切片算梯度

设想这样一个节点：  
+ 输入x为[NxD]矩阵，w为[DxM]矩阵
+ 本地运算为x @ w
+ 输出y为[N,M]矩阵

按照实际的规则，本地梯度dy/dx将会是[(NxD),(N,M)]矩阵，dy/dw将会是[(DxM),(NxM)]矩阵。  
代入训练时常用的一个batch：N=64，D=M=4096，则每一个使用fp32的jacobian将会占用约256GB的内存，这在显存上是可怕的。**所以依然需要一种简化的实现方式**来完成。  

这就是切片算局部梯度的**一部分**，再**合起来**的方法：  
![](pictures/CV/tensorback.png)  
+ 计算dL/dx=dy/dx \* dL/dy时，dy/dx这一**本地梯度**不好求  
	那么则计算其**每一部分**，譬如dL/dx11=dy/dx11 \* dL/dy
	+ dy/dx11可以看作是与y相同形状，且是对应项与x11的导数，
	+ 如第一项为dy11/dx11,有y11 = x11\*w11 + x12\*w21 + x13\*w31，所以dy11/dx11 = w11  
	+ 如此类推，可以填充第一行：dy12/dx11=w12,dy13/dx11=w13,dy14/dx11=w14
	+ 第二行则不一样：dy21/dx11中，y21 = x21\*w11 + x22\*w21 + x23\*w31与x11无关，所以dy21/dx11、dy22/dx11等都为0  
	由此得到dy/dx11实际上是第一行为w的第一行，其余为0的与y型号zua那个相同的矩阵  
+ 继续由此规律，有dy/dx_mn，是一个与y形状相同的矩阵，其中，只有第m行有数值，并且数值是w的第n行，剩下的均为0。  
+ 这样，实际上的dL/dx_mn就可以转换为一个内积：**dL/dx_ij = dy/dx_ij * dL/dy = (w_j:) dot (dL/dy_i:)**  

由上述结论再次推导，可以得到结论：  
对于节点y=xw，有：**dL/dx = (dL/dy) @ W_t**  
（事实上，这个组合是唯一能够拼出所需要的张量形状的，因为其必须包括上游梯度与另一个输入，只能这样组合）  
还有：**dL/dw = w_t @ (dL/dy)**  
推导不出来就看.shape()吧！  

## 梯度传播链角度

我们计算损失是按照特定的顺序的：X0-(f1)->X1-(f2)->X2-(f3)->X3-(f4)->L，那同样的，在计算反向传播时，是反着沿着这个链条逐项计算的。但实际上由于矩阵乘法的结合律，只要合适的安排顺序，计算出的结果都是相同的，所以就有**传播链**的说法。

### Reverse-Mode 反向传播模式

我们一般用到的就是反向传播链。先从输入一直计算到输出，再从最终损失的标量出发，逐步往前，好处是可以直接一路矩阵乘法，最后得到答案；但是倘若需要从起点一边推导梯度，一边获取某步骤的梯度，就没有办法了。  

### Forward-Mode 前向传播模式

直接从代表损失的标量出发，一步一步往前推导。  
这一模式对于模拟真实世界很有用，譬如仿真物理世界中的某一个参数发生变化，剩下所有过程随之变化的过程。  
所以这一过程远远超出机器学习的应用范围，所有科学计算都可应用这一模式。  
但可惜的是，pytorch与tensorflow都不支持这一模式，但有一个代数技巧，利用两次反向传播来实现前向传播，不高效，但优雅。  

## 高阶求导

有时我们不光想要求dL/dx，更想要计算高阶的，以展现随着变量变化，趋势会怎样，也就是d2L/dx2。  

如果dL/dx是D0维向量，那么d2L/dx2是[D0,D0]矩阵，称为**Hessian 海塞矩阵**。  

当进行**Hessian x vector**时，譬如d2L/dx2 @ v时，牢记线性是巨大力量的，所以可以写为d[dL/dx @ v]/dx，是梯度与向量内积关于x的梯度，当且仅当v与x无关时。  

由此可以根据高阶梯度来扩充我们的计算图。  
![](pictures/CV/higherorder.png)  
这一点在pytorch等框架上有实现。  

在具体应用上，有“Regularization to penalize the norm of the gradient”  

***

# Convolutional Networks

迄今，我们所有的工作都是基于将图片展平为一个大的向量，这一过程并不尊重输入图像的二维空间结构，这是不可接受的。者需要我们定义一种新的基于图像数据计算方式。利用卷积，可以实现保留像素之间的位置。  

## 全连接网络 VS 卷积网络

之前的全连接网络需要**Fully-Connected Layers 全连接层**与**激活函数 Activation Function**；  
而卷积网络有新的操作：**Convolution Layers 卷积层**、**Pooling Layers 池化层**与**Normalization 归一化**。  

## 卷积层 Conv

### 基本构成：输入、卷积盒、bias、输出

**全连接**层的输入是压平后的原始图像，输出隐藏层大小向量或最终得分向量；而**卷积层**的输入是不经处理的原始图像，有**长、宽、通道**三个维度，还有一个**Convolve 卷积盒**（或者叫滤波器filter），有**长、宽、通道**三个维度，通常是方形的，并且通道数需要对应匹配。  
计算时用卷积核在图像上滑动并进行点积操作。最后输出特定大小的一组数据，称为**activation maps 激活图**。  
并且卷积层内往往会有多组filter，最后得到的也是数量相匹配的一组激活图像。  
值得一提的是，同样会带有**偏置项 bias**需要考虑，每一个filter都对应一项偏置。  
最后，实际使用时的输入往往是一组图像，因此输出也要聚合成一组，最后的卷积层关系如下图所示。  
![](pictures/CV/convolution.png)  

### 卷积堆叠

在此之上，自然还有**Stacking Convolutions 卷积堆叠**，也就是多个卷积层堆叠起来。  
就和全连接神经网络一样，每层之间的结果都需要一个**激活函数**，比如ReLU，否则多个卷积操作直接等效于一个卷积操作，需要加入一个非线性操作。  

### 直观视觉模板

就像在线性分类器与MLP中，将第一层的图像展示出来，近似于一类的“模板”，卷积层也可以这样提取出模板图像，如下图：  
![](pictures/CV/contemp.png)  

+ 不同于之前的两类模板与原图等大，卷积网络的图像是一个个**局部的**、**简单的**图像
+ 这些模板包括各个方向上的**边缘**，还有**对立的颜色**，与CV之起点的神经识别一脉相承

### 空间角度超参数

#### padding：填充

每经过一个卷积层，图像就会**shrink**，具体规律是：*输入W，卷积核K，输出W-K+1*。  
可见这样会导致卷积神经网络的深度不会很深，并不利于深度学习。解决方法就是**padding 填充**：  
在卷积操作前，先填充原图像的边框的**额外像素**，并使其值为0（这称为0填充，在不同使用情景下可以采用不同的填充策略，譬如最近邻居值等，但最常用的是0填充）。  
由此就有**新规律**：*输入W，卷积核K，padding为P，输出W-K+1+2P*。  
通常令P=(K-1)/2，这样能使**输入与输出同维度**，成为**same padding**。  

0填充实际上不会带来任何信息（但是事实上会破坏translation与variance），只是一种符号方便，对抗卷积网络向内部收缩的趋势，使输入输出同维；加0实际上不知道是一个bug还是feature，但是某种意义上会增加网络的代表性力量，增加泛用性（根据老师的看法）。  

#### strided：步幅

首先引入一个**receptive field 感受野**的概念。见下图：  
![](pictures/CV/receptive.png)  
（为什么不直接用一个7的卷积核？如上所述，每层都要有一个激活函数使其非线性）  
通过四层卷积，每层卷积核大小为3，最后输出中的一个像素对应输入中的7\*7像素，这一范围就称为**感受野**。  
分析可以发现，感受野的大小与网络的层数呈**线性关系**，也就是说，为了让最后输出中的感受野足够大，就需要足够深的层数才能“看”到整个图像，这样的效率并不高。  

所以引入超参数**步幅**。  
步幅实际上就是卷积核每次移动的距离，在之前的情况里，步幅算是1，一旦将其变为2，那么就可以认为在层数一致的情况下，建立感受野的速度翻倍。  
更一般的，有计算方法：*输入W，卷积核K，padding为P，stride为S，输出(W-K+2P)/S + 1*。
（***这里的式子不太明白，后面有空记得推导***）  

#### 1x1卷积意义

看起来有些奇怪，但其实是有一定意义的。  
相当于卷积神经网络中的“全连接层”，在空间中的每一个特征向量上独立运作；  
也可以认为是改变通道这一维度数量的方式，不同于真正的全连接层，会摧毁空间结构，所以一般只用在最后一层来输出分数。  
（***其实现在还不懂，以后实践再研究罢***）  
所以在实践中经常使用。  

### 常用卷积层参数

![](pictures/CV/convpara.png)  

### 补充：其他卷积类型

上面提到的是**二维卷积**，事实上，可以拓展到其他维度，拥有不同的解释。  
在pytorch中也可以进行对应的设置。  

+ **1D Conv**：  
相当于在一个维度上按照给定的宽度不断取平均；  
可以用于按照序列输入的文本，或者音频数据。  

+ **3D Conv**：  
其实同理。  

## 池化层 Pooling

**Pooling**是另一种**Downsample**的方式，和先前调整步幅的**Downsample**都是不涉及可学习的过程的计算过程，为了减小维度与增大感受野，参数只有**kernel size**、**strip**与**Pooling func**。  

### 最常用：Max Pooling

取kernel中的最大值来替代原kernel中的所有。如图：  
![](pictures/CV/maxpooling.png)  

其为模型带来了一定的**抗干扰能力**，更加鲁棒，因为如果图像中的某物体进行了小幅度的移动，但对应的最大值并没有发生变化。  

### 另一种：Average Pooling

kernel内求平均即可  

### 池化总结

池化有三个参数需要设置：**kernel size**、**strip**与**Pooling func**。  
经常使用的是**max pooling**。  

常用的池化设置如下：  
![](pictures/CV/poolingpara.png)  

## 卷积神经网络 Convolutional Networks

在目前掌握的**全连接层 FC layer**、**激活函数**、**卷积层 Conv**、**池化层 Pooling**后，就可以构建出一些经典的**卷积神经网络**了。  

经典的网络结构基本都为：  
若干*[Conv, ReLU, Pool]*构成的卷积层->展平->若干*[FC, ReLU]*构成的全连接层->最后作为输出层的FC->分数  

### LeNet-5

出现在1998年，用于**字符识别**，输入28\*28的灰度图像，输出10个分数。  
具体的各层网络细节如下：  
![](pictures/CV/lenet.png)  

可以发现一个**有趣之处**：当沿着网络走时，会发现每层**单独输入的大小**（空间尺寸）会通过池化或步长减小，但是相应的，**通道数**会增加，也就是**总体积**会以某种方式保留下来，只不过被捏成了另外形状。  

## 归一化层 Normalization

上述的经典卷积神经网络有一个**缺点**：深度过深，在大数据集上训练十分困难。  
为解决这一问题，在网络内部增加了**归一化层**，这有助于克服上述困难，训练深层网络。  

经常使用的归一化方式是**Batch Normalization**。  

### 归一化原理

每一层都**接收前一层的输出**，并将其按照某种方式将其**规范化**，使其均值为0，并且有单位方差分布。  

按照提到的论文，这一处理减小了“**internal covariate shift**”（内部协变量移位？），粗浅的理解是：在训练深层神经网络时，每一层都在see上一层的输出，也就是说在训练的过程中，随着上一层中权重矩阵的不断优化，其输出在不断变化，因此影响当前层的输入、进而影响输出。这种变化在某种程度上并不利于模型的优化，因此要尽量减小。  

而一个自然的思路就是**标准化所有层**，使其都适于一些目标分布，所以强制每一层都满足均值为0，并且有单位方差分布，这会有利于稳定或加速优化过程。  

### Batch Normalization

就是**减去平均值，除以标准差**。  
这构造出一个可微函数，因此可以直接加到计算图中（计算图的一个性质，可微，或者只有有限位置不可微，都能够加入到计算图中成为一个单元）。  

假设输入为x=N\*D，是一组（batch）、总共有N个数据，而每个数据又有D维的特征或者channel  
计算每个channel的平均值u（形状为D）、方差o^2（也为D），最后计算出归一化后的结果：***(x-u)/✓(o^2+e)***，其中e是一个超参数，为了避免除零。  
这样就得到了batch归一化后的结果。  

**问题1：在均值为0的约束下，拥有单位方差十分困难**（不明白）  
因此在实践中的输出还要再加上额外的操作，增加可学习的参数*gama*与*beta*，均为D维向量，使每一个输出都经过*y_ij = gama_j xhat_ij + beta_j*的变换，这使网络能够自己学习期望的均值和方差。  
值得一提的是，当gama=o、beta=u时，就可以复原为原函数。  

**问题2：在训练时能够计算数据，而测试时不能**  
在之前的训练中，一组数据里面的每个都是独立的，并不会因为其他数据变化而影响自身分数。  
但是在归一化中则不同，由于参数都是根据一批数据决定的，所以在训练中可以用，但是在测试或实战时并不能再根据一批的值来灵活调整，一方面是数据量，另一方面是测试集是不可见的。  
所以会在测试集使用训练集中跟踪并存储的某些数据的均值来替代，其可以代表测试、实际情况。  
还有一点，当测试集直接取这些历史均值后，整个过程就是一个线性运算，开销很小，也就是在最后使用时用极小开销完成了目标。  

### 扩展到卷积网络

之前是全连接网络，拓展到卷积神经网络中，保留的N是通道数，所以之前有提到通道。  

### prop&corn

使神经网络更容易训练，可以用更大的学习率，由于归一化使鲁棒性强，等等；  
理论理解困难，在训练集和测试集表现十分不同，是bug的一大来源。  

### 另一种：Layer Normalization

不同于batch是跨元素之间同一维度间求均值与方差，这种方式是基于自身所有维度求均值与方差，使得元素个体又变得独立。  
多用于RNN，Transformers。这个就能做到训练集与测试集行为一致了。  

### 又另一种：Instance Normalization 实例归一化

比layer稍弱，layer是一个元素内部全部求平均，也就是所有通道都在一起算；  
而instance是将不同通道分开算的。  
多用于风格迁移、图像生成。

有这么一个可视图：  
![](pictures/CV/normalization.png)  
说明了不同的特点，batch是单通道、跨元素的，layer是多通道、单元素，而instance是单通道、单元素，是平均的。  
注意到还有一种新式的group，是介于instance与layer之间的改良，自定义要融合哪些通道。效果也很好，譬如物体检测。  

***

# CNN Architectures

***

# Recurrent Netural Networks






***

# 在校课程

## Lec1-4 

这几节上课未能记下一些有效的东西。  
应该记点的，十分愧疚，相当于浪费了时间。  

## Lec5

### O3的ImageGPS与VQA

### 莫拉维克悖论

无意识的感知等，对于人来说简易的事，对于机器十分难以实现；  
推理等对于人困难的，对机器十分简易。  

### AlexNet卷积部分运算更复杂

### 深层网络性能反而差

通过简单堆砌网络层数，会发现性能反而差，可能会误用正则化，以为是发生过拟合，但其实不是，是应该用优化工具，因为深层网络的监督信号（实为梯度）反向传播要从最深层穿越深层网络才能到第一层，中间的损耗。  
ResNet就在解决这一问题。（21世纪引用次数最多）  
残差路径很多，砍掉一部分，skipconnection，能部分实现非凸的优化
