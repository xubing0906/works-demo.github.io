---
title: Ocean
cover_picture: images/ocean.png
categories: Unity Shader
tags:
    - ocean 
    - shader
---
#### 海水效果
主要效果：

1 使用_Time函数让法线偏移使水面动起来，这里深水和浅水使用了不同的法线贴图。

2 浅水和深水做颜色过度，原理就是观察空间下比较水面和地面的线性深度，地面的深度可以通过深度纹理获取，水面深度是NDC空间下的Z值，ComputeScreenPos函数返回齐次裁剪空间下的屏幕坐标，除以W分量获取NDC坐标，Z值就是水面深度值，使用XY坐标采样深度纹理获取到地面的深度值，两者的差值越小越靠近岸边，由此进行深水和浅水的颜色插值。

3 反射使用了CubeMap，使用菲涅尔系数对水面颜色和反射颜色进行插值。

4 透射效果参考，GDC 2011 – Approximating Translucency for a Fast, Cheap and Convincing Subsurface Scattering Look

5 高光部分使用Phong高光公式。

6 岸边浪花使用正弦函数偏移UV后采样浪花贴图进行简单模拟。


<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/water1-1.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
your browser does not support the video tag
</video>


