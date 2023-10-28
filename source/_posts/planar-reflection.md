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
#### 偏移world pos后的效果
![](/works-images/planar-2.png)

