---
title: GLSL2UnityShader
tags: Unity3dShader
grammar_cjkRuby: true
---

```
if(aaa.a = _Alpharef)
                {  
                    discard;  
                }  
				
----------------------------------------------------------------------
#ifdef UNITY_ADRENO_ES3
    u_xlatb0 = !!(vec4(0.0, 0.0, 0.0, 0.0)!=vec4(_Alpharef));
#else
    u_xlatb0 = vec4(0.0, 0.0, 0.0, 0.0)!=vec4(_Alpharef);
#endif
    if((int(u_xlatb0) * int(0xffffffffu))!=0){discard;}
	
```

```
if(aaa.a != _Alpharef)
                {  
                    discard;  
                } 
-----------------------------------------
#ifdef UNITY_ADRENO_ES3
    u_xlatb0 = !!(u_xlat0!=_Alpharef);
#else
    u_xlatb0 = u_xlat0!=_Alpharef;
#endif
    if((int(u_xlatb0) * int(0xffffffffu))!=0){discard;}
```

```
float4 aaa = tex2D(_MainTex,i.uv);

// float4 dhsk = saturate(aaa * _Alpharef);
clip(saturate(aaa * _Alpharef).w - _Alpharef);
				
--------------------------------------------------------------------
u_xlat10_0 = texture(_MainTex, vs_TEXCOORD0.xy).w;
u_xlat0 = u_xlat10_0 * _Alpharef;
#ifdef UNITY_ADRENO_ES3
    u_xlat0 = min(max(u_xlat0, 0.0), 1.0);
#else
    u_xlat0 = clamp(u_xlat0, 0.0, 1.0);
#endif
    u_xlat0 = u_xlat0 + (-_Alpharef);
#ifdef UNITY_ADRENO_ES3
    u_xlatb0 = !!(u_xlat0<0.0);
#else
    u_xlatb0 = u_xlat0<0.0;
#endif
    if((int(u_xlatb0) * int(0xffffffffu))!=0){discard;}

```

```
 u_xlat16_0 = dot(gl_FragCoord.xyz, gl_FragCoord.xyz);
    u_xlat16_1 = max(u_xlat16_0, 0.00100000005);
    u_xlat16_0 = inversesqrt(u_xlat16_1);
    SV_Target0.xyz = vec3(u_xlat16_0) * gl_FragCoord.xyz;
    SV_Target0.w = 1.0;
	
----------------------------------------------------

inline half3 Unity_SafeNormalize(half3 inVec)
{
    half dp3 = max(0.001f, dot(inVec, inVec));
    return inVec * rsqrt(dp3);
}
float3 nor = Unity_SafeNormalize(i.vertex.xyz);

```