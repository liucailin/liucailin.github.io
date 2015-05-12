---
layout: post
title: 计算机图形学读书笔记
---

阅读书籍

- GPU编程与CG语言之阳春白雪下里巴人
- 3D数学基础：图形与游戏开发

## GPU图形绘制管线 ##

图形绘制管线描述**GPU渲染流程**，即“给定视点、三维物体、
光源、照明模式和纹理等元素，如何绘制一幅二维图像”。

### 三个主要阶段 ###

- 应用程序阶段，与CPU和内存打交道，诸如碰撞检测、场景图建立、空间八叉树更新、视锥裁剪等在执行，在该阶段末端，将**几何体数据**(顶点坐标、法向量、纹理坐标、纹理等)通过数据总线传送到图形硬件
- 几何阶段，与GPU打交道，主要负责顶点坐标变换、光照、裁剪、投影以及屏幕映射，该阶段末端得到经过变换和投影之后的几何体数据
- 光栅阶段，基于上阶段的输出数据，为**像素**（Pixel）正确配色，该阶段进行的都是单个像素的操作，每个像素的信息存储在颜色缓冲器（frame buffer）中

#### 顺序 ####

几何阶段

模型坐标空间（Object Space） -`变换`-> 世界坐标空间（World Space） -`变换`-> 观察坐标空间（Eye Space） -`变换`-> 裁剪空间（标准视体空间，Canonicalview Volume Space） -`投影`-> 屏幕坐标空间（Screen Space）

光栅阶段

---> 图元装配 -> 光栅化-> 像素计算-> 颜色缓冲区

## CG ##

Vertex program 负责顶点变换和光照计算，Fragment program 负责像素颜色计算

### 数据类型 ###

**注意,该页面和官网不一致！**

##### 基本数据类型 #####

1. float， 32位浮点型
1. half， 16位浮点型
1. int， 32位整型，有些profile会识别成float
1. fixed， 12位定点数，被所有的fragme profile支持
1. bool， 布尔，用于`if`和条件操作符`？：`
1. sampler*， 纹理对象的句柄，sampler，sampler1D，sampler2D，sampler3D，samplerCUBE，samplerRECT
1. string， 不被当前的profiler支持

##### 内置向量数据类型 #####

基于基本数据类型，向量最长不能超过四元，如 float1、 float2、 ··· 、 float4

初始化

	float4 array = float4(1.0, 2.0, 3.0, 4.0)
	float2 a = float2(1.0, 1.0)
	float4 b = float4(a, 1.0, 1.0)

##### 内置矩阵数据类型 #####

如 float1×1、 float2×2、 ··· 、float4×4

初始化

	float2*3 m = {1.0, 2.0, 3.0, 4.0, 5.0, 6.0}

#### 数组 ####

	float a[2] = {1.0, 2.0} 

支持结构体定义，但结构体不支持继承，只用于简单封装数据和方法，语法与c一致，一般来说，Cg源码会在文件首部定义二个结构体，分别定义输入和输出类型，与C不一样的是，在定义成员变量的时，除了指明变量，还需要额外指明该成员的**绑定语义类型（Binding Semantics)**

包括 **POSITION， COLOR， NORMAL Texcoord**  等等

其他语法与c类似，不再记录

###  输入输出与语义绑定 ###

in， out， inout, 语义绑定是两个程序之间的桥梁，相当于指定了寄存器的位置。

## 光照模型 ##

#### 物体发光原理 ####

当光照射到物体表面时，一部分被物体表面吸收，另一部分被反射，对于透明物体而言，还有一部分被透射。被吸收的光能转化为热能，其余的进入人体眼睛，产生视觉效果。由光的波粒二重性，光的强度决定了物体表面的亮度，不同波长的比例决定了物体表面的颜色。

所以，物体表面光照颜色由入射光、物体材质，以及关于物体的交互方式（也就是反射，理想情况下入射角等于反射角）决定。

