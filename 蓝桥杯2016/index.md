# 【蓝桥杯】2016题解


⏰总用时：146/240			🎯总分：66/100

| 题号   | 时间 | 结果 | 满分 | 难度 | 备注                                             |
| ------ | ---- | ---- | ---- | ---- | ------------------------------------------------ |
| **1**  | 3    | ✅    | 3    | 🌕    | 🔸 计算完一定要带入样例检验，漏加了 1 ❗❗❗         |
| **2**  | 3    | ✅    | 5    | 🌕    |                                                  |
| **3**  | 40   | ❌    | 11   | 🌓    | 🔸 直接用 next_permutation(a,a+10) 来枚举所有可能 |
| **4**  | 5    | ✅    | 9    | 🌕    |                                                  |
| **5**  | 10   | ✅    | 13   | 🌓    | 🔸 消除末尾连续的 1 ： `x&(x+1)`                  |
| **6**  | 35   | ✅    | 15   | 🌕    | 🔹 dfs                                            |
| **7**  | 30   | ❌    | 19   | 🌓    | 🔸 模拟<br/>🔸                                     |
| **8**  | 10   | ✅    | 21   | 🌕    | 🔹 并查集（不过没用）                             |
| **9**  | 5    | ❌    | 25   | 🌓    |                                                  |
| **10** | 5    | ❌    | 29   | 🌓    |                                                  |

## 1 【填空】网友年龄

### 题目

某君新认识一网友。 当问及年龄时，他的网友说： “我的年龄是个 2 位数，我比儿子大 27 岁, 如果把我的年龄的两位数字交换位置，刚好就是我儿子的年龄”

请你计算：网友的年龄一共有多少种可能情况？

提示：30 岁就是其中一种可能哦。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int cnt=0;
	for(int i=1;i<=9;i++){
		for(int j=0;j<=9;j++){
			if(i*10+j-j*10-i==27){
				cout<< i*10+j <<" ";
				cnt++;
			}
		}
	}
	
	cout<<endl<<cnt;
} 
```

## 2 【填空】生日蜡烛

### 题目

某君从某年开始每年都举办一次生日 party，并且每次都要吹熄与年龄相同根数的蜡烛。现在算起来，他一共吹熄了 236 根蜡烛。

请问，他从多少岁开始过生日 party 的？请输出他开始过生日 party 的年龄数。

请输出他开始过生日 �����*p**a**r**t**y* 的年龄数。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){

	for(int i=1;i<=200;i++){
		for(int j=i;j<=200;j++){
			if((i+j)*(j-i+1)/2==236){
				cout<< i<<" "<<j;
				return 0;
			}
		}
	}
	
	return 0;
} 
```

## 3 【填空】方格填数

### 题目

如下的 10 个格子，填入 0 ~ 9 的数字。要求：连续的两个数字不能相邻。 （左右、上下、对角都算相邻）

一共有多少种可能的填数方案？

```
   +--+--+--+
   |  |  |  |
+--+--+--+--+
|  |  |  |  |
+--+--+--+--+
|  |  |  |
+--+--+--+
```

### 解析

🟠 直接用 next_permutation(a,a+10) 来枚举所有可能

🟡 check 的条件比较多，小心下标标错❗❗❗

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int cnt=0;
	int a[10]={0,1,2,3,4,5,6,7,8,9};
	do{
		if(abs(a[0]-a[1])!=1 && abs(a[0]-a[3])!=1 && abs(a[0]-a[4])!=1 && abs(a[0]-a[5])!=1 &&
		abs(a[1]-a[2])!=1 && abs(a[1]-a[4])!=1 && abs(a[1]-a[5])!=1 && abs(a[1]-a[6])!=1 &&
		abs(a[2]-a[5])!=1 && abs(a[2]-a[6])!=1 && abs(a[3]-a[4])!=1 && abs(a[3]-a[7])!=1 &&
		abs(a[3]-a[8])!=1 && abs(a[4]-a[5])!=1 && abs(a[4]-a[7])!=1 && abs(a[4]-a[8])!=1 &&
		abs(a[4]-a[9])!=1 && abs(a[5]-a[6])!=1 && abs(a[5]-a[8])!=1 && abs(a[5]-a[9])!=1 &&
		abs(a[6]-a[9])!=1 && abs(a[7]-a[8])!=1 && abs(a[8]-a[9])!=1)	cnt++;
	}while(next_permutation(a,a+10));
	cout<<cnt;
	
	return 0;
}
```

## 4 【填空】快速排序

### 题目

以下代码可以从数组 a[ ] 中找出第 k 小的元素。

它使用了类似快速排序中的分治算法，期望时间复杂度是 O(N) 的。

请仔细阅读分析源码，填写划线部分缺失的内容。

### 解析

```c++
#include <stdio.h>

