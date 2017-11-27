---
title: 2017-11-27-GLSL2UnityShader
tags: 新建,模板,小书匠
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