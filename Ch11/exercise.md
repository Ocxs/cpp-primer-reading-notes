# 如有错误，欢迎指正
---

### exercise 11.7
pezy答案中使用了`lambda`表达式，
```cpp
	while ([&]() -> bool
	{
		std::cout << "Please enter last name:\n";

		return std::cin >> lastName && lastName != "@q";
	}())
	{
		std::cout << "PLZ Enter children's name:\n";
		while (std::cin >> chldName && chldName != "@q")
		{
			family[lastName].push_back(chldName);
		}
		cin.clear();
	}
```

这里`lambda`捕获列表`[&]`为隐藏捕获列表，表示采用引用捕获方式。`lambda`体中所使用的来自函数的实体都采用引用方式使用.还有就是记住`while`判断条件中使用`lambda`表达式当然lambda捕获列表还有其他形式，具体请看中文版primer第五版P352.