int quick_select(int a[], int l, int r, int k) {
    int p = rand() % (r - l + 1) + l;
    int x = a[p];
    {int t = a[p]; a[p] = a[r]; a[r] = t;}
    int i = l, j = r;
    while(i < j) {
        while(i < j && a[i] < x) i++;
        if(i < j) {
            a[j] = a[i];
            j--;
        }
        while(i < j && a[j] > x) j--;
        if(i < j) {
            a[i] = a[j];
            i++;
        }
    }
    a[i] = x;
    p = i;
    if(i - l + 1 == k) return a[i];
    if(i - l + 1 < k) return quick_select(a,l+1,i,k); //填空
    else return quick_select(a, l, i - 1, k);
}
    
int main()
{
    int a[100];
    int n;
    scanf("%d %d",&n);
    for(int i=0;i<n;i++)
    scanf("%d",&a[i]);
    printf("%d\n", quick_select(a, 0, n-1, 5));
    return 0;
}
```

## 5 【填空】消除尾一

### 题目

下面的代码把一个整数的二进制表示的最右边的连续的1全部变成0 如果最后一位是0，则原数字保持不变。

请仔细阅读程序，填写划线部分缺少的代码。

### 解析

🟠 消除末尾连续的 1 ： `x&(x+1)`

```c++
#include <stdio.h>

void f(int x)
{
    int i;
    for(i=0; i<32; i++) printf("%d", (x>>(31-i))&1);
    printf("....");
    
    x =  x&(x+1);
    
    for(i=0; i<32; i++) printf("%d", (x>>(31-i))&1);
    printf("\n");    
}

int main()
{
    f(128+64+2);
    f(128+64+15);
    f(128+64+1);
    
    return 0;
}
```

## 6 【填空】寒假作业

### 题目

现在小学的数学题目也不是那么好玩的。 看看这个寒假作业：

```
   □ + □ = □
   □ - □ = □
   □ × □ = □
   □ ÷ □ = □
```

每个方块代表 1~13 中的某一个数字，但不能重复。（加法，乘法交换律后算不同的方案）

你一共找到了多少种方案？

### 解析

🟠 【dfs】挨个遍历每个位置，看填什么数字

```c++
#include <bits/stdc++.h>
using namespace std;

int cnt=0;
bool v[14]={false};		// 表示这个位置有没有遍历过 
int a[15]={0};
void dfs(int x){		// 当前正决定位置 x 放哪个数 
	if(x==14){			// 遍历到底，此情况成立 
		cnt++;return;
	}
	
	if(x==4 && a[1]+a[2]!=a[3])	return;
	if(x==7 && a[4]-a[5]!=a[6])	return;
	if(x==10 && a[7]*a[8]!=a[9])	return;
	if(x==13 && a[11]*a[12]!=a[10])	return;
	
	for(int i=1;i<=13;i++){		// 遍历所有的位置 
		if(v[i]==false){		// 这个位置没有放数字，可以放 x 
			v[i]=true;
			a[x]=i;				// x 这个位置放了 i
			dfs(x+1);			// 继续看下一个位置放什么数字 
			v[i]=false;
		}
	}
}

int main(){
	dfs(1);
	cout<<cnt; 
	
	return 0;
}
```

## 7 【填空】剪邮票

### 题目

如图, 有12张连在一起的12生肖的邮票。



现在你要从中剪下 5 张来，要求必须是连着的。（仅仅连接一个角不算相连）

请你计算，一共有多少种不同的剪取方法。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

struct u{
    map<int,int> t;        					// 表示用户有订单的时刻和订单数量 
};
u user[100005];

int main(){
    int n,m,t,ts,id,cnt=0;
    map<int,int>::iterator it;
    cin>>n>>m>>t;
    for(int i=0;i<m;i++){        			// 输入订单信息 
        scanf("%d %d",&ts,&id);
        (user[id].t[ts])++;
    }
    
    for(int i=1;i<=n;i++){
        int s=0,t1=0,t2=0,flag=0;
        for(it=user[i].t.begin();it!=user[i].t.end();it++){
            t1=t2;t2=it->first;
            if(s-(t2-t1-1) <0)    s=0;		// 注意这里的 s 应该减去多少！！！ 
            else s-=(t2-t1-1);
            if(s>5)    flag=1;				// 注意这里也要判断一下！！！ 
            if(s<=3 && flag)    flag=0;
            
            s+=(it->second*2);
            if(s>5)    flag=1;
            if(s<=3 && flag)    flag=0;
        }
        if(user[i].t.count(t)!=1)
            s-=(t-t2);
        if(s>5)    flag=1;
        if(s<=3 && flag)    flag=0;
        if(flag)    cnt++;
    } 
    
    cout<<cnt;
    return 0;
}
```

