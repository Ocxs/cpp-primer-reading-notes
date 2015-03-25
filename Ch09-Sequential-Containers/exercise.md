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
