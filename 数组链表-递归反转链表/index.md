# 数组链表-递归反转链表


## 1 递归反转整个链表

### 题目

**力扣 [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)**

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

示例：

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

### 解析

图解：

![img1](/img/labuladong/4-1.png)

代码实现：

```java
public ListNode reverseList(ListNode head) {
	if(head==null || head.next==null){
		return head;
	}
        
	ListNode last=reverseList(head.next);
	head.next.next=head;
	head.next=null;
	return last;
    }
```

2个注意点：

🟡 **递归函数要有 base case，如果链表为空或者只有⼀个节点的时候，反转结果就是它自己。**

```java 
if (head == null || head.next == null) {
	return head;
}
```

🟢 **当链表递归反转之后，新的头结点是 last，而之前的 head 变成了最后⼀个节点，别忘了链表的末尾要指向 null。**

```java
head.next = null;
```



## 2 递归反转前 N 个节点

图解：

![img1](/img/labuladong/4-2.png)

代码实现：

```java
ListNode successor = null; // 后驱节点
// 反转以 head 为起点的 n 个节点，返回新的头结点
ListNode reverseN(ListNode head, int n) {
	if (n == 1) {
		// 记录第 n + 1 个节点
		successor = head.next;
		return head;
	}
	// 以 head.next 为起点，需要反转前 n - 1 个节点
	ListNode last = reverseN(head.next, n - 1);
	head.next.next = head;
	// 让反转之后的 head 节点和后⾯的节点连起来
	head.next = successor;
	return last;
}
```

2 个区别：

🟠 base case 变为 n == 1，反转⼀个元素，就是它本身，**同时要记录后驱节点**。

🔵 刚才我们直接把 head.next 设置为 null，因为整个链表反转后原来的 head 变成了整个链表的最后⼀个节点。**但现在 head 节点在递归反转之后不⼀定是最后⼀个节点了，所以要记录后驱 successor（第 n + 1 个节点），反转之后将 head 连接上。**  

## 3 递归反转部分节点

### 题目

**力扣 [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)**

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。

请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

示例：

![img1](/img/labuladong/4-3.png)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

### 解析

图解：

![img1](/img/labuladong/4-4.png)

🟣 如果 m == 1，就相当于反转链表开头的 n 个元素，也就是刚刚实现的 reverseN () 。

🟤 如果 m != 1 ，我们把 head 的索引视为 1，即从第 m 个元素开始反转；如果把 head.next 的索引视为 1 ，那么反转的区间应该从第 m - 1 个元素开始；以此类推......

代码实现：

```java
ListNode successor = null;
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if(left==1)
            return reverseN(head,right-left+1);
        head.next=reverseBetween(head.next,left-1,right-1);
        return head;
        
    }
```

💥💥💥 递归操作链表并不高效。和迭代解法相比，虽然**时间复杂度都是 O(N)**，但是迭代解法的空间复杂度是 **O(1)**，而递归解法需要堆栈，空间复杂度是 **O(N)**。  