## 8 四平方和

### 题目

给定一个长度为 n 的数组 $A=[A_1,A_2,...A_n]$ ，数组中有可能有重复出现的整数。

现在小明要按以下方法将其修改为没有重复整数的数组。小明会依次修改 $A_2,A_3...A_n$ 。

当修改 $A_i$ 时，小明会检查 $A_i$ 是否在 $A_1...A_{i-1}$ 中出现过。如果出现过，则小明会给 $A_i$ 加上 1 ；如果新的 $A_i$ 仍在之前出现过，小明会持续给 $A_i$ 加 1 ，直 到 $A_i$ 没有在 $A_1...A_{i-1}$ 中出现过。

当 $A_n$ 也经过上述修改之后，显然 A 数组中就没有重复的整数了。

现在给定初始的 A 数组，请你计算出最终的 A 数组。

**输入描述**

第一行包含一个整数 n 。

第二行包含 n 个整数 $A_1,A_2,...A_n$ 。

其中，$1≤ n ≤ 10^5, 1≤ A_i ≤ 10^6$ 。

**输出描述**

输出 n 个整数，依次是最终的 $A_1,A_2,...A_n$ 。

### 解析

🟠 用 map 等 STL 容器容易超时，这道题不用可以过 80% 的样例，用了只能过 40% 。非必要不使用❗❗❗

🟡 **dev 数组开大会爆栈 ❗❗❗ 可以用 static 或者修改分配的栈大小 ❗❗❗**

🟢 过80%的简单代码（会超时）：

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	cin>>n;
	for(int i=0;i*i<=n;i++){
		for(int j=i;i*i+j*j<=n;j++){
			for(int k=j;i*i+j*j+k*k<=n;k++){
				for(int r=k;i*i+j*j+k*k+r*r<=n;r++){
					if(i*i+j*j+k*k+r*r==n){
						cout<<i<<" "<<j<<" "<<k<<" "<<r;
						return 0;
					}
				}
			}
		}
	}
	
	return 0;
}
```

## 9 密码脱落

### 题目

糖果店的老板一共有 m 种口味的糖果出售。为了方便描述，我们将 m 种口味编号 1∼ m。

小明希望能品尝到所有口味的糖果。遗憾的是老板并不单独出售糖果，而是 k 颗一包整包出售。

幸好糖果包装上注明了其中 k 颗糖果的口味，所以小明可以在买之前就知道每包内的糖果口味。

给定 n 包糖果，请你计算小明最少买几包，就可以品尝到所有口味的糖果。

**输入描述**

第一行包含三个整数 n,m,k。

接下来 n 行每行 k 个整数 $t_1,t_2,...t_k$ ，代表一包糖果的口味。

其中，1≤n≤100, 1≤m≤20, 1≤k≤20, $1≤t_i≤m$ 。

**输出描述**

输出一个整数表示答案。如果小明无法品尝所有口味，输出 −1。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n,m,k,t;
	set<int> s;						// 用于判断 n 包糖的总种类数 
	cin>>n>>m>>k;
	int a[105];						// 每包糖的状态
	static int dp[1<<20+1];			// 每个状态所需的最小包数 
	
	// 1. 输入并存储每包糖的状态 
	for(int i=1;i<=n;i++){			// 遍历每包糖
		int x=0,y;
		for(int j=1;j<=k;j++){		// 遍历一包糖的每颗糖 
			cin>>y;
			x |= (1<<(y-1));		// 加上这颗糖的状态 
			s.insert(y);			// 更新总种类 
		}
		a[i]=x; 
	}
	
	// 2. 判断总共是否有 m 种糖，如果没有则输出 -1 并退出
	if(s.size()<m){
		cout<< -1;	return 0;
	} 
	
	// 3. 动态规划找出每个状态所需的最小包数
	fill(dp,dp+(1<<20)+1,0xffff);	// 初始化 
	dp[0]=0;						// 设置 base case 
	t=(1<<m)-1;						// m 个 1 
	for(int i=1;i<=n;i++){
		for(int j=t;j>=0;j--)		// 遍历每个状态，决定要不要加这包糖 
			dp[j|a[i]]=min(dp[j|a[i]],dp[j]+1);
	}
	
	cout<<dp[t];
	return 0;
}
```

## 10 最大比例

