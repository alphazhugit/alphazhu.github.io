---
layout:     post
title:      "Unity3d C# SendMessageReceiverMessage"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3d技术
---

```
void ShowNumber(int number)
	{
		Debug.Log("收到的数字是"+number);
	}//等待被调用的方法
public GameObject receiver;
	void Start()
	{
		receiver.SendMessage("ShowNumber",100,SendMessageOptions.DontRequireReceiver);
	}//控制开关
```