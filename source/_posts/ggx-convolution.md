---
title: GGX Convolution
cover_picture: images/ggx-mipmaps.png
categories: Cocos
tags:
    - convolution
    - cubemap
---
#### CubeMap卷积
由于光源使用的是 GGX BRDF 模型，IBL必须使用同样的BRDF模型，光照效果才能对的上。利用cmft库烘焙每个层级的mipmap，替换自动生成的mipmap，shader中根据粗糙度去匹配mip，卷积计算可以修复以下问题：
•	因为能量不守恒导致高光的泛光区和拖尾被削弱
•	自动生成的mip和材质粗糙度对不上

#### GPU自动生成mipmaps
![](/works-images/mipmaps.png)
#### GGX卷积后的mipmaps
![](/works-images/ggx-mipmaps.png)
#### 对比，左侧为自动生成的，右侧为GGX卷积后的
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/convolution.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>
