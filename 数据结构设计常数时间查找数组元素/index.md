# 【数据结构设计】常数时间查找数组元素


## 1 题目

**力扣 [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)**

实现`RandomizedSet` 类：

- `RandomizedSet()` 初始化 `RandomizedSet` 对象
- `bool insert(int val)` 当元素 `val` 不存在时，向集合中插入该项，并返回 `true` ；否则，返回 `false` 。
- `bool remove(int val)` 当元素 `val` 存在时，从集合中移除该项，并返回 `true` ；否则，返回 `false` 。
- `int getRandom()` 随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有 **相同的概率** 被返回。

你必须实现类的所有函数，并满足每个函数的 **平均** 时间复杂度为 `O(1)` 。

## 2 解析

考虑题目的两个要求：  

🔴 插入、删除、查询随机元素的时间复杂度必须都是 O(1)。  想到使用 STL 中的 map 结构。

🟡 getRandom() 必须等概率的返回随机元素。 那么底层必须用数组实现，且数组是紧凑的，这样就可以直接生成随机数作为数组索引。

🟢  综合考虑以上2个条件：在 O(1) 的时间删除数组中的某⼀个元素 val，可以先把这个元素交换到数组的尾部，然后再 pop 掉。而交换两个元素需要知道索引，故用哈希表存储每个元素及其索引。 

代码实现：

```c++
class RandomizedSet {
public:
    // 存储元素的值
    vector<int> ve;
    // 键是元素值，值是元素在ve中的索引
    unordered_map<int, int> ma; 
    RandomizedSet() {

    }
    
    bool insert(int val) {
        // 若 val 不存在，则插入并返回true
        if(ma.find(val) == ma.end()){   // 注意条件！别写反了
            // 并记录 val 对应的索引值，注意添加键值对的写法
            ma[val]=ve.size();  
            ve.push_back(val);
            return true;
        }
        return false;
    }
    
    bool remove(int val) {
        if(ma.find(val) != ma.end()){
            // 将最后⼀个元素对应的索引修改为 ma[val]
            ma[ve.back()]=ma[val];  
            // 交换 val 和最后⼀个元素
            swap(ve[ma[val]],ve.back());    
            // 在数组中删除元素 val
            ve.pop_back();  
            // 删除元素 val 对应的索引
            ma.erase(val);  
            return true;
        }
        return false;
    }
    
    int getRandom() {
        // 随机获取 nums 中的⼀个元素
        return ve[rand() % ve.size()];
    }
};
```

