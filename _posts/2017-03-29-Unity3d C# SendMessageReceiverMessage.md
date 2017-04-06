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