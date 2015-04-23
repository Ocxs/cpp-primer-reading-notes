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
写代码的时候发现如果去掉`sort`排序，出来的结果跟预计结果差很多，翻书一看，发现书上说了`unique`将**相邻的重复项“消除”**，当然这里也不是真消除，覆盖了相邻元素，使得不重复元素出现在序列开始部分，`unique`返回的迭代器指向最后一个不重复元素之后的位置。这里我测试输出了返回的迭代器之后的元素，发现并没有任何规律，只是输出了部分元素。（中文版P343）

---
### exercise 10.18
题目要求用`partition`算法来做，刚开始直接这么写的`auto wc = partition(words.begin(), words.end(), isLonger5);` 但是出来结果与预期不一样，查看10.13题目描述才知道，**`partition`算法将容器内容进行划分，使谓词为`true`的值排在容器的前半部分，为`false`的排在后半部分**，所以原因也就出来了，我们需要更改`isLonger5`函数，之前用`find_if`的时候`isLonger5`的函数是这样的
```cpp
bool isLonger5(const string &s1)
{
	return s1.size() > 4;
}
```
现在需要改成这样才能行
```cpp
bool isLonger5(const string &s1)
{
	return s1.size() <= 4;
}
```
小细节问题。

---
### exercise 10.23
本题使用bind函数需要有几个注意的地方，举例说明
```cpp
bool check_size(const string &s, string::size_type sz)
{
	return s.size() >= sz;
}


auto check6 = bind(check_size,_1,6);
```
其中的`_1`是定义在一个名为`placeholders`的命名空间中，而这个命名空间本身定义在`std`中，所以我们要使用形如`_n`的名字，需要使用如下方法:
```cpp
1.using namespace std::placeholders;//可以只用_1,_2,_3····_n
2.using std::placeholders::_1;	//只能使用_1
```
调用bind的一般形式为：
`auto newCallable = bind(callable, arg_list);//形如_n的参数可能就在arg_list参数列表中`    
上面举例的`check6`中的`_1`我们称之为占位符，`bind`调用只有一个**占位符**，表示`check6`只接受一个参数，`check_size`和`check6`都是可调用对象，调`用check6`的时候，`check6`会调用`check_size`,并传入`arg_list`中的参数。

---
### exercise 10.24
另外一种解法
```cpp
bool ex10_22C(const string &s, string::size_type sz)
{
	return s.size() >= sz;
}
int main()
{
	vector<int> ivec{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	auto ex10_24 = bind(ex10_22C, _1, _2);
	string str("C++");
	int n = 0;
	for (auto c : ivec)
	{
		if (!ex10_24(str,c))
		{
			n = c;
			break;
		}
	}
	cout << n << endl;
	return 0;
}
```
---
### exercise 10.27
pezy答案中方法更好，更简单。
```cpp
unique_copy(ivec.begin(), ivec.end(), back_inserter(ls));
```
查阅[资料](http://www.cplusplus.com/reference/algorithm/unique_copy/?kw=unique_copy)
我们可以看到unique_copy返回的是拷贝不同元素的最后一个位置(An iterator pointing to the end of the copied range, which contains no consecutive duplicates.),cplusplus上的示例代码，可以看出返回值
```cpp
template <class InputIterator, class OutputIterator>
  OutputIterator unique_copy (InputIterator first, InputIterator last,
                              OutputIterator result)
{
  if (first==last) return result;

  *result = *first;
  while (++first != last) {
    typename iterator_traits<InputIterator>::value_type val = *first;
    if (!(*result == val))   // or: if (!pred(*result,val)) for version (2)
      *(++result)=val;
  }
  return ++result;
}
```

---
### exercise 10.30
这里有两种方法来构造ivec;如下：
```cpp
1.	vector<int> ivec(in_iter,eof);	//这两种方法都能行，这是从迭代器范围构造vec
	
2. 	while ( in_iter != eof)
 	{
 		ivec.push_back(*in_iter++);
 	}
```

--- 
### exercise 10.31
我的笨办法：新建一个`ivec2`作为`unique_copy`的存储容器，然后用使用流迭代器遍历`ivec2`的元素
pezy的方法：直接来大招：`unique_copy(ivec.begin(), ivec.end(), ostream_iterator<int>(cout, " "));`

---
### exercise 
