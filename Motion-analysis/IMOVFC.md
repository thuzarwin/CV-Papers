# Identification of a Moving Object's Velocity with a Fixed Camera

## Abstract
> In this paper, a continuous estimator strategy is utilized to asymptotically identify the six degree-of-freedom velocity of a moving object using a single fixed camera. The design of the estimator is facilitated by the fusion of homography-based techniques with Lyapunov design methods. Similar to the stereo vision paradigm, the proposed estimator utilizes different views of the object from a single camera to calculate 3D information from 2D images. In contrast to some of the previous work in this area, no explicit model is used to describe the movement of the object; rather, the estimator is constructed based on bounds on the object’s velocity, acceleration, and jerk.

本文提出了一种连续估计策略，用来在使用单目固定相机的情况下，渐进识别移动物体六个自由度上的速度。这种估计器的设计融合了基于单应性的技术以及李雅普诺夫设计方法。与立体视觉的模式相似，这种估计器利用单目相机对物体不同角度的二维图像，来计算三维信息。与本领域之前的一些工作不同的是，估计时没有物体运动的显式模型，而是基于物体运动的速度、加速度以及加速度的导数来构建估计器。

## 1 Introduction
> Often in an engineering application, one is tempted to use a camera to determine the velocity of a moving object. However, as stated in [8], the use of a camera requires one to interpret the motion of a 3-dimensional (3D) object through 2D images provided by the camera. That is, the primary problem is 3D information is compressed or nonlinearly transformed into 2D information; hence, techniques or methods must be developed to obtain 3D information despite the fact that only 2D information is available. 

工程应用上常常需要使用一个照相机来得到一个运动物体的速度。然而正如在[8]中所提到的那样，要做到这样，首先需要从相机提供的二维图像中描述三维物体的运动情况。也就是说，基本问题就是从三维信息到二维信息的转换是有压缩或者说是非线性的；因此，我们需要研究一种在只能获得二维信息的情况下，重建三维信息的技术或者说是方法。

> To address the identification of the object’s velocity (i.e., the motion parameters), many researchers have developed various approaches. For example, if a model for the object’s motion is known, an observer can be used to estimate the object’s velocity [10]. In [20], a window position predictor for object tracking was utilized. In [12], an observer for estimating the object velocity was utilized; however, a description of the object’s kinematics must be known. In [9], the problem of identifying the motion and shape parameters of a planar object undergoing Riccati motion was examined in great detail. In [13], an autoregressive discretetime model is used to predict the location of features of a moving object. In [1], trajectory filtering and prediction techniques are utilized to track a moving object. Some of the work [24] involves the use of camera-centered models that compute values for the motion parameters at each new frame to produce the motion of the object. In [2] and [21], object-centered models are utilized to estimate the translation and the center of rotation of the object. In [25], the motion parameters of an object are determined via a stereo vision approach.

为了应对识别物体速度（也就是物体的运动参量）这个问题，很多研究者探索了各种各样的方法。比如说，如果物体的运动模型是已知的，一个观测器（observer） 就能被用来估计物体速度（见[10]）。在[20]中，利用了一个窗口位置预测器（A window position predictor）来跟踪目标。在[12]中，在物体运动学描述已知的情况下，利用了一个观测器来估计物体的运动速度。在[9]中，对于作黎卡提运动（Riccati motion）的平面物体的运动以及形状参量的识别问题，做了详尽的讨论。在[13]中，自回归离散模型（an autoregressive discretetime model）被用来运动物体的特征点位置。在[1中，弹道滤波与预测技术（trajectory filtering and prediction technique）被用来追踪运动物体。一些工作（如[24]）涉及到使用相机为中心的模型（camera-centered models），计算每一帧上的运动参数的值，来描述物体运动（？）。在[2]和[21]中，利用以物体为中心的模型（object-centered models）来估计变换以及物体的旋转中心。在[25]中，使用了立体视觉的方法来确定物体的运动参数。

> While it is difficult to make broad statements concerning much of the previous work on velocity identification, it does seem that a good amount of effort has been focused on developing system theory-based algorithms to estimate the object’s velocity or compensate for the object’s velocity as part of a feedforward control scheme. For example, one might assume that object kinematics can be described as
follows
$$
\dot x = Y(x) \phi
$$