---
layout:     post
title:      "Unity3d C#射线Ray"
date:       2016-03-02 12:00:00
author:     "Alpha"
header-img: "img/in-post/default-bg.jpg"
tags:
    - Unity3d技术
---

```
public class Raytest : MonoBehaviour {
	private Vector3 direction = new Vector3(1,0,0);//前进及射线方向//
	private float distance = 1;//射线距离//

	void Update () {
		transform.position += direction * Time.deltaTime;//玩家向右移动//
		Ray ray = new Ray(transform.position, transform.forward);//新建射线,第一个参数为射线的起点方向,第二个参数为射线的方向//
		RaycastHit info;//接受射线撞击信息变量//
		//发射射线方法1//
		if(Physics.Raycast(ray,out info,distance))
		{
			//发现障碍//
			Debug.Log("前方有障碍"+info.collider.name);
		}
		Debug.DrawLine(ray.origin,ray.origin + direction*distance);//用drawline显示射线//
//		if(Physics.Raycast(transform.position,new Vector3(1,0,0),5))
//		{
//			Debug.Log("前方有障碍");
//		}
//		if(Physics.Raycast(transform.position,transform.right,5))
//		{
//			Debug.Log("前方有障碍");
//		}
	}
}
```
* vector3(1,0,0)=transform.right
* Raycast返回一个bool值
* info储存碰撞信息（碰到什么东西了，在说明位置碰到的）
* 也可以不创建射线，通过“(Physics.Raycast(transform.position,transform.right,5)”来传入射线的各种属性