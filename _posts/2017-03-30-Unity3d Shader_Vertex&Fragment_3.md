---
layout:     post
title:      "Unity3d Shader_Vertex&Fragment_3"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

* 此篇主要讲CG里的**自定义函数**以及**cginc**的自定义

---

>主要的shader代码

```
Shader "Custom/vf2" 
{
 
	SubShader 
	{
		pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			#include "sbin.cginc"//如果有文件夹，用/反斜杠区分//
			void Func(out float4 c);//定义函数，前项声明//



			void vert(in float2 objPos:POSITION,out float4 pos:POSITION,out float4 col:COLOR)
			{
				pos = float4(objPos,0,1);
			}


	

			void frag(inout float4 col:COLOR)
			{
//				Func(col);//调用函数//
				float arr[] = {0.5,0.5};
				col.x = Func2(arr);//调用Func2函数，函数来自自定义的"sbin.cginc"文件//
 			}

 			void Func(out float4 c)//函数具体内容//
 			{
 			 	c = float4(1,1,0,1);
 			}

			ENDCG

			
		}
	}
}
```
---

>自定义的cginc文件

```
float Func2(float arr[2])
			{
				float sum = 0;
				for(int i = 0;i<arr.Length;i++)
				{
					sum +=arr[i];
				}
				return sum;

			}
			//此自定义的文件后缀其实是可以是任何后缀，//
			//但是如果是其他后缀,与发高亮将会消失//
```

>[Cg（C for Graphic）标准函数库之数学函数与几何函数](http://i4.tietuku.com/9ec4062d7f003d50.jpg)
