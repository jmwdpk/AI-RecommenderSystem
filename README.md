# AI-RecommenderSystem
该仓库尝试整理推荐系统领域的一些经典算法模型，主要包括传统的推荐算法模型和深度学习模型， 并尝试用浅显易懂的语言把每个模型或者算法解释清楚！此次整理依然是通过CSDN博客+GitHub的形式进行输出， CSDN主要整理算法的原理或者是经典paper的解读， 而GitHub上主要是模型的复现和实践。实验所用的数据集如下：
* [Movielens](https://github.com/ZiyaoGeng/Recommender-System-with-TF2.0/wiki/Movielens)
* [Amazon Dataset](https://github.com/ZiyaoGeng/Recommender-System-with-TF2.0/wiki/Amazon-Dataset)
* [Criteo Dataset](https://github.com/ZiyaoGeng/Recommender-System-with-TF2.0/wiki/Criteo-Dataset)

模型的原理部分， 可以参考我的博客链接[推荐系统学习笔记](https://blog.csdn.net/wuzhongqiang/category_10128687.html)
# 1. 传统的推荐算法模型

![传统推荐模型演化关系图](imgs/传统推荐模型演化关系图.png)

## [1.1 协同过滤算法](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/CollaborativeFiltering)
协同过滤算法， 虽然这个离我们比较久远， 但它是对业界影响力最大， 应用最广泛的一种经典模型， 从1992年一直延续至今， 尽管现在协同过滤差不多都已经融入到了深度学习，但模型的基本原理依然还是基于经典协同过滤的思路， 或者是在协同过滤的基础上进行演化， 所以这个算法模型依然有种“宝刀未老”的感觉， 掌握和学习非常有必要。<br><br>所谓协同过滤(Collaborative Filtering)算法， 基本思想是**根据用户之前的喜好以及其他兴趣相近的用户的选择来给用户推荐物品**(基于对用户历史行为数据的挖掘发现用户的喜好偏向， 并预测用户可能喜好的产品进行推荐)， **一般是仅仅基于用户的行为数据（评价、购买、下载等）, 而不依赖于项的任何附加信息（物品自身特征）或者用户的任何附加信息（年龄， 性别等）**。目前应用比较广泛的协同过滤算法是基于邻域的方法， 而这种方法主要有下面两种算法：
* **基于用户的协同过滤算法(UserCF)**: 给用户推荐和他兴趣相似的其他用户喜欢的产品
* **基于物品的协同过滤算法(ItemCF)**: 给用户推荐和他之前喜欢的物品相似的物品

所以这一块知识的主要内容是UserCF和ItemCF的工作原理和代码实践， 工作原理部分详情见我写的博客, 代码实践是把项亮推荐系统实践里面的协同过滤算法(UserCF和ItemCF)实现了一遍， 并进行详细的解释和说明<br>
* 优点： 可解释性强， 直观， 简单
* 缺点： 泛化能力差， 没有用到用户， 物品和上下文特征

筋斗云：[AI上推荐之协同过滤](https://blog.csdn.net/wuzhongqiang/article/details/107891787)

## [1.2 隐语义模型与矩阵分解](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/LFM_MF)
 协同过滤的特点就是完全没有利用到物品本身或者是用户自身的属性， 仅仅利用了用户与物品的交互信息就可以实现推荐，是一个可解释性很强， 非常直观的模型， 但是也存在一些问题， 第一个就是处理稀疏矩阵的能力比较弱， 所以为了**使得协同过滤更好处理稀疏矩阵问题， 增强泛化能力**， 从协同过滤中衍生出矩阵分解模型(Matrix Factorization,MF), 并发展出了矩阵分解的分支模型。在协同过滤共现矩阵的基础上， 使用更稠密的隐向量表示用户和物品， 挖掘用户和物品的隐含兴趣和隐含特征， 在一定程度上弥补协同过滤模型处理稀疏矩阵能力不足的问题。
 
 这一块的知识主要是隐语义模型的含义， 矩阵分解算法的原理和矩阵分解算法的计算方法， 具体内容我已经写到了博客上面。<br>
* 优点： 泛化能力强， 空间复杂度低， 更好的扩展性和灵活性
* 缺点：依然没有用到用户， 物品和上下文特征
 
 筋斗云：[AI上推荐之隐语义模型(LFM)和矩阵分解(MF)](https://blog.csdn.net/wuzhongqiang/article/details/108173885)
 
 ## [1.3 GBDT+LR模型](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/GBDT%2BLR)
 这是2014年Facebook提出的一个模型，协同过滤和矩阵分解同属于协同过滤家族， 之前分析过这协同过滤模型存在的劣势就是仅利用了用户与物品相互行为信息进行推荐， 忽视了用户自身特征， 物品自身特征以及上下文信息等，导致生成的结果往往会比较片面。 而今天的这两个模型是逻辑回归家族系列， 逻辑回归能够综合利用用户、物品和上下文等多种不同的特征， 生成较为全面的推荐结果。GBDT+LR模型利用GBDT的”自动化“特征组合， 使得模型具备了更高阶特征组合的能力，被称作特征工程模型化的开端。
<br><br>**模型原理**： GBDT是一种常用的非线性模型，基于集成学习中boosting的思想，由于GBDT本身可以发现多种有区分性的特征以及特征组合，决策树的路径可以直接作为LR输入特征使用，省去了人工寻找特征、特征组合的步骤。所以可以将GBDT的叶子结点输出，作为LR的输入，如图所示：

 ![](https://camo.githubusercontent.com/e47281e79031a53c4616ed5be48c1342dcf382da/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f343135353938362d386134636235306165666261323837372e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f353038)
 
 这一块的重要知识点是LR模型的原理， GBDT模型的原理和细节， 具体内容我已经整理到了博客
 * 优点： GBDT与LR模型互补， 实现了特征工程的自动化模式， 且增加了特征交叉组合的能力， 能够利用更加高阶的特征
 * 缺点： 容易过拟合， 对于调参带来了难度
 
 筋斗云：[AI上推荐 之 逻辑回归模型与GBDT+LR(特征工程模型化的开端)](https://blog.csdn.net/wuzhongqiang/article/details/108349729)<br>

## [1.4 FM+FFM模型](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/FM_FFM) 
FM模型是2010年提出， FFM模型是2015年提出， 这两个属于因子分解机模型族， 在传统逻辑回归的基础上， 加入了二阶部分， 使得模型具备了特征组合的能力，逻辑回归是一个简单、直观、应用的模型， 但是局限性就是表达能力不强， 无法进行特征交叉和特征筛选等， 因此为了解决这个问题， 推荐模型朝着复杂化发展， GBDT+LR的组合模型就是复杂化之一， 通过GBDT的自动筛选特征加上LR天然的处理稀疏特征的能力， 两者一结合初步实现了推荐系统特征工程化的开端。 其实， 对于改造逻辑回归模型， 使其具备交叉能力的探索还有一条线路， 就是POLY2->FM->FFM， 这条线路在探索特征之间的两两交叉， 从开始的二阶多项式， 到FM， 再到FFM， 不断演化和提升。

这一块的重要知识点是了解POLY2->FM-FFM的演化历程， FM的原理和FFM的原理细节。 具体内容我已经整理到了博客。
* 优点： 在逻辑回归的基础上有了自动的二阶交叉能力， 使得模型的表达能力进一步增强
* 局限： 由于组合爆炸问题的限制， 模型不能很好的进行高阶特征交叉

 筋斗云：[AI上推荐 之 FM和FFM(九九归一)](https://blog.csdn.net/wuzhongqiang/article/details/108719417)<br>

# 2. 深度学习的浪潮之巅
![](imgs/深度学习模型演化关系图.png)

## [2.1 DeepCrossing模型](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/Deep_Crossing)
该模型是2016年微软提出的一个模型， 是一次深度学习框架在推荐系统中的完整应用， 该模型完整的解决了从特征工程、稀疏向量稠密化， 多层神经网络进行优化目标拟合等一系列深度学习在推荐系统中的应用问题， 为后面研究打下了良好的基础。 该模型的结构如下：

![](https://img-blog.csdnimg.cn/2020100916594542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1emhvbmdxaWFuZw==,size_1,color_FFFFFF,t_70#pic_center)

这就是DeepCrossing的结构了， 比较清晰和简单， 没有引入特殊的模型结构， 只是常规的Embedding+多层神经网络。但这个网络模型的出现， 有革命意义。 DeepCrossing模型中没有任何人工特征工程的参与， 只需要清洗一下， 原始特征经Embedding后输入神经网络层， 自主交叉和学习。 相比于FM， FFM只具备二阶特征交叉能力的模型， DeepCrossing可以通过调整神经网络的深度进行特征之间的“深度交叉”， 这也是Deep Crossing名称的由来。

筋斗云：[AI上推荐 之 AutoRec与Deep Crossing模型(改变神经网络的复杂程度）](https://blog.csdn.net/wuzhongqiang/article/details/108948440)<br>
## [2.2 NeuralCF Model](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/NeuralCF)
Neural CF是2017年新加坡国立大学的研究人员提出的一个模型， 提出的动机就是看着MF的内积操作比较简单， 表达能力不强， 而此时正是深度学习的浪潮啊， 所以作者就用一个“多层的神经网络+输出层”替换了矩阵分解里面的内积操作， 这样做一是让用户向量和物品向量做更充分的交叉， 得到更多有价值的特征组合信息。 二是引入更多的非线性特征， 让模型的表达能力更强。 模型如下：

![](https://img-blog.csdnimg.cn/20201019200457212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1emhvbmdxaWFuZw==,size_1,color_FFFFFF,t_70#pic_center)

该模型的优缺点：
* 优点： 表达能力加强版的矩阵分解
* 局限： 只用了用户和物品的id特征， 没有加入更多其他特征

筋斗云： [AI上推荐 NCF模型](https://blog.csdn.net/wuzhongqiang/article/details/108985457)

## [2.3 Product-based Neural Networks](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/PNN)
该模型是2016年上海交大团队提出的一个模型， PNN模型在输入、Embedding层， 多层神经网络及最后的输出层与DeepCrossing没有区别， 唯一的就是Stacking层换成了这里的Product层， 结构如下：

![](https://img-blog.csdnimg.cn/20201019225606860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1emhvbmdxaWFuZw==,size_1,color_FFFFFF,t_70#pic_center)

该模型的提出是为了研究特征之间的交叉方式， DeepCrossing模型是加深了网络的层数， 但是不同特征的embedding向量它统一放入了一个全连接层里面去学习， 这可能会丢失掉一些信息， 比如可能有特征之间一点关系也没有， 有特征之间关系非常相似， 这种在DeepCrossing之中是没法学习到的。 所以PNN 模型用了Product层替换了原来的stacking层， 在这里面主要就是两两特征的外积和内积交叉。这一块的内容我已经更新到博客：
* 优点： 提高特征交叉能力， 使得模型学习特征有了针对性
* 局限： “外积”操作一定程度上会影响表达能力

筋斗云： [AI上推荐 PNN模型(改变特征交叉方式）](https://blog.csdn.net/wuzhongqiang/article/details/108985457)

## [2.4 Wide&Deep Networks](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/WideDeep)
这是谷歌在2016年提出的一个经典模型， 该模型在深度学习的模型中处在了非常重要的地位，它将线性模型与DNN很好的结合起来，在提高模型泛化能力的同时，兼顾模型的记忆性。Wide&Deep这种线性模型与DNN的并行连接模式，后来成为推荐领域的经典模式， 奠定了后面深度学习模型的基础。具体的结构如下：
![](https://img-blog.csdnimg.cn/20201026181206978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1emhvbmdxaWFuZw==,size_1,color_FFFFFF,t_70#pic_center)
该模型取得成功的关键在于它的两个特色：
* 抓住业务问题的本质特点， 能够融合传统模型的记忆能力和深度模型的泛化能力
* 结构简单， 容易在工程上实现，训练和部署

筋斗云：![AI上推荐 之 Wide&Deep与Deep&Cross模型(记忆与泛化并存的华丽转身）](https://blog.csdn.net/wuzhongqiang/article/details/109254498)

## [2.5 Deep&Cross NetWork(DCN)](https://github.com/zhongqiangwu960812/AI-RecommenderSystem/tree/master/DeepCross)
这是2017年， 斯坦福大学和谷歌的研究人员在ADKDD会议上提出的模型， 该模型针对W&D的wide部分进行了改进， 因为Wide部分有一个不足就是需要人工进行特征的组合筛选， 过程繁琐且需要经验， 2阶的FM模型在线性的时间复杂度中自动进行特征交互，但是这些特征交互的表现能力并不够，并且随着阶数的上升，模型复杂度会大幅度提高。于是乎，作者用一个Cross Network替换掉了Wide部分，来自动进行特征之间的交叉，并且网络的时间和空间复杂度都是线性的。 通过与Deep部分相结合，构成了深度交叉网络（Deep & Cross Network），简称DCN。模型结构如下：
![](https://img-blog.csdnimg.cn/20201026193641246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1emhvbmdxaWFuZw==,size_1,color_FFFFFF,t_70#pic_center)

Deep&Cross的设计思路相比于W&D并没有本质上的改变，但是Cross交叉网络的引进使得模型的记忆部分的能力更加强大了。具体的可以参考博客：<br><br>
筋斗云：![AI上推荐 之 Wide&Deep与Deep&Cross模型(记忆与泛化并存的华丽转身）](https://blog.csdn.net/wuzhongqiang/article/details/109254498)
# 参考：
* 项亮-《推荐系统实践》
* 王喆-《深度学习推荐系统》
* [https://github.com/BlackSpaceGZY/Recommender-System-with-TF2.0](https://github.com/BlackSpaceGZY/Recommender-System-with-TF2.0)
* [https://github.com/shenweichen/DeepCTR](https://github.com/shenweichen/DeepCTR)
* 相关论文： [https://github.com/zhongqiangwu960812/ReadPapaers/tree/master/RecommendSystem](https://github.com/zhongqiangwu960812/ReadPapaers/tree/master/RecommendSystem)
