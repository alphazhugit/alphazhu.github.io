
![274da8955bc7d4ab.gif](http://storage1.imgchr.com/images/274da8955bc7d4ab.gif)

```
Shader "Custom/Wave" {
	Properties {
		_Color ("Color", Color) = (1,1,1,1)
		_MainTex ("Albedo (RGB)", 2D) = "white" {}
		_Circle ("Circle",float) = 1

	}
	SubShader {
		Pass
			{
				CGPROGRAM
				#pragma vertex vert
				#pragma fragment frag
//				#include "UnityCg.cginc"

				sampler2D _MainTex;
				float _Circle;

				struct vertexInput
				{
					float4 vertex:POSITION;
					float4 texcoord : TEXCOORD0;

				};

				struct fragmentInput
				{
					float4 position : SV_POSITION;
					float4 uv : TEXCOORD;

				};

				fragmentInput vert(vertexInput v)
				{
					fragmentInput o;
					o.position = mul(UNITY_MATRIX_MVP,v.vertex);
					o.uv = v.texcoord;
					return o;					
				}

				half4 frag(fragmentInput i):COLOR
				{

					float new_Circle = sin(_Circle * _Time.x) * 30;
					fixed2 center = fixed2(0.5,0.5);
					fixed r = length(i.uv - center);
					fixed a = sin(r * new_Circle)/20;
					fixed2 offsetUV = (i.uv - center) * (1 + a) + center;
					float4 trans = tex2D(_MainTex,offsetUV);
					return trans;

				}
			

				ENDCG
			}
		}

}

```