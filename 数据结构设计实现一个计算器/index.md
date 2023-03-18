# 【数据结构设计】实现一个计算器


## 1 题目

实现一个计算器，功能如下：

1、输入一个字符串，可以包含 + - * /、数字、括号以及空格，你的算法返回运算结果。

2、要符合运算法则，括号的优先级最高，先乘除后加减。

3、除号是整数除法，无论正负都向 0 取整（5/2=2，-5/2=-2）。

4、可以假定输入的算式⼀定合法，且计算过程不会出现整型溢出，不会出现除数为 0 的意外情况。  

## 2 解析

### 2.1 字符串转整数

```c++
// 把字符串s转为整数n
int n = 0;
for (int i = 0; i < s.size(); i++) {
	char c = s[i];
	n = 10 * n + (c - '0');
}
```

❗❗❗ 注意 (c - '0')  的括号不能省略，否则可能造成整型溢出。  

### 2.2 处理加减法

🟠 先给第⼀个数字加⼀个默认符号 +，变成 +1-12+3。 

🟡 把⼀个运算符和数字组合成⼀对，也就是三对 +1，-12，+3，把它们转化成数字，然后放到⼀个栈 中。 

🟢 将栈中所有的数字求和，就是原算式的结果。

```c++
int calculate(string s) {
	stack<int> stk;
	// 记录算式中的数字
	int num = 0;
	// 记录 num 前的符号，初始化为 +
	char sign = '+';
	for (int i = 0; i < s.size(); i++) {
		char c = s[i];
		// 如果是数字，连续读取到 num
		if (isdigit(c))
		num = 10 * num + (c - '0');
		// 如果不是数字，就是遇到了下⼀个符号，
		// 之前的数字和符号就要存进栈中
		if (!isdigit(c) || i == s.size() - 1) {
			switch (sign) {
				case '+':
					stk.push(num); break;
				case '-':
					stk.push(-num); break;
			}
			// 更新符号为当前符号，数字清零
			sign = c;
			num = 0;
		}
 	}
 	// 将栈中所有结果求和就是答案
 	int res = 0;
 	while (!stk.empty()) {
 		res += stk.top();
 		stk.pop();
 	}
 	return res;
}
```

### 2.3 处理乘除法

思路跟仅处理加减法类似，其他部分都不变，只需要在 switch 部分加上对应的 case 。

```c++
for (int i = 0; i < s.size(); i++) {
 	char c = s[i];
 	if (isdigit(c))
	num = 10 * num + (c - '0');
 	if (!isdigit(c) || i == s.size() - 1) {
 		switch (sign) {
 			int pre;
 			case '+':
 				stk.push(num); break;
 			case '-':
 				stk.push(-num); break;
 				// 只要拿出前⼀个数字做对应运算即可
 			case '*':
 				pre = stk.top();
 				stk.pop();
 				stk.push(pre * num);
 				break;
 			case '/':
 				pre = stk.top();
 				stk.pop();
 				stk.push(pre / num);
 				break;
 		}
 		// 更新符号为当前符号，数字清零
 		sign = c;
 		num = 0;
 	}
}
```

当遇到空格时，只需要控制空格不进入 if 条件：

```c++
if ((!isdigit(c) && c != ' ') || i == s.size() - 1) {
 	...
}
```

### 2.4 处理括号

💥 **括号具有递归性质。** 我们只需在遍历时，遇到  `(`  开始递归，遇到  `)`  结束递归。

```Python
def calculate(s: str) -> int:

 	def helper(s: List) -> int:
 		stack = []
 		sign = '+'
 		num = 0
 		while len(s) > 0:
 			c = s.popleft()
 			if c.isdigit():
 				num = 10 * num + int(c)
 			# 遇到左括号开始递归计算 num
 			if c == '(':
				num = helper(s)
 			if (not c.isdigit() and c != ' ') or len(s) == 0:
 				if sign == '+':
 					stack.append(num)
	 			elif sign == '-':
 					stack.append(-num)
 				elif sign == '*':
 					stack[-1] = stack[-1] * num
 				elif sign == '/':
 					# python 除法向 0 取整的写法
 					stack[-1] = int(stack[-1] / float(num))
 				num = 0
 				sign = c
 			# 遇到右括号返回递归结果
 			if c == ')': break
 		return sum(stack)
    
 return helper(collections.deque(s))
```


