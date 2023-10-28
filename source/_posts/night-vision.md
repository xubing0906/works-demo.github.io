---
title: Night Vision
cover_picture: images/night-vision1.jpg
categories: Unity Shader
tags:
    - unity
    - urp
    - post process
---
#### 项目案例
![](/works-images/night-vision1.jpg)
![](/works-images/night-vision2.jpg)
夜视和全息效果
主要效果包括：
1 部队和野怪替换成全息效果的材质
2 部队和野怪增加bloom效果
3 夜视效果
4 切换的过程中有径向模糊效果
实现思路：
1 全息材质效果，URP下通过添加RenderFeature进行多pass渲染，可以通过筛选Layer的方式，把部队和野怪所在unitLayer的物体全部通过增加一个全息材质的Pass在渲染一遍。
2 Bloom效果，把画面中亮度较强的像素提取出来进行模糊操作，在叠加到原纹理上。
3 夜视效果，通过在Shader中调整像素的亮度、饱和度、对比度来实现，但是需要把部队和野怪排除掉，通过添加RenderFeature把unityLayer的物体渲染到一张Mask纹理中，然后在做夜视效果的Shader中采样Mask贴图来决定是否添加夜视效果。
4 径向模糊效果，切换夜视效果的时候过渡的过程中添加径向模糊效果，首先定义一个中心点，从中心指向该像素点的方向就是径向模糊的方向，然后取当前像素点，以及沿着径向模糊方向再取几个点作为采样点，采样点越靠近中心越密集，越远离中心越稀疏，最后，该像素点的输出就是这些采样点的均值。这样，在靠近中心点的位置，采样距离小，几乎为0，也就不会模糊；而越靠近边界的位置，采样的距离越大，图像也就会越模糊。
#### Video
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/dark.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>
