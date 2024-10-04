---
layout: post
title: 变换
---

## 线性变换

### [定义](https://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E6%98%A0%E5%B0%84)

保持向量加法和标量乘法性质的映射

- 向量加法
$$ T(v+w) = T(v) + T(w) $$

- 标量乘法
$$ T(c \cdot w) = c \cdot T(w) $$

#### 原点不变性

向量空间中的原定就是零向量，零向量具有性质
- $v + 0 = v$
- $0 \cdot v = 0$

由标量乘法保持性得出 $T(0 \cdot v) = 0 \cdot T(v)$，由于 $0 \cdot v = 0$ ， 因此 $T(0) = 0 \cdot T(v) = 0$

## 几何解释

[3blue1brow视频](https://www.bilibili.com/video/BV1ys411472E?p=4&vd_source=337e6ca0b625397098605e2b560eac76)

- Lines remian lines (直线依然是直线)
- Origin remians fixed （原点保持固定）

### 常见的线性变换
- 缩放 (Scaling)：  $\begin{pmatrix} sx & 0 \\ 0 & sy \end{pmatrix}$
- 旋转 (Rotation)：$\begin{pmatrix} cos\theta & -sin\theta \\ sin\theta & cos\theta \end{pmatrix}$ 绕原点旋转 $\theta$
- 反射 (Reflection)： $\begin{pmatrix} -1 & 0 \\ 0 & 1 \end{pmatrix}$ 关于 y 轴反射
- 剪切 (Shear) $\begin{pmatrix} 1 & k \\ 0 & 1 \end{pmatrix}$ 沿 x 轴的剪切
- 投影 (Projection) $\begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix}$ 投影到 x 轴


**平移不是线性变换**，是一种仿射变换
$$ T(v) = Av + b $$



## 齐次坐标

[什么是齐次坐标?](https://caadxyz.github.io/blog/homogeneous-coordinates)

为了统一变换，需要引入齐次坐标表示平移变换


$$
\begin{pmatrix}
    0 & 0 & tx \\
    0 & 0 & ty \\
    0 & 0 & 1
\end{pmatrix}
\cdot
\begin{pmatrix}
    x \\ y \\ 1
\end{pmatrix} 
= 
\begin{pmatrix}
    x + tx \\ y + ty \\ 1
\end{pmatrix}
$$

## 旋转矩阵

- 二维坐标中，逆时针为正方形。
- 三维左手坐标系中，顺时针为正方向，从坐标轴看向原点。
- 三维右手坐标系中，逆时针为正方形，从坐标轴看向原点。

[Unity中的旋转](https://docs.unity3d.com/Manual/QuaternionAndEulerRotationsInUnity.html)

注意，这并不是说左右手中旋转算法不一样，反而是一样的。**opengl**使用右手坐标系。但是可以通过

- `glm::perspectiveLH`
- `glm::lookAtLH`

改成左手坐标系，也可以直接设置宏 `#define GLM_FORCE_LEFT_HANDED `





