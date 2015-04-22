以下内容是我在做课后习题时遇到的问题以及一些随笔，有时候怕以后忘了，所以写下来帮助自己理解记忆，所以如果其他人看的话须保留自己
意见，因为我也可能描述有错，如果有人看，并且看到错误，跪求指点！

## exercise 9.14 
> Write a program to assign the elements from a list of char*  pointers to C-style character strings to a
vector of strings.

这里有个坑，用中文版的时候，翻译是赋值，我自己在写的时候，一不小心就写成下面这样了
```cpp
	list<char*> list1{ "cpp-primer" };
	vector<string> svec(list1.begin(),list1.end());//注意，这里题目要求是赋值，不是初始化，所以这样写不符合题目要求，虽然也是对的
```

然后我在写的时候，又犯了一个错误， `list<char*> list1{ 'c','+','+' };`这样写的时候我看vs2013老师在花括号的位置老是提示一个红色
波浪线，说明这里有错，后来想想才发现，这里的类型是`char*`,不是char，所以要写成双引号，表示这是一个字符串，而不是一个字符。
此外假设这里是char的话，那么我这样写`list<char*> list1{ 'c','+','+' };`，也不是太好，因为没有结束标志。
C++中，
字符数组的特殊性，当用字符串字面值对此类数组初始化，一定要注意字符串字面值的结尾处还有一个空字符，
这个空字符也会像字符串的其他字符一样被拷贝到字符数组里
```cpp
char a1[] = {'C','+','+'};              //列表初始化，没有空字符
char a2[] = {'C','+','+','\0'};         //列表初始化，含有显示的空字符
char a3[] = "C++";                      //自动添加表示字符串结束的空字符
const char a4[6] = "123456";            //错误：没有空间存放空字符
```
a1维度是3，a2和a3维度都是4，a4定义错误，因为其维度至少是7，需要额外的存放'\0'

以上内容摘自 c++primer 第五版（中文版）

---
## exercise 9.16
题目：
> compare elements in a list<int> to a vector<int>.

我自己写的是for循环比较，一看pezy的答案，觉得人家的技高一筹,此方法应该记住。还应该记住要经常使用`()? :`而不是使用`if else`.
```cpp
std::cout << (std::vector<int>(vec1.begin(), vec1.end()) == vec2 ? "true" : "false")
              << std::endl;
```

---
## exercise 9.18
一般人写这道题在输入的时候肯定跟我差不多
```cpp
	while (cin >> str)
	{
		input.push_back(str);
	}
```
不过另外一种也可以，希望可以记住。
```cpp
for (string str; cin >> str; deque1.push_back(str));//这个写法也可以
```
---
## exercise 9.20
pezy的方法比较不错
```cpp
for (auto iter = l.cbegin(); iter != l.cend(); ++iter)
        (*iter & 0x1 ? d_odd : d_even).push_back(*iter);
```
不过根据他的代码，我在简洁一点，如下：
```cpp
	for (const auto &c : list1)
		(c & 0x1 ? deque1 : deque2).push_back(c);
```
这里不一定非要将1写成16进制，直接写1，运行结果依然正确，毕竟只是和最后一位相与。

---
## exercise 9.22
> Assuming iv is a vector of ints, what is wrong with the following program? How might you correct the problem(s)?
 ```cpp
vector<int>::iterator iter = iv.begin(), mid = iv.begin() + iv.size()/2;
while (iter != mid)
    if (*iter == some_val)
        iv.insert(iter, 2 * some_val);
```

这一题当时做的时候只考虑到了死循环，没考虑到mid会失效,中文版page315说了：
> 向容器添加元素后，如果容器是vector或string，且存储空间被重新分配，则指向容器的迭代器，指针和引用都会失效。如果存储空间没有失效（capacity没有变）指向插入插入位置之前的元素的迭代器，指针和引用仍有效，但是指向插入位置后的元素的迭代器，指针和引用都会失效。

这里插入元素的位置在mid之前，所以mid会失效。具体可以参见[这里](https://github.com/Ocxs/Cpp-Primer/tree/master/ch09)exercise 9.22

---
## exercise 9.28
本题解法书上也有类似代码，但是书上中间有部分是这么写的
```cpp
prev = curr;
++curr;
```
参考pezy的写法，可以将这一段更加精简
```cpp
prev = curr++;
```

---
## exercise 9.52
`auto buff = static_cast<char>(0);` 与 `auto buff = char(0);` 一样，只不过后者`char(0)`是c语言中的一种强制转换.
另外本题答案也重新 改写，请参看[这里](https://github.com/pezy/CppPrimer/pull/31)
