---
title: Reflection Probe
cover_picture: images/reflection-probe.png
categories: Cocos
tags:
    - blend
    - box projection
---
#### 项目案例
![](/works-images/reflection-probe.png)
![](/works-images/use-probe.jpg)
![](/works-images/use-skybox.jpg)
#### 开启BoxProjection
box projection功能可校正采样方向，以适应不同表面形状，可提高反射效果的准确性，下图看看出，开启box projection后地面反射几乎达到平面反射的效果
![](/works-images/box-p1.png)
#### 无修正效果，地面的反射效果完全对不上
![](/works-images/box-p2.png)
#### Blend ReflectionProbe
Blend Probes功能，支持多个反射探针的混合同时也支持和天空盒混合，允许在不同的位置捕捉反射信息，并平滑地过渡它们，以实现更自然的效果。
视频左侧角色没开启blend功能，移动时颜色会产生跳变。
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/probe-blend.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>
