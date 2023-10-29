---
title: Planar Reflection
cover_picture: images/planar.png
categories: Cocos
tags:
    - planar reflection
---
#### 平面反射效果
原理：把摄像机放到关于平面对称的位置，然后看向平面的反射方向，把场景渲染至RT，shader中通过屏幕uv采样rt即可得到反射效果。
只需解决两个问题
1 摄像机关于平面对称的位置
2 摄像机方向
平面方程的点法表达形式通常为 N·P + d = 0
假设平面位于原点处，P为相机的位置，N为平面法线
根据平面方程可得到相机到平面的距离d。
p沿着-N方向移动2d就是反射相机的位置
所以p' = P + 2 * d * (-N)或者p' = P - 2 * d * N，如果平面不在原点处，还需计算平面到原点的距离dist
p' = P - 2 * (d - dist) * N，
对于向量同样适用此计算方式，只是无需dist
写成函数:
private _reflect (out: Vec3, point: Vec3, normal: Vec3, offset: number): Vec3 
{
    &emsp;&emsp;const n = Vec3.clone(normal);
    &emsp;&emsp;const dist = Vec3.dot(n, point) - offset;
    &emsp;&emsp;n.multiplyScalar(2.0 * dist);
    &emsp;&emsp;Vec3.subtract(out, point, n);
    &emsp;&emsp;return out;
}
![](/works-images/planar-1.png)
上图有一个问题，就是地面虽然使用了法线贴图，但是平面反射的效果却没受法线贴图凹凸的影响，看起来就像镜面反射一样特别光滑。
采样平面反射贴图的uv坐标是顶点worldpos经过投影矩阵变换后除以w分量后的ndc坐标，在乘以0.5 + 0.5映射到0-1之间，要想让法线贴图影响uv坐标，需要在投影矩阵变换之前对worldpos进行偏移，过程如下：
![](/works-images/worldpos.png)
以平面上的点w来举例：渲染反射贴图时相机的方向是和反射方向一致的，对于光滑的平面w必然经过相机的方向。
但是应用反射贴图后，反射向量会指向不同的方向，相机指向新的反射方向时会和平面产生一个新的交点，只需计算出交点的坐标，然后把此坐标转到裁剪空间进行透视除法后映射到屏幕坐标，然后进行采样就应该能得到想要的效果。
w'就是新的worldpos，所以问题变成求w'的坐标，计算w'的位置关键在与计算T的距离

W' = C' + R' * T
w'· N + d = 0
(C' + R' * T) · N + d = 0
C'· N + R' * T · N = abs(d)
T = (abs(d) - C' · N ) / R'· N
W' = C' + T * R'
#### 偏移world pos后的效果
![](/works-images/planar-2.png)

