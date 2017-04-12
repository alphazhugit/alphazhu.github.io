---
layout:     post
title:      "Unity3dShader渐变裁剪科技感效果"
date:       2017-04-12 16:13:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

![iZYEq.gif](http://storage1.imgchr.com/iZYEq.gif)
![iZtU0.gif](http://storage1.imgchr.com/iZtU0.gif)

```
Shader "ZX/VF_AddAni" {
	Properties 
	{
		_Color("Color",Color) = (1,1,1,1)
		_FColor("frontColor",Color) = (1,1,1,1)
		_Prograss("Prograss",float) = 0
//		_MainTex ("Base (RGB)", 2D) = "white" {}
		_Alpha ("Alpha",float) = 1
		
	}
	SubShader 
	{
	Tags{"Queue" = "Transparent" "RenderType"="Transparent"}
		blend srcalpha one
		pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			
			
			struct v2f
			{
				half4 pos : SV_POSITION;
				half2 uv : TEXCOORD0;
			};
			
			struct appdata 
			{
				half4 vertex : POSITION;
				half4 texcoord : TEXCOORD0;
			};
			
		//	sampler2D _MainTex;
			fixed4 _Color;
			half4 _FColor;
			half _Prograss;
			fixed _Alpha;
			
			v2f vert (appdata v)
			{
				v2f o;
				o.pos = mul(UNITY_MATRIX_MVP,v.vertex);	
				o.uv=v.texcoord;
				return o;
			}
			
			half4 frag(v2f i) : COLOR
			{
			half4 tex =_Color;//  tex2D(_MainTex,i.uv) * _Color;
			tex.a =  (_Prograss - i.uv.y) ;
		
			if (tex.a<0.15f)
			{
			  tex.rgb = lerp(tex.rgb, _FColor.rgb,(1-tex.a/0.15f));
			}
			if (tex.a>0) tex.a =1;
			return tex * _Alpha;
			}
			
			
			ENDCG
		} 
//	FallBack "Diffuse"
	}
}


```

* 目前在unity5.x上测试可能还不行，等待以后更新。



