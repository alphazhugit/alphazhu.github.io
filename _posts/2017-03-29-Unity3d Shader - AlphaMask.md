```

Shader "ZX_AlphaMask" {
Properties {
	_Color ("Main Color", Color) = (1,1,1,1)
	_MainTex ("Base (RGB) Trans (A)", 2D) = "white" {}
	_MaskTex ("MaskTex",2D) = "white" {}
	_EmissionPower ("EmiPower",float) = 1
	_Progress ("Progress",Range(0,10)) = 0
}

SubShader {
	Tags {"Queue"="Transparent" "IgnoreProjector"="True" "RenderType"="Transparent"}
	LOD 200

CGPROGRAM
#pragma surface surf Lambert alpha

sampler2D _MainTex;
sampler2D _MaskTex;
fixed4 _Color;
float _EmissionPower;
float _Progress;

struct Input {
	float2 uv_MainTex;
	float2 uv_MaskTex;
};

void surf (Input IN, inout SurfaceOutput o) {

	fixed2 newUV = IN.uv_MaskTex;
	newUV.y *= _Progress;
		
	fixed4 c = tex2D(_MainTex, IN.uv_MainTex) * _Color;
	fixed4 d = tex2D(_MaskTex, newUV);
	o.Albedo = c.rgb;
	o.Alpha = d.a *c.a;
	o.Emission = c.rgb*_EmissionPower;
}
ENDCG
}

Fallback "Legacy Shaders/Transparent/VertexLit"
}

```