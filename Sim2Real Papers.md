# Tinker sim2real paper

### Contribution:

仿真环境：

视频里加上数字孪生

sim2real：

* 深度图 sim2real 
* 两次点云
* 仿真平台

learning: 

* yolo微调
* grasp train一个抓的姿态   可以做一下sim2real 
  * 生成6D pose 的paper        Efficient Heatmap-Guided 6-Dof Grasp Detection in Cluttered Scenes？
  * 铰接物体 3D RL
* 冠神 openchat

### task:

技术路线：跟人  语音

鲁棒性   细节





---------

#### 1. Close the Optical Sensing Domain Gap by Physics-Grounded Active Stereo Sensor Simulation 深度传感器仿真

In this paper, we focus on the simulation of active stereovision depth sensors, which are popular in both academic and industry communities. Inspired by the underlying mechanism of the sensors, we designed a fully **physics-grounded simulation pipeline **that includes **material acquisition, ray-tracingbased infrared (IR) image rendering, IR noise simulation, and depth estimation**. The pipeline is able to generate **depth maps** with materialdependent error patterns similar to a real depth sensor in real time. We conduct real experiments to show that perception algorithms and reinforcement learning policies trained in our **simulation platform** could transfer well to the real-world test cases without any fine-tuning. Furthermore, due to the high degree of realism of this simulation, our depth sensor simulator can be used as a convenient testbed to evaluate the algorithm performance in the real world, which will largely reduce the human effort in developing robotic algorithms. The entire pipeline has been integrated into the SAPIEN simulator and is open-sourced to promote the research of vision and robotics communities



Off-the-Shelf Real-Time Sensor Simulation  （ real time 60+ FPS ）

1. pipeline:  

   **material acquisition**, ray-tracingbased infrared (IR) image rendering, IR noise simulation, and depth estimation.

2. contribution:

* We propose a **multispectral loss function** to acquire object material parameters, which include visual appearance loss and neural network based perceptual loss to help eliminate the mismatch in brightness and color in both visible spectrum and infrared spectrum. 
* Our second highlighted innovation is in adding textured light support. Existing ray tracing rendering systems usually do NOT support the rendering of textured lights.（textured light ?）
  * In order to achieve real-time simulation of the depth sensor, we further **integrate learning-based denoisers** into the renderer such that we can generate high-fidelity IR images with hundreds of FPS using a small number of samples per pixel (SPP). 
  * We build a **GPU-accelerated stereo matching module consists of common algorithms in real-world depth sensors. （achieve 200+ FPS under common settings while usual CPU implementations can hardly achieve 1 FPS.）



*  To generate simulation data with a relatively low domain gap, the simulation process must also create material-dependent error patterns similar to real sensors
*  仿真环境和真实世界中的深度图建立方式一模一样，才能减少sim2real的gap

* 使用现实中的深度获得方式，来构建仿真中的深度图





-------

#### Maniskill2: A unified benchmark for generalizable manipulation skills 可泛化操作

Generalizable manipulation skills, which can be composed to tackle longhorizon and complex daily chores, are one of the cornerstones of Embodied AI. However, existing benchmarks, mostly composed of a suite of simulatable environments, are insufficient to push cutting-edge research works because they lack **object-level topological and geometric variations**, are not based on **fully dynamic simulation**, or are short of native support for multiple types of manipulation tasks. To this end, we present ManiSkill2, the next generation of the SAPIEN ManiSkill benchmark, to address critical pain points often encountered by researchers when using benchmarks for generalizable manipulation skills. ManiSkill2 includes 20 manipulation task families with 2000+ object models and 4M+ demonstration frames, which cover stationary/mobile-base, single/dualarm, and rigid/soft-body manipulation tasks with 2D/3D-input data simulated by fully dynamic engines. It defines a unified interface and evaluation protocol to support a wide range of algorithms (e.g., classic sense-plan-act, RL, IL), visual observations (point cloud, RGBD), and controllers (e.g., action type and parameterization). Moreover, it empowers fast visual input learning algorithms so that a **CNN-based** policy can collect samples at about 2000 FPS with 1 GPU and 16 processes on a regular workstation. It implements a **render server** infrastructure to allow **sharing rendering resources** across all environments, thereby significantly **reducing memory usage**. We open-source all codes of our benchmark (simulator, environments, and baselines) and host an online challenge open to interdisciplinary researchers.



don't have to collect assets or design tasks by yourself, and can focus on algorithms

* 更好的使用计算资源（对于深度学习来说）



----

#### ActiveZero++: Mixed Domain Learning Stereo and Confidence-based Depth Completion with Zero Annotation 深度学习双目重建混合学习框架

