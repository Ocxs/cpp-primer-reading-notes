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

---
### exercise 10.4
Q:假定`v`是一个`vector<double>`,那么调用`accumulate(v.begin(),v.end(),0);`有何错误？    
A:中文版书上P338下面有句话：序列中的元素的类型必须与第三个参数匹配，或者能够转换成为第三个参数的类型。本题中0是`int`类型，
但是`double`可以转成`int`类型，所以没有错误，但是这样会损失精度，因为`double`转成`int`会舍去小数点后面的值。

---
### exercise 10.7
这一题第一问pezy的答案是用`vec.resize(lst.size());`来解决的，这没有问题，但是结合这一节内容所学，也许出题者的想要的答案应该是这个`copy(lst.cbegin(), lst.cend(), back_inserter(vec));` 能解决问题就好，两者都能debug.

---
### exercise 10.9
写代码的时候发现如果去掉`sort`排序，出来的结果跟预计结果差很多，翻书一看，发现书上说了`unique`将相邻的重复项“消除”，当然这里也不是真消除，覆盖了相邻元素，使得不重复元素出现在序列开始部分，`unique`返回的迭代器指向最后一个不重复元素之后的位置。这里我测试输出了返回的迭代器之后的元素，发现并没有任何规律，只是输出了部分元素。（中文版P343）


