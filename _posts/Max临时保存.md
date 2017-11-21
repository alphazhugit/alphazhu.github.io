---
layout:     post
title:      "Max临时保存"
date:       2017-11-14 23:27:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - maxscript
---

maxscript UV编辑

```
modPanel.addModToSelection (Unwrap_UVW ()) ui:on
subobjectLevel = 3
$.modifiers[#unwrap_uvw].unwrap6.selectFacesByNode #{1..6852} $
$.modifiers[#unwrap_uvw].unwrap2.RotateSelected -2.52516 [0.5,0.5,0]
$.unwrap_uvw.mirrorH()

```