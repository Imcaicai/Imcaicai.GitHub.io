# 【CSP】202209题解


## 1 如此编码

🔗 **题目：[如此编码](http://118.190.20.162/view.page?gpid=T153)**

本来看到题目很迷茫来着，想着第一题怎么就那么难，后来发现题目最后有提示，看完之后醍醐灌顶， **【取模】** 确实是很妙的思路，可以积累下来。

![img1](/img/CSP/1.png)

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int n,m,a[30],c[30],b,t=0;
	c[0] = 1;
	scanf("%d %d",&n,&m);
	for(int i=1;i<=n;i++){
		scanf("%d",&a[i]);
		c[i] = c[i-1]*a[i];
	}
	for(int i=1;i<=n;i++){
		b = (m%c[i]-t)/c[i-1];
		t += c[i-1]*b;
		printf("%d ",b);
	}
	return 0;
} 
```



## 2 何以包邮

🔗 **题目：[何以包邮](http://118.190.20.162/view.page?gpid=T152)**

🔴 **【动态规划】** 设 **s** 为所有参考书价格总和，题目可以理解为在 **s-x** 价格内，最大化被删除的书价格总和，这样就可以把这个问题看作经典的 **01背包问题** 。

🔵 注意动态规划的基本做法：设数组；计算base case；写出动态转移方程。

🟡 `dp[i][j]` ：在a0~ai编号中选书，得到不超过 j 的最大书价。

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	// 输入 
	int n,x,s=0,t,a[40];
	scanf("%d %d",&n,&x);
	for(int i=0;i<n;++i){
		scanf("%d",&a[i]);
		s += a[i];
	}
	t = s-x;
	
	// 动态规划：计算base case 
	int dp[n][t+1]={0};
	for(int i=0;i<=t;i++)
		dp[0][i] = a[0]<i ? a[0] : 0;
		
	// 动态规划：状态转移 
	for(int i=1;i<n;++i){
		for(int j=0;j<=t;++j){
			if(j<a[i])	dp[i][j] = dp[i-1][j];
			else dp[i][j] = max(dp[i-1][j],dp[i-1][j-a[i]]+a[i]);
		}
	}
	
	printf("%d",s-dp[n-1][t]);
	return 0;
} 
```



## 3 防疫大数据

🔗 **题目：[防疫大数据](http://118.190.20.162/view.page?gpid=T151)**

这题不算难，但要选好数据结构。另外，题目好绕，读懂题意很重要！

🔴 **【数据结构】**

-  `struct my{ int d,u,r; };` ：表示一条漫游数据
-  `vector<my> v1` ：保存前一天的风险用户
-  `map<int,set<int>> mp;` ：键：日期，值：当天的风险地区（不仅包括今天的，还包括之前积累的风险地区）

🔵 **【解题步骤】**

- 更新风险地区：每次删除7天外的风险地区信息，节省空间；每天的风险地区不仅包括当天新增的，还有7天内累积的风险地区
- 筛掉不在7天内的风险用户。（这些用户访问的地区都是 [d,i-1] 天内连续风险的）
- 新增今日的风险用户。（注意：严格判断用户访问的地区在 [d,i] 天内是否都为风险的；同时访问记录在7天内）

```c++
#include<bits/stdc++.h>
using namespace std;

struct my{					// 一条漫游数据 
	int d,u,r;
};
vector<my> v1;				// 前一天的风险用户
map<int,set<int>> mp;		// 键：日期，值：当天的风险地区 
int n,ri,mi,p,d,u,r;
 
int main(){
	scanf("%d",&n);					// 总天数
	for(int i=0;i<n;i++){
		scanf("%d %d",&ri,&mi);		// 风险地区数、漫游数据数
		
		// 更新风险地区信息 
		if(i-7>=0) mp.erase(i-7);	// 删除超过7天的风险地区信息 
		for(int j=0;j<ri;j++){
			scanf("%d",&p);			// 风险地区
			for(int k=i;k<i+7;k++)
				mp[k].insert(p);		 
		} 
		
		// 筛掉前一天中不再有风险的用户 
		vector<my> v2;
		for(auto v:v1){
			if(i-v.d>=7) continue;	// 7天外的数据，忽略
			if(mp[i].count(v.r)) v2.push_back(v); 
		}
		v1.clear();v1=v2; 
		
		// 更新今日新增的风险用户
		for(int j=0;j<mi;j++){
			scanf("%d %d %d",&d,&u,&r);
			if(d<=i-7) continue;		// 7天外的数据，忽略
			int flag=1;
			for(int k=d;k<=i;k++){
				if(!mp[k].count(r)){	// [d,i]有1天不是风险，就排除 
					flag=0;break;
				}
			} 
			if(flag) v1.push_back({d,u,r});
		} 
		
		// 输出结果
		set<int> ans;
		for(auto v:v1) ans.insert(v.u);
		printf("%d ",i);
		for(auto &v:ans) printf("%d ",v);
		printf("\n");
	} 
	return 0;
}
```


