---
layout:     post
title:      "Unity3d Planar Shadow"
date:       2017-04-18 15:54:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

因为一直没有接触过游戏具体的行业，所以对于游戏行业内部的很多东西是不了解的，比如实时投影的问题一直困扰很多开发者，这边要感谢QQ上的**程序_Sea**提供的解决方案。

说白了就是把想要产生投影的模型复制一份，进行压扁，有骨骼动画的模型进行保留，下面看一下实际的效果，据**程序_Sea**所说，**王者农药**这款游戏上的实时投影也是采用这种方式进行制作的。

![](http://storage1.imgchr.com/iMoqg.jpg)

可以看到图中哪吒这个英雄下面的投影，同时还做了周围虚化的处理。


**程序_Sea**给了我一个大神的方案，下面是我实测效果

![](http://storage1.imgchr.com/iMWGt.gif)

为了方便大家观察，我把投影和模型分开进行展示

![](http://storage1.imgchr.com/iMfRP.gif)

最最牛逼的功能是这影子能随着太阳（日）的旋转进行变化(ﾟДﾟ≡ﾟДﾟ)

![](http://storage1.imgchr.com/iMhxf.gif)

后来我用这种方式的影子跟实时投影进行了一下对比：

![](http://storage1.imgchr.com/iM5M8.png)

这张是在**实时**投影下的一些参数

![](http://storage1.imgchr.com/iMIsS.png)

这个是在**Planar Shadow**下的参数

可以看出来优化程度确实不小

那么代码如下

```c#:
Shader "Unlit/PlanarShadow"
{
	Properties
	{
        Center("Center", Vector) = (0,0,0,0)
        Normal("Normal", Vector) = (0,1,0,0)
	}
	SubShader
	{
		Tags { "RenderType"="Opaque" "LightMode"="ForwardBase"}
		LOD 100
		blend srcalpha oneminussrcalpha
		Pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			
			#include "UnityCG.cginc"

			struct appdata
			{
				float4 vertex : POSITION;
				float4 texcoord: TEXCOORD0;
			};

			struct v2f
			{
				float4 pos : SV_POSITION;
				float4 uv: TEXCOORD1;
			};            

            float4 Center, Normal;
			v2f vert (appdata v)
			{
				v2f o;
				UNITY_INITIALIZE_OUTPUT(v2f, o);

                float4 wPos = mul(unity_ObjectToWorld, v.vertex);
                // directional light
                float3 direction = normalize(_WorldSpaceLightPos0.xyz);
                // https://en.wikipedia.org/wiki/Line%E2%80%93plane_intersection
                float dist = dot(Center.xyz - wPos.xyz, Normal.xyz) / dot(direction, Normal.xyz);
                wPos.xyz = wPos.xyz + dist * direction;
				o.pos = mul(UNITY_MATRIX_VP, wPos);
				o.uv = v.texcoord;
				return o;
			}
			
			fixed4 frag (v2f i) : SV_Target
			{
					return fixed4(0.25,0.25,0.25,1);

			}

			ENDCG
		}
	}
}

```

[钱康来 PlanarShadow](http://qiankanglai.me/2016/12/23/planar-shadow/)

[钱康来知乎](https://www.zhihu.com/people/qian-kang-lai/answers)

我只是大自然的搬运工 ⁄(⁄ ⁄•⁄ω⁄•⁄ ⁄)⁄


