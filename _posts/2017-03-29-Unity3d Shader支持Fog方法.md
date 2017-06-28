---
layout:     post
title:      "Unity3d_添加Fog支持"
date:       2017-06-28 17:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---


[unity官方论坛链接](https://forum.unity3d.com/threads/official-changing-how-fog-is-done-in-unity-shaders.261217/)


Not much activity here, but quite some discussion (& approval) on the beta testing list. So here's the current plan; quite likely to become reality in Unity 5.0:
For surface shaders, nothing needs to be done; fog variants & code will be generated. You can add "nofog" to #pragma surface line, if you really don't want fog.


For vertex/fragment shaders, if you want fog you have to do this:
* Add #pragma multi_compile_fog
* Add UNITY_FOG_COORDS(n) to your vertex-to-fragment struct
* Add UNITY_TRANSFER_FOG(o,o.vertex); to your vertex shader. "o" is output struct name, and "o.vertex" is position in clip space.
* Add UNITY_APPLY_FOG(i.fogCoord, col); to end of your pixel shader. "i" is input struct name, and "col" is the color computed in your pixel shader. This applies standard fog color; if you want custom fog color (as some particle/additive shaders do, for example), you can do UNITY_APPLY_FOG_COLOR(i.fogCoord, col, fixed4(0,0,0,0)); to fog towards black for example.
* For fixed function shaders, nothing needs to be done. "Fog { ... }" command in shaderlab still works, but now it only affects fixed function shaders (for non-fixed function, see point 2 above).
* By default, fog modes used by scenes are included into game data build. If you know you'll want to change them at runtime, you can choose "Custom fog modes" (default is "automatic") under project's Graphics Settings, and tick checkboxes you need.
