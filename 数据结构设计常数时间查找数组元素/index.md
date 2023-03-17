# 【数据结构设计】常数时间查找数组元素


## 1 题目

**力扣 [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)**

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

## 2 解析

### 2.1 LRU算法设计

由于 put 和 get 方法的时间复杂度为 O(1)，我们可以总结出 cache 这个数据结构必要的条件：  

> 🔴 cache 中的元素必须有时序，以区分最近使用的和久未使用的数据  
>
> 🟡 能在 cache 中快速找某个 key 是否已存在并得到对应的 val  
>
> 🟢 cache 要支持在任意位置快速插入和删除元素，便于访问某个 key 并将这个元素变为最近使用的

哈希表查找快，但是数据无固定顺序；链表有顺序之分，插入删除快，但是查找慢。所以结合一下，形成一种新的数据结构：**哈希链表 LinkedHashMap**。  

LRU 缓存算法的核心数据结构就是**哈希链表**，**双向链表和哈希表的结合体**。 如图所示：

![img1](/img/labuladong/6-1.png)

根据这个结构分析以上 3 个条件：

> 🟥 每次从链表尾部添加元素，则尾部就是最近使用的，头部就是最久未使用的
>
> 🟨 通过哈希表可以根据 key 快速获得 val
>
> 🟩 用哈希表将 key 快速映射到链表节点后，可以很方便的进行插入、删除操作

两个问题：

> ❓ 用单链表取代双链表行不行？
>
> ❓ 哈希表中存了 key，链表中为什么还要存 key 和 val？可以只存 val 吗？



### 2.2 代码实现

首先写出双链表节点类型 Node：

```java
class RandomizedSet {
public:
    vector<int> ve;
    unordered_map<int, int> ma; // 键是元素值，值是元素在ve中的索引
    RandomizedSet() {

    }
    
    bool insert(int val) {
        if(ma.find(val) == ma.end()){   // 注意条件！别写反了
            ma[val]=ve.size();  // 注意添加键值对的写法
            ve.push_back(val);
            return true;
        }
        return false;
    }
    
    bool remove(int val) {
        if(ma.find(val) != ma.end()){
            ma[ve.back()]=ma[val];  // 更新 ma 中的索引
            swap(ve[ma[val]],ve.back());    // 更新ve中的两个元素
            ve.pop_back();  // 删除ve中的元素
            ma.erase(val);  // 删除ma中的元素
            return true;
        }
        return false;
    }
    
    int getRandom() {
        return ve[rand() % ve.size()];
    }
};
```


