---
layout:     post
title:      "unity3d_Shader支持Fog方法"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3dShader
---

UNITY_TRANSFER_FOG(o,o.pos);
shader中加这个  UNITY_APPLY_FOG(i.fogCoord, restCol);
