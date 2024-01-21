# LLM for Robotics

## TidyBot 

* 核心：LLM 的**总结能力**与**个性化机器人**的泛化需求非常匹配。LLM 展示了通过总结实现泛化的惊人能力，利用从海量文本数据集中学习到的复杂对象属性和关系。

  * 利用文本数据中学习的总结能力

  * 不同于需要昂贵的数据收集和模型训练的传统方法

---

#### 相关工作：

* Household cleanup：Embodied AI的很多工作都提出了解决这个问题的基准或框架。之前的工作需要给每个对象**手动指定目标位置**。后续一些工作根据人类的通常偏好选择解决了这一问题，但这也会导致不能个性化摆放，所以有些工作**通过收集大量数据**，然后预测用户的喜好来选择摆放位置，而本文没有收集额外数据，只使用了大语言模型。

* Object sorting：以前的方法是根据物体的**具体属性**（比如颜色，形状，大小等），通过聚类、主动学习、度量学习、启发式搜索等方法进行排序，但这些方法都无法根据**语义或者常识**进行分类，而本文使用了LLM使用常识知识对推理对象进行分类。

* LLMs for robotics：这些都希望LLM输出的是**通用的计划设置**，而不是个性化的。本文会根据用户的偏好生成个性化的计划。

#### 方法

* 先让用户给出一个例子，然后通过LLM总结一下用户的喜好，之后根据总结的喜好进行选择。
  * 研究人员首先测试了一个基于文本的基准数据集²，其中**输入了用户偏好**，并要求模型创建**个性化规则**来确定物品归属。模型将示例总结为一般规则，并使用总结来确定新物品的放置位置。
  * 基准场景定义在四个房间中²，每个房间有 24 个场景。每个场景包含两到五个放置物品的地方，并且有相同数量的已见和未见物品供模型分类。

* 硬件构成
  * 机械臂Kinova Gen3 7-DoF arm，Robotiq 2F-85 parallel jaw gripper
  * two overhead cameras
  * AGV自动导引车（3-degree-of-freedom motion）。four caster wheels

* 实现步骤

  ![image-20231122103509499](D:\My_desktop\Blog备份\Paper_reading\LLM_tidybot.png)

  * two overhead cameras for 2D robot pose estimation (x, y, θ) and 2D object localization (x, y).
  * 用了ViLD网络目标检测来识别有哪些物体
  * 对于最近的检测物体，将这个物体所在的矩形框图片（图像）和之前LLM summarized的物体概括（文本），通过CLIP进行embedding之后，计算余弦相似度获取和这个物体最匹配的**物体概括**，从而确定这个物体属于哪一类。
    * **抽象的物体概括**：如果不概括，那么就需要人实现物体列表，泛化能力会下降。
  * 有了这个物体的概括之后，然后再通过LLM，来确定这个物体应该放到哪（"recycling bin", "plastic storage box", "black storage box", "sofa", "drawer"）以及以怎样的方式放（place、toss）。

* 整体涉及到的模型（**目标检测模型**ViLD、**图像文本比对模型**CLIP、**大语言模型**）
  * 利用了CLIP将机器人感知系统获取到的图像，和大语言模型生成的文本进行了**多模态的桥接**，然后再利用大语言模型的规划能力**生成高层控制代码**，实现完整的自动化流程。

（补充：LLM机器人任务规划：ChatGPT输出的代码，本质上是对输入的prompt进行理解*，*然后结合其训练的海量知识（这里使用的是生活常识），然后对给定的API进行代码组合，生成一套完整的**能够完成任务的规划**。这样一套框架其实相当**有扩展性**的API可以补充，只要机器人本体能够实现这些功能（主要是**感知**、**控制**）。那么通过LLM，可以很好地用prompt，理解人类意图并根据提供的API，构建**规划**，从而形成一个逐步走向智能的执行单元。）

#### 实验

后面的实验比较了几种方法：

1. 只给用户喜好的例子，不总结
2. 根据WordNet的分类标准
3. 根据RoBERTa的文本编码
4. 根据CLIP的编码
5. 总结（本文方法）

![image-20231122104259633](D:\My_desktop\Blog备份\Paper_reading\LLM_tidybot_com.png)

第二组实验比较了本文方法的几个组件对性能的影响（示例方面，只使用常识，不使用偏好）（摘要方法，替换成人工摘要）

* 不了解偏好准确率大幅下降

* 将摘要换成人工摘要准确率会带来6%的性能提升（也就是从摘要方面性能可以接着提升）

* 还比较了不同的大模型，以及真实场景的测试

  ![image-20231122104436419](D:\My_desktop\Blog备份\Paper_reading\LLM_tidybot_com2.png)

#### 缺陷：

1. LLM的摘要不够好
   * simply **lists** out the seen objects rather than summarizing into categories
   * **summarizes receptacles** by grouping them together

2. 真实世界系统：对真实世界进行了简化，面对复杂场景不能很好工作，可能考虑添加高层次的规划（比如先清除出一条道路，再去抓取物体）

* hand-written manipulation primitives
* use of top-down grasps
*  assumption of known receptacle locations

-----











## Saycan

https://zhuanlan.zhihu.com/p/655418399

语言操控机器人执行各类任务？——Google SayCan论文概要

PaLM-SayCan是SayCan的升级版，后续的一些工作基本都是以saycan为开端，比如code as policies, PaLM-E, RT-2等

## PaLM-E: An Embodied Multimodal Language Model

https://arxiv.org/abs/2303.03378



## Open X-Embodiment: Robotic Learning Datasets and RT-X Models 2023.10

https://zhuanlan.zhihu.com/p/666323712

具身智能机器人大数据集Open X-Embodiment和具身大模型RT-X论文解读



## **VoxPoser: Composable 3D Value Maps for Robotic Manipulation with Language Models**

https://zhuanlan.zhihu.com/p/651670658

李飞飞



## **ChatGPT for Robotics: Design Principles and Model Abilities**

https://zhuanlan.zhihu.com/p/652094830

大模型控制机器人   微软



## 语言模型做规划——Language Models as Zero-Shot Planners论文速读

Language Models as Zero-Shot Planners: Extracting Actionable Knowledge for Embodied Agents

https://zhuanlan.zhihu.com/p/656399047



------

最近一些Embodied AI工作的总结(SayCan/LM-Nav/WebShop/Gato/VPT/MINEDOJO)

https://zhuanlan.zhihu.com/p/541492104



