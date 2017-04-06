---
layout:     post
title:      "Unity3d Shader_Vertex&Fragment_1"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

```
Shader "Custom/vf1" {
	SubShader 
	{
		pass
		{
			CGPROGRAM
			#pragma  vertex vert
			#pragma fragment frag
			#define MACROFL FL4(fl4.ab,fl3.zy)//宏定义//

			typedef float4 FL4;//为类型定义别名//






			void vert(in float2 objPos:POSITION,out float4 pos:POSITION,out float4 col:COLOR)
			{
				pos = float4(objPos,0,1);
				col = pos;
			}

			void frag(inout float4 col:COLOR)
			{
				fixed r = 1;
				fixed g = 0;
				fixed b = 0;
				fixed a = 1;

				col = float4(r,g,b,a);

				//数据类型//
				//float ==>float2 / float3 / float4
				//half ==>half2 / half3 / half4
				//fixed ==>fixed2 / fixed3 / fixed4
				//fixed有可以存储256个数，所以颜色值一般用fixed来申明//

				bool bl = false;
				col = bl? col:fixed4(0,1,0,1);//三目运算，如果为真，输出红色，如果为假，输出绿色//

				float4 fl4 = (1,1,0,1);
				float3 fl3 = (1,0,1);
				float2 fl2 = (1,1);
				FL4 fl = MACROFL;//小点语法 = Swizzle操作,可以重复取值，可以取rgba，不可以取xyba// //后面是引用宏//

				float2x2 M2x2 = {1,0,1,1};//2行2列的矩阵//
				float2x4 M2x4 = {fl4,{0,1,0,1}};//2行4列的矩阵//

//				col = M2x4[0];

				float arr[4] = {1,0.5,0.5,1};//定义数组的方法//
//				col = arr;col = float4(arr.x,arr.y,arr.z,arr.w);//这两种方法不能使用//
				col = float4(arr[0],arr[1],arr[2],arr[3]);//必须采用此方法调用数组内的值//


				//结构体声明//
				struct v2f{
					float4 pos : SVPOSITION;
					float2 uv : TEXCOORD0;
				};

				//结构体调用//
				v2f o;
				o.pos = fl4;
				o.uv = fl2;

			}


			ENDCG
		}
	}
}
```

---

---
==参考资料==

[Nvdia fp20](http://http.developer.nvidia.com/Cg/fp20.html)

[Nvdia Cg_language](http://http.developer.nvidia.com/Cg/Cg_language.html)