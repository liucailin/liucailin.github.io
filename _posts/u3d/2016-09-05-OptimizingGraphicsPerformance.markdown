---
layout: post
title: Unity3D 优化图形性能
---

翻译自unity参考文档[OptimizingGraphicsPerformance](http://docs.unity3d.com/Manual/OptimizingGraphicsPerformance.html)，部分参考自[夜风的BLOG](http://blog.csdn.net/ynnmnm/article/details/39003147)

## 定位影响

两个主要因素影响图形性能：（1）GPU （2）CPU，要首先确定瓶颈所在，因为GPU优化和CPU优化策略常常是不同的，甚至有可能是相对的，优化一方往往是要求另外一方承担更多的工作。

### 典型的瓶颈和检查方法

* GPU通常受限于**填充率（fillrate）**和存储器带宽（memory bandwidth）
	* 可以降低游戏显示分辨率，如果游戏跑得更快，说明你可能受限于GPU的填充率

* CPU通常受限于批处理（batches）的数量
	* 通过"[Rendering Statistics window](http://docs.unity3d.com/Manual/RenderingStatistics.html)"查看"batches"数量

### 其他常见瓶颈

* GPU要处理太多的顶点，至于多少是合适的顶点数量，依赖于具体的GPU设备和vertex shader的复杂程度。一般在移动平台上不要超过**100,000**个顶点，PC平台上可达几百万个顶点，不过要尽可能减少顶点数
* CPU要处理太多的顶点，这可能是蒙皮网格（skinned meshes)、衣服模拟、粒子等
* 如果GPU和CPU都没问题，那也有可能是脚本或者物理问题，可以通过[Unity Profier](http://docs.unity3d.com/Manual/Profiler.html)去定位问题

## CPU优化

要渲染物体到屏幕上，CPU有很多工作要处理，计算出哪些光会影响到物体，设定shader和shader参数，发送绘制命令（drawing commands）到显卡驱动（graphics driver)，然后显卡驱动准备好命令发送给显卡。

每一个物体都需要CPU开销，所以如果你有很多可视物体，开销会叠加在一起。举个例子，如果你有1000个三角形，对于CPU来讲，如果三角形都在同一个网格上的情形会比一个三角形一个网格的情形简单得多，虽然这两种情形对于GPU来讲是没多大区别的。

### 减少**可视物体（visible object）**的数量

* 合并相近的物体，通过手动的方式或者unity的[draw call batching](http://docs.unity3d.com/Manual/DrawCallBatching.html)
* 通过把不同的纹理合并成大图集来减少材质球的数量
* 减少会导致物体多次渲染的效果（比如反射，阴影，像素光照等）

合并物体时的每个网格至少有几百个三角形，并且整个网格使用同一种材质，合并两个不公用同一材质的物体并不会提升性能。拥有多个材质的最常见原因是两个网格不共用相同纹理，所以为了优化CPU，要确保合并的物体共用相同的纹理。

然而，如果在正向渲染路径（ Forward rendering path）下使用很多像素光照，合并可能会没有效果，参照接下来的光照优化。

## GPU：优化模型几何结构

下面是优化模型几何结构的两个基本规则

* 不要使用任何非必要的多余三角形
* 尽可能减少UV贴图接缝（seams）和硬边（hard edges）（顶点增多了一倍）的数量

注意，图形硬件处理的顶点实际数量常常跟3D应用程序报告的不一致。建模应用常常显示几何顶点数量，即构成模型的不同角点的数量。然而，图形显卡为了渲染目的可能会把一些几何顶点拆分成两个或者更多个逻辑顶点。如果一个顶点有多个法线、UV坐标或者顶点颜色，那么必须把它拆分。因此，Unity中的顶点数量一定会比3D应用程序给的定点数多。

模型的几何数量主要对GPU有意义，Unity中的一些特性也在CPU上处理模型，比如网格蒙皮。

## 光照性能

不需要计算的光照是最快的方法。可以使用[光照贴图](http://docs.unity3d.com/Manual/GIIntro.html)来烘培来代替每帧去计算。
* 更快（2-3倍对比与2个逐像素光照）
* 效果更好，因为可以烘培全局光照

在很多情况下都可以使用简单的技巧来避免添加多余的光源，比如用shader来计算Rim效果。

## GPU：纹理压缩和mipmaps

使用压缩纹理会减少纹理大小（结果是更快的加载速度和更小的内存占用）并且大幅提高渲染性能。压缩纹理占用的存储带宽只有未压缩的32位RGBA纹理的一小部分。

对于3D场景，通常建议都是打开Generate mipmaps，例外情况是纹理像素需要1 ：1映射到渲染的屏幕像素，如UI和2D游戏。

## LOD和每层剔除距离（per-layer cull distances）

剔除物体使得物体不可见，可以有效的减少CPU和GPU的开销，在很多游戏中，一个迅速而且有效的方法而且不压缩用户体验，就是通过剔除小物体，比如小石块或碎片这些在远处不可见的物体。

有多种方法可达成剔除

* 使用[Level of Detail](http://docs.unity3d.com/Manual/LevelOfDetail.html)系统
* 手动设置每层的剔除距离，把小物体放到另一个不同的层，通过[Camera.layerCullDistance](http://docs.unity3d.com/ScriptReference/Camera-layerCullDistances.html)来设置剔除距离

## 实时阴影

实时阴影对性能有很高的影响，查看[光照性能](http://docs.unity3d.com/Manual/LightPerformance.html)







