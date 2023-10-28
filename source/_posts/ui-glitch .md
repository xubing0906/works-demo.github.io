---
title: UI Glitch
cover_picture: images/ui-2.png
tags:
    - unity
    - shader
---
#### 项目案例
![](/works-images/ui-2.png)
主要需求：
1 间歇波动抖动效果
2 扫描线效果
3 内发光效果
实现:
1 波动抖动故障使用了噪声生成函数库 XNoiseLibrary，用双层的noise实现波浪形扭动uv。
2 扫描线效果通过采样扫描线贴图做纹理动画
3 内发光效果，思路是在图片上叠加一层颜色，然后从边缘到内部对Alpha值进行渐变计算。
![](/works-images/ui-1.png)
#### Video
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/ui-glitch%20.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>