Traditional depth sensors generate accurate real world depth estimates that surpass even the most advanced learning approaches trained only on simulation domains. Since ground truth depth is readily available in the simulation domain but quite difficult to obtain in the real domain, we propose a method that leverages the best of both worlds. In this paper we present a new framework, ActiveZero, which is a **mixed domain learning solution** for active stereovision systems that **requires no real world depth annotation**. First, we demonstrate the transferability of our method to out-of distribution real data by using a mixed domain learning strategy. In the **simulation domain**, we use a combination of **supervised disparity loss and self-supervised losses** on a shape primitives dataset. By contrast, **in the real domain**, we only use **self-supervised losses** on a dataset that is outof-distribution from either training simulation data or test real data. Second, our method introduces a novel **selfsupervised loss called temporal IR reprojection ** to increase the robustness and accuracy of our reprojections in hardto-perceive regions. Finally, we show how the method can be trained end-to-end and that each module is important for attaining the end result. Extensive qualitative and quantitative evaluations on real data demonstrate state of the art results that can even beat a commercial depth sensor.

* 使用现实中的深度获得方式，来构建仿真中的深度图



----

#### Sim2Real: Actively Building Explicit Physics Model for Precise Articulated Object Manipulation

Accurately manipulating articulated objects is a challenging yet important task for real robot applications. In this paper, we present a novel framework called Sim2Real2 to enable the robot to manipulate an **unseen articulated object** to the desired state precisely in the real world with **no human demonstrations**. We leverage recent advances in physics simulation and learning-based perception to build the interactive explicit physics model of the object and use it to plan a long-horizon manipulation trajectory to accomplish the task. However, the interactive model cannot be correctly estimated from a static observation. Therefore, we learn to **predict the object affordance (?) from a single-frame point cloud**, control the robot to actively interact with the object with a one-step action, and **capture another point cloud**. Further, the **physics model is constructed from the two point clouds**. Experimental results show that our framework achieves about 70% manipulations with < 30% relative error for common articulated objects, and 30% manipulations for difficult objects. Our proposed framework also enables advanced manipulation strategies, such as manipulating with different tools. Code and videos are available on our project webpage



* 使用一步现实中的交互，通过两次点云数据，来构建更精确的物理模型

-------

#### Part-Guided 3D RL for Sim2Real Articulated Object Manipulation 铰接物体Sim2real操作     （**）

* tinker 开门
* 鹏威



抓取6D的姿态







---

语言大模型

#### Saycan



#### tidybot





-----

#### 

#### Bidirectional Sim-to-Real Transfer for GelSight Tactile Sensors With CycleGAN 触觉传感器Sim2real

#### TransTouch: Learning Transparent Objects Depth Sensing Through Sparse Touches 视触觉融合用于透明物体三维重建





----

前置：

**A. Active Stereovision Depth Sensor**

depth sensors： passive stereovision, **active stereovision**, structured light, and Time-of-Flight (ToF)

* active stereovision depth sensor ：an IR pattern projector, two IR cameras, and an RGB camera.
  * The projector casts an IR pattern (typically random dots) onto the scene. The two IR cameras capture the scene with the reflected pattern in the IR spectra. The depth map is computed by stereo matching on the two captured IR images.
  * Compared with passive stereovision, active stereovision can measure texture-less areas. 
  * Compared with structured light which uses coded pattern(s), active stereovision is unaffected when multiple devices measure the same area simultaneously. 
  * Compared with ToF, active stereovision has higher spatial resolution and is not affected by multi-path interference.

**B. Physically Based Rendering**

A PBR pipeline seeks to model both the light transport process and the surface material in a physically correct way.

* Light transport process. Many rendering techniques have been proposed to model the light transport process, such as ray casting, recursive path tracing, and photon mapping. These methods are covered by an umbrella term **ray tracing**.
  * ray tracing algorithm calculates the color for each pixel by tracing a path from the camera and accumulates the material-dependent weight along the path to light sources.
  * cutting-edge acceleration approaches： deep-learning-based denoising， superresolution

* Surface material modeling.
  * Bidirectional Scattering Distribution Function双向散射分布函数：describes how light scatters from a surface.
    * However, researchers have proposed many parameterized models to approximate the function.   Among all these efforts, the Disney™ PBR material model



**active stereovision depth sensor simulation （realsense d415）**

reason:

1. Depth map is more suitable for robotic tasks
2. Depth maps do not involve color information, which is extremely challenging to align between the simulator and the real world.
3. Active stereovision depth sensor has wide adoption：have relatively high accuracy, high spatial resolution, low cost, and robustness to lighting conditions（17
4. Active stereovision depth sensor simulation requires the estimation of fewer parameters ： work at a specific wavelength, mostly infrared
5. Real-time realistic ray tracing techniques have matured just recently
   1. Active stereovision sensors use IR lights to probe the environment and form stereo image pairs.

