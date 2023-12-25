---
title: NPR角色渲染
cover_picture: images/full.png
categories: Unity Shader
tags:
    - unity
    - urp
    - shader
---
#### 项目案例
![](/works-images/head.png)
![](/works-images/head_shadow.png)
二次元风格
头发：
头发漫反射系数使用半兰伯特，通过2个材质参数对范围和边缘进行smoothstep
half shadowArea = smoothstep(_ShadeOffset - _ShadeSoftness, _ShadeOffset + _ShadeSoftness, halfNoL);
差值计算阴影区域的颜色
half3 shadowColor = lerp(_ShadowMapColor, 1, shadowArea )
result = albedo * light.color * shadowColor
高光部分：
这个案例高光使用了贴图，优点是形状可以根据美术偏好进行调整，缺点是动态高光受限，可以通过世界空间观察方向的y值对uv坐标y轴做偏移，实现简单动态高光。
 float offset = V.y > 0 ? V.y * 0.3 : V.y * 1.3;
 float3 specValue= tex2D(_SpecMap, float2(input.uv1.x, saturate(input.uv1.y - offset * 0.05))).rgb;
脸部：
![](/works-images/full.png)
脸部阴影使用常规方式计算会很丑陋，这里使用了SDF面部阴影贴图，存了不同角度下阴影覆盖脸上哪些部分。
<!--SDF是一个函数，函数的输入是空间的一个点，函数的输出是这个点到shape的距离，我所了解到的就是我们可以通过
SDF贴图来判断当前像素是否在阴影里,SDF涵盖了0-90度的范围，90-180需要水平翻转一下阴影图。-->
dot(leftDir, lightDir)，用来决定是不是需要切换左右阴影图，dot(leftDir, lightDir) > 0正常采样，否则翻转x轴。
dot(frontDir, lightDir)，用来和阴影图做对比，用来判断当前是不是在阴影中，阴影里的部分直接乘了材质参数_ShadowColor
float lightPos = 1 - clamp(0, 1, dot(frontDir, lightDir) * 0.5 + 0.5);
float lightMap = dot(lightDir, leftDir) > 0 ? lightData.x : lightData.y;
isSahdow = step(lightMap, lightPos);
漫反射部分直接采样albedo贴图
#### 使用Ramp贴图模拟了次表面散射部分
float2 rampUV = float2(NdotL * 0.5 + 0.5, 0.5);
half3 rampColor = tex2D(_RampDiffuse, rampUV).rgb;
#### 对暗部做了补光处理
rampColor += max(0,_ScatteringIntensity * pow(1 - max(0, NdotL * 0.5 + 0.5), _ScatteringPower)   * _ScatteringColor.rgb);
diffuse *= rampColor;
最后加了边缘光和自发光。
身体皮肤:
漫反射系数的处理和头发一样，通过2个材质参数对范围和边缘进行smoothstep
half shadowArea = smoothstep(_ShadeOffset - _ShadeSoftness, _ShadeOffset + _ShadeSoftness, halfNoL);
同样暗部做了和脸部一样的补光处理。
高光部分用了GGX的法线分布函数和几何函数，菲涅尔项没处理。
衣服:
衣服的材质和URP标准的PBR材质处理方式一致，只添加了描边pass
描边:
描边用了顶点沿着法线方向外扩的方式，对于三角面共用顶点产生的断边问题进行了处理。
#### Video
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/pbr-npr.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>

偏写实风格
![](/works-images/pbr-face.png)
头发：
漫反射部分用的Lambert。
![](/works-images/AnisotropicVariation.png)
高光部分使用了Kajiya Kay的模型实现了各向异性高光，加了一张Shift Tangent贴图调整高光形状。
脸部：
![](/works-images/3S.jpg)
脸部渲染除了albedo贴图还用到了thickness和curvature贴图
采用了预积分皮肤渲染来模拟次表面散射，y轴采样用curvature贴图，同样对暗部做了补光处理。
![](/works-images/mouth.png)
同时对于对于厚度较薄的地方耳朵、鼻子、嘴唇进行了补光处理。

#### UE4模仿原神角色渲染
<video src="https://xb-resource.oss-cn-shanghai.aliyuncs.com/yuanshen.mp4" controls="controls" style="max-width: 100%; display: block; margin-left: auto; margin-right: auto;">
</video>








