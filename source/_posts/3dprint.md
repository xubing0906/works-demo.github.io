---
title: 3D Print
cover_picture: images/effect.png
tags:
    - unity
    - shader
---
#### 项目案例
![](/works-images/effect.png)
需求：
1 模型从下到上逐渐出现
2 未显示区域显示半透明的全息材质
3 全息材质增加一些扰动效果
实现：
1 从下至上打印就是固定方向的消融效果，可以采用给模型增加二套UV的方式或者采用世界坐标的Y轴，这里采用世界坐标Y轴的方式。float3 center = mul(unity_ObjectToWorld , float(0,0,0,1)).xyz，这样可以获取模型的中心点，unity的世界变化矩阵最后一列是存的Transform里的Position，所以可以在shader里提取这部分数据做一些计算。worldPos返回的是顶点的世界坐标，用模型的高度加上本身的位置减去顶点的y轴坐标，就可以得到一个随着模型位置改变的区间值，可以XYZ三个轴向来实现上下，左右，前后的消融效果。
2 全息材质，法线和视角方向点积结果做Pow运算实现边缘光效果，法线和灯光方向点积求出漫反射，扰动效果使用一张噪声贴图做pow运算，最后叠加到一起。
![](/works-images/3dprint-code.png)
#### Video
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/3dPrint.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>
