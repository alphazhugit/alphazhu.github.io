---
layout:     post
title:      "Unity3d Shader_SurfaceShader"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

```
Shader "Custom/SF" {
	Properties {
		_Color ("Color", Color) = (1,1,1,1)
		_MainTex ("Albedo (RGB)", 2D) = "white" {}
		_Glossiness ("Smoothness", Range(0,1)) = 0.5
		_Metallic ("Metallic", Range(0,1)) = 0.0
	}
	SubShader {
		Tags {"Queue" = "Transparent" "RenderType"="Opaque" }
		LOD 200
		
		CGPROGRAM
		// Physically based Standard lighting model, and enable shadows on all light types
		#pragma surface surf Lambert fullforwardshadows addshadow alpha

		// Use shader model 3.0 target, to get nicer looking lighting
		#pragma target 3.0

		sampler2D _MainTex;

		struct Input {
			float2 uv_MainTex;
		};

		half _Glossiness;
		half _Metallic;
		fixed4 _Color;

		void surf (Input IN, inout SurfaceOutput o) {
			// Albedo comes from a texture tinted by color
			fixed4 c = tex2D (_MainTex, IN.uv_MainTex) * _Color;
			o.Albedo = c.rgb;
			// Metallic and smoothness come from slider variables
//			o.Metallic = _Metallic;
//			o.Smoothness = _Glossiness;
			o.Alpha = c.a;
		}
		ENDCG
	}
//	FallBack "Diffuse"
}
```
* surfaceShader中，添加透明的方法使在#pragma后面添加alpha关键字
* 添加投影的方式有两种:
 * 一种是在不写投影通道的情况下在==FallBack==中指定一个含有投影通道的shader
 * 另一种是在==#paagma==中指定**==addshadow==**关键字（此方法在透明中没有效果）

[Unity手册](http://docs.unity3d.com/Manual/SL-SurfaceShaders.html)