#### 漫反射与lambert模型 ####

物体表面等同地向各个方向散射的现象称为光的漫发射(diffuse reflection）。产生光的漫反射现象的物体表面称为理想漫反射体，也称为朗伯（Lambert）反射体。

对于仅暴露在环境光下的朗伯反射体，某点的漫反射公式的光强

$$I_{ambdiff} = k_aI_a$$

其中，$I_a$表示环境光强度，$k_a$ 为材质对环境光的反射系数， $I_{ambdiff}$是漫发射体与环境光交互反射的光强。

Lambert定律：当方向光照射到朗伯反射体上是，漫反射光的光强与入射光的方向和入射光点表面法向量夹角的成正比：

$$I_{ldiff} = k_dI_l\cos\theta$$

也就是

$$I_{ldiff} = k_dI_l(\vec{N} \cdot \vec{L})$$

$\vec{N}$是顶点的单位法向量，$\vec{L}$是**顶点指向光源的单位向量**,$k_d$是**漫发射系数，也就是材质的漫发射颜色？**

综合下来

$$I_{diff} = I_{ambdiff} + I_{ldiff} = k_dI_a + k_dI_l(\vec{N} \cdot \vec{L})$$

#### 镜面发射与Phong模型 ####


$$I_{spec} = k_sI_l(\vec{V} \cdot \vec{R})^{n_s}$$

$k_s$为材质的镜面反射系数，$n_s$是高光指数，V表示从顶点到视点的观察方向，R代表反射光方向。

高光指数反映了物体表面的光泽程度。越大反射光越集中。

$$\vec{R} + \vec{L} = (2 \vec{N} \cdot \vec{L}) \vec{N}$$

#### Blinn-Phong光照模型 ####

在OpenGL和Direct3D渲染管线中，Blinn-Phong是默认的渲染模型。速度比Phong模型快。

$$I_{spec} = k_sI_l(\vec{N} \cdot \vec{H})^{n_s}$$

$$\vec{H} = normalize(\vec{L} + \vec{V})$$

#### cg基本光照模型代码 ####

书上的解释有点概念混淆，详细请看代码

	void ComputingLight(float4 position  : POSITION,

              float3 normal    : NORMAL,

              out float4 oPosition : POSITION,

              out float4 color     : COLOR,

              uniform float4x4 modelViewProj,

              uniform float3 lightPosition,

              uniform float3 eyePosition,

			  /// LIGHT PARAMETERS

              uniform float3 globalAmbient,

              uniform float3 lightColor,


			  /// MATERIAL PARAMETERS

			  // Emissive reflectance
              uniform float3 Ke,

			  // Ambient reflectance
              uniform float3 Ka,

			  // Diffuse reflectance

              uniform float3 Kd,

			  // Specular reflectance
              uniform float3 Ks,

              uniform float  shininess)



	{
	
		  oPosition = mul(modelViewProj, position);
		
		
		
		  float3 P = position.xyz;
		
		  float3 N = normal;
		
		
		
		  // Compute the emissive term
		
		  float3 emissive = Ke;
		
		
		
		  // Compute the ambient term
		
		  float3 ambient = Ka * globalAmbient;
		
		
		
		  // Compute the diffuse term
		
		  float3 L = normalize(lightPosition - P);
		
		  float diffuseLight = max(dot(N, L), 0);
		
		  float3 diffuse = Kd * lightColor * diffuseLight;
		
		
		
		  // Compute the specular term
		
		  float3 V = normalize(eyePosition - P);
		
		  float3 H = normalize(L + V);
		
		  float specularLight = pow(max(dot(N, H), 0),
		
		                            shininess);
		
		  if (diffuseLight <= 0) specularLight = 0;
		
		  float3 specular = Ks * lightColor * specularLight;
		
		
		
		  color.xyz = emissive + ambient + diffuse + specular;
		
		  color.w = 1;
	
	}

