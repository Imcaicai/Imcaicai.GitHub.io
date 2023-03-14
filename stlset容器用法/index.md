# 【STL】set容器用法


## 1 简介

C++ 标准函数库中的 `set` 可以用来存储集合，`set` 里面的元素都是唯一的，不可以重复，可以新增或删除元素，但不可以修改元素的值。

🔴 头文件：`#include <set>`

## 2 初始化

`std::set` 的初始化有三种方式：1️⃣ 以 `insert()` 函數新增元素 2️⃣ 直接在创建时以大括号初始化 `set` 内部的元素 3️⃣ 通过数组初始化。

```c++
// 第 1 种初始化方式
set<int> set1;
set1.insert(1);
set1.insert(2);
set1.insert(3);

// 第 2 种初始化方式
// 注意这里没有 '='
set<int> set2 {1,2,3};

// 第 3 种初始化方式
int num[] = {1,2,3};
set<int> set3(num, num+3);
```

## 3 增/删元素

`std::set` 若要新增、刪除元素，可以使用 `insert()` 和 `erase()` 函数。

```c++
// 新增元素
set1.insert(1);

// 删除元素
set1.erase(1);
```

## 4 查询元素

查询 `std::set` 中是否包含特定的元素，可以使用 `find()` 函数，若成功找到指定的元素，就返回对应的 iterator，而如果没有找到，就返回 `set::end`。

```c++
// 在 set1 中寻找 6 这个元素
set<int>::iterator iter;
iter = set1.find(6);

// 如果找到，就返回正确的 iterator，否则返回 set1.end()
if (iter != set1.end()) {
  cout << "Found: " << *iter << endl;
}
```

## 5 列出所有元素

列出 `std::set` 中的所有元素有2种方式：1️⃣ 标准的 iterator 方式 2️⃣ 新的 `for` 遍历方法

🟠 想要拷贝元素：for(auto x:range)

🟡 想要修改元素：for(auto &&x:range)

🟢 想要只读元素：for(const auto& x:range)

```c++
// 第 1 种列出所有元素的方法
set<int>::iterator iter;
for(iter=set1.begin(); iter!=set1.end(); iter++){
    cout << *iter << endl;	// 注意 endl 不要写成 end
}

// 第 2 种列出所有元素的方法
for(const auto &i : set1){
    std::cout << i << endl;	// 注意这里 i 前面没有 '*'
}
```

## 6 清空所有元素

1️⃣ 清空 `std::set` 中的所有元素： `clear()` 函数 2️⃣ 获取元素个数： `size()` 函数 3️⃣ 检查 `std::set` 是否是空的： `empty()` 函数。

```c++
// 清空所有元素
set1.clear();

// 获取元素个数：
cout << "number of elements:" << set1.size() << endl;

// 检查 set 是否为空
if(set1.empty()){
    cout << "set1 is empty." <<
}
```


