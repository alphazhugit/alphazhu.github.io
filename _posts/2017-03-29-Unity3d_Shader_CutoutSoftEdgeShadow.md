---
layout:     post
title:      "Unity3d_Shader_CutoutSoftEdgeShadow"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

![26f8057e51def20c.png](http://storage1.imgchr.com/images/26f8057e51def20c.png)

```

Shader "MXR/CutoutSoftEdgeShadow" { 
Properties {
	_Color ("Main Color", Color) = (1, 1, 1, 1)
	_Cutoff ("Base Alpha cutoff", Range (0,.9)) = .5 
	_Texture1 ("Texture 1", 2D) = "" 
}

SubShader {
	Tags { "Queue"="AlphaTest" "IgnoreProjector"="True" "RenderType"="TransparentCutout"}

	Pass {  

	 Tags { "LightMode"="ForwardBase"}
		CGPROGRAM

			#pragma vertex vert
			#pragma fragment frag
			#pragma multi_compile_fwdbase
		
			#include "UnityCG.cginc"
			#include "AutoLight.cginc"

			struct appdata_t {
				float4 vertex : POSITION;
				float4 color : COLOR;
				float2 texcoord : TEXCOORD0;
			};

			struct v2f {
				float4 pos : SV_POSITION;
				float4 color : COLOR;
				float2 texcoord : TEXCOORD1;
				LIGHTING_COORDS(2, 3) 
			};

			sampler2D _Texture1;
			float4 _Texture1_ST;
			float _Cutoff;
			float4 _LightColor0;

			
			v2f vert (appdata_t v)
			{
				v2f o;
//				o.pos = mul(UNITY_MATRIX_MVP, v.vertex);
				o.pos = mul(UNITY_MATRIX_MVP, v.vertex);
				o.color = v.color;
				o.texcoord = TRANSFORM_TEX(v.texcoord, _Texture1);
				TRANSFER_VERTEX_TO_FRAGMENT(o);
				return o;
			}
			
			float4 _Color;
			half4 frag (v2f i) : COLOR
			{
				half4 col = tex2D(_Texture1, i.texcoord);
				clip(col.a - _Cutoff);
				float4 atten = LIGHT_ATTENUATION(i);
				return col * atten;
			}
		ENDCG
	}

	// Second pass:
	//   render the semitransparent details.
	Pass {
		Tags { "RequireOption" = "SoftVegetation"  "LightMode"="ForwardBase"}
		blend srcalpha oneminussrcalpha
		// Dont write to the depth buffer
		ZWrite off
		
		// Set up alpha blending
		Blend SrcAlpha OneMinusSrcAlpha
		
		CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			#pragma multi_compile_fwdbase
			
			#include "UnityCG.cginc"
			#include "AutoLight.cginc"

			struct appdata_t {
				float4 vertex : POSITION;
				float4 color : COLOR;
				float2 texcoord : TEXCOORD0;
			};

			struct v2f {
				float4 pos : SV_POSITION;
				float4 color : COLOR;
				float2 texcoord : TEXCOORD0;
				LIGHTING_COORDS(4, 5)
			};

			sampler2D _Texture1;
			float4 _Texture1_ST;
			float _Cutoff;

			v2f vert (appdata_t v)
			{
				v2f o;
				o.pos = mul(UNITY_MATRIX_MVP, v.vertex);
				o.color = v.color;
				o.texcoord = TRANSFORM_TEX(v.texcoord, _Texture1);
				TRANSFER_VERTEX_TO_FRAGMENT(o);
				return o;
			}
			
			float4 _Color;
			half4 frag (v2f i) : COLOR
			{
				half4 col = tex2D(_Texture1, i.texcoord);
				clip(-(col.a - _Cutoff));
				float4 atten = LIGHT_ATTENUATION(i);
				return col * atten;
			}
		ENDCG
	}
}

FallBack "Diffuse"
}

```