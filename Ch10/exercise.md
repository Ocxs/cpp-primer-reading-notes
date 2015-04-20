## 如有错误，跪求指点！
---

### exercise 10.1

```cpp
	vector<int> ivec;
	int element = 0;
	while (cin >> element)
	{
		ivec.push_back(element);
	}
	int n = 0;
	cin.clear();
	while (cin >> n)
	{
		int num = count(ivec.begin(), ivec.end(), n);
		cout << n << " occurs " << num << " times" << endl;
	}
```

刚开始写这道题没有在两个`while`循环之间没有加	`cin.clear();`，导致按 “CTRL + Z”的时候，第一个`while`循环和第二个`while`循环
一并结束翻看了第八章的知识，知道了“CTRL + Z” 改变了`cin`的状态，这里应该是改变了`eofbit`，导致第二个`while`循环里的`cin`的状态
没有复位，所以`while`循环里的`cin`没有成功读取值，加上`cin.clear();`复位就能成功读取值了.
