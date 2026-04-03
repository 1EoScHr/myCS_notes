**GAMES202 高质量实时渲染**  
依旧感谢闫老师！  

***

# Lec1：Introduction and Overview

## What is GAMES202 about?

顾名思义，三个方面：**Real-Time**、**High Quality**、**Renderding**：  
+ 速度：超过30FPS，即可称为实时；在VR、AR领域要求更高，会到达90FPS；对于勉强算是连续的个位数FPS，则称为Interactive
+ 互动性 Interactivity：能够进行及时的交互
+ 真实感：trade off与贪婪，既要实时，又要真实感
+ 正确性 Dependability：要保证满足正确性，对错误率有高要求
+ 渲染是通过计算方式来模拟虚拟摄像机看到的虚拟场景  

因此202会涉及四个方面：  
1. **Shadows 阴影**：Shadow and Environment Mapping，阴影和环境光
2. **Global Illum 全局光照**：不需要预计算的Interactive Global Illumination Techniques、需要预计算的Precomputed Radiance Transfer
3. **Physically-based Shading 基于物理的着色**：Participating Media Rendering 参与介质渲染、Image Space Effects、Non-Photorealistic Rendering 非真实感渲染
4. **Real-time ray tracing 实时光线追踪**  

还有些共同的内容，譬如反走样、超采样等。  

与101相比，其特点是内容比较分散，类似不同方向的研讨会。  
科学是简单的，但科技是复杂的。  

从游戏的发展最能够看出实时渲染技术的发展。  

## NOT about

+ 3D建模、游戏引擎使用：这些内容是对工具的使用方法，更需要的是积累经验
+ 离线渲染：图形学应该有三门课程，入门、实时、离线，本课程是实时
+ Neural Rendering：譬如NeRF，暂时做不到实时、高质量（现在已经发展到实时了……发展啊。）但是会提到用神经网络降噪
+ 如何使用OpenGL。学习理论，OpenGL不重要，Shader Language才是重要的
+ 关于场景、着色器的优化，这是纯技术的问题，是工程向的
+ 逆向别人的Shader，不道义
+ 高性能计算  

## How to Study GAMES202

首先明白科学Science != 科技Technology，两者同样重要。  

**Real-time rendering = fast & approximate offline rendering + systematic engineering**  
实时渲染就是把离线渲染背后的知识进行简化，令其变得更快，同时使用系统工程。  

实时渲染技术上，工业界远比学术界领先，并且由于版权，学术界能得到的比较少。  
all in all, practice make pretty.  

## Why Study GAMES202

CG is awsome.   

## Motivation

计算机图形学能生成足够真实的图片，但这些方法往往无法兼顾质量与速度。  
对于实时渲染，需要做一些尽可能的近似估计，尽可能加速并使结果真实。  

## Evolution of Real-Time Rendering

略，但重要节点是可编程渲染管线的出现、预计算方法、Interactive Ray Tracing。  
至于实时光追，的确是里程碑。  

***

# Lec2：Recap of CG Basic

## Graphics (Hardware) Pipeline

经典的那套流程：  
+ 初始：三维模型，被描述为三维空间中的点集
+ Vertex Processing：屏幕空间上的点集
+ Triangle Processing：按照原本的连接关系连接成三角形（屏幕上）
+ Rasterization：把原本连续的三角形离散化，变为一个个像素（或者fragment），同时遮挡处理也在这里完成
+ Fragment Processing：施加着色模型
+ Display：变成图片  

## OpenGL

是一系列运行在CPU上的API，负责调用GPU，因此语言是无关的  
跨平台  

有些坏处：  
+ 版本碎片化
+ 其代码是C风格的，没有面向对象，不好用  

在101中推导的每一步，都能在OpenGL中找到对应的方法。  

重要的理解方式：画油画  
+ 首先把物体/模特摆好
+ 把画家放在指定位置（找好角度）
+ 在画架上贴上画布
+ 绘制
+ 如果想继续画，再贴一张新画布
+ 如果换了画家的位置，还可以用之前画好的  

GPU中有专门一块名为**Vertex buffer object (VBO)**，用来存放模型的元数据  
OpenGL把那些复杂的矩阵都自动推导，不用自己写。  

对于画架，OpenGL中就是**framebuffer**  
指定一个framebuffer（画架），就可以同时渲染多张图，称为MRT  
一次渲染计算（pass）中输出多张图到framebuffer，譬如最后图片、深度、其他风格  

可以选择直接渲染到屏幕，但并不推荐，因为会导致上一帧没有画完、下一帧就覆盖，撕裂画面。用垂直同步。  
或者双重缓冲、三重缓冲  

在绘制时，就用到**vertex shader**与**fragment shader**两大着色器，都由我们自己定义：  
vertex shader将顶点进行一系列变换、插值，传给frangment shader  
其会对每一个fragment做后续着色，深度测试可以自己写，也可以交给OpenGL自己做  

**总结**：在每一个渲染pass中，OpenGL的作用就是告诉GPU应该去做什么  
+ 传入场景内的物体、相机、MVP等
+ 传入需要使用的画架（framebuffer）、输入输出textures
+ 传入vertex shader、fragment shader
+ 接收到所有必须元素后，GPU就会开始绘制  

## OpenGL Shading Language (GLSL)

### Shading Language

着色语言定义了vertex shader、fragment shader怎么做，类似C风格，最早是没有的（人们在GPU上写汇编！），后面才逐步建立起来。  
着色语言终归是要经过编译、成为GPU汇编的。  

### Shader Setup

写完shader后，就要经过一系列的过程才能用。  

+ **Initializing**，写完后相当于是create，接着编译compile，把编译出的各类shader都放在一起成为program供GPU调用，接着链接、使用
+ Shader源文件也就是一串字符串  

### Debugging Shaders

多年以前，使用Nvidia的Nsight  
但现在有Nsight Graphics，跨平台，但只支持N卡；还有RenderDoc，同样跨平台，不限品牌  

## The Rendering Equation

经典的渲染方程：  
*out_radiance(p,w) = emission(p,w) + BRDF(p,w,w) \* incident_radiance(p,w) \* cos*  

在实时渲染中，会引入一些其他元素，比如visibility.  
并且为了性能，一般只考虑一次弹跳。  