# 【蓝桥杯】2015题解


⏰总用时：89/240			🎯总分：73/150

| 题号   | 时间 | 结果 | 满分 | 难度 | 备注                                                         |
| ------ | ---- | ---- | ---- | ---- | ------------------------------------------------------------ |
| **1**  | 3    | ✅    | 3    | 🌕    |                                                              |
| **2**  | 3    | ✅    | 5    | 🌕    | 🔹 在Excel中直接用日期格式拉出答案                            |
| **3**  | 10   | ✅    | 9    | 🌕    | 🔸 直接用next_permutation(a,a+10) 来枚举                      |
| **4**  | 15   | ✅    | 11   | 🌕    | 🔹 注意 printf、scanf 中 %*s 的用法                           |
| **5**  | 1    | ✅    | 15   | 🌕    | 🔸 注意此题 dfs 的用法                                        |
| **6**  | 15   | ✅    | 17   | 🌕    | 🔹 dfs                                                        |
| **7**  | 10   | ❌    | 21   | 🌓    | 🔸 全排列+特殊去重<br/>🔸 去重旋转：判断 t 是否是 s+s 的子串<br/>🔸 在字符串中寻找子串：s.find(t)!=string::npos |
| **8**  | 12   | ✅    | 13   | 🌕    |                                                              |
| **9**  | 15   | ❌    | 25   | 🌓    | 🔸 动态规划<br/>🔸 记录当前状态和上一个状态：用 `cur=1-cur` 循环<br/>🔸 快速幂<br/>🔸 矩阵乘法<br/>🔸 注意审题！！！题中的骰子和一般骰子不同！！！ |
| **10** | 5    | ❌    | 31   |      |                                                              |

## 1 【填空】方程整数解

### 题目

方程:  $a^2+b^2+c^2=1000$

这个方程有正整数解吗？有：a, b, c=6, 8, 30 就是一组解。 你能算出另一组合适的解吗？

请填写该解中最小的数字。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	for(int i=1;i*i<=1000;i++){
		for(int j=i;i*i+j*j<=1000;j++){
			for(int k=j;i*i+j*j+k*k<=1000;k++){
				if(i*i+j*j+k*k==1000){
					cout<<i<<" "<<j<<" "<<k<<endl;
				}
			}
		}
	} 
	
	return 0;
}
```

## 2 【填空】星系炸弹

### 题目

在 X 星系的广袤空间中漂浮着许多X星人造“炸弹”，用来作为宇宙中的路标。 每个炸弹都可以设定多少天之后爆炸。

比如：阿尔法炸弹 2015 年 1 月 1 日放置，定时为15天，则它在2015年1月16 日爆炸。

有一个贝塔炸弹，2014 年 11 月 9 日放置，定时为 1000 天，请你计算它爆炸的准确日期。

请输出该日期，格式为 yyyy-mm-dd 。比如：2015−02−19。

### 解析

🟠 在Excel中直接用日期格式拉出答案。

## 3 【填空】奇妙的数字

### 题目

小明发现了一个奇妙的数字。它的平方和立方正好把 0 ~ 9 的 10 个数字每个用且只用了一次。

你能猜出这个数字是多少吗？

### 解析

🟠 直接用 next_permutation(a,a+10) 来枚举所有可能

🟡 根据所给条件，推测平方数为 4 位，立方数为 6 位。

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int a[10]={0,1,2,3,4,5,6,7,8,9};
	
	do{
		int t=pow(a[0]*1000+a[1]*100+a[2]*10+a[3],0.5);
		if(t*t*t==a[4]*100000+a[5]*10000+a[6]*1000+a[7]*100+a[8]*10+a[9]){
			cout<<t;break;
		}
	}while(next_permutation(a,a+10));
	
	return 0;
}
```

## 4 【填空】格子中输出

### 题目

StringInGrid函数会在一个指定大小的格子中打印指定的字符串。要求字符串在水平、垂直两个方向上都居中。

如果字符串太长，就截断。如果不能恰好居中，可以稍稍偏左或者偏上一点。

下面的程序实现这个逻辑，请填写划线部分缺少的代码。

### 解析

注意 `printf` 、 `scanf` 中 `%*s` 的用法。

> 🟠 scanf("%d%*s",&a,b); 
>
> 添加了 * 的部分会被忽略,不会被参数获取
>
> 🟡 printf("%*s", 10, s);
>
> 输出字符串s，但至少占10个位置，不足的在字符串s左边补空格，这里等同于printf("%10s", s);

```c++
#include <stdio.h>
#include <string.h>

void StringInGrid(int width, int height, const char* s)
{
    int i,k;
    char buf[1000];
    strcpy(buf, s);
    if(strlen(s)>width-2) buf[width-2]=0;
    
    printf("+");
    for(i=0;i<width-2;i++) printf("-");
    printf("+\n");
    
    for(k=1; k<(height-1)/2;k++){
        printf("|");
        for(i=0;i<width-2;i++) printf(".");
        printf("|\n");
    }
    
    printf("|");
    printf("%*s%s%*s",(width-2-strlen(s))/2,"",buf,(width-1-strlen(s))/2,"");
              
    printf("|\n");
    
    for(k=(height-1)/2+1; k<height-1; k++){
        printf("|");
        for(i=0;i<width-2;i++) printf(".");
        printf("|\n");
    }    
    
    printf("+");
    for(i=0;i<width-2;i++) printf("-");
    printf("+\n");    
}

int main()
{
    StringInGrid(10,4,"abcd123");
    return 0;
}
```

## 5 【填空】九数组分数

### 题目

1,2,3...9 这九个数字组成一个分数，其值恰好为1/3，如何组法？

下面的程序实现了该功能，请填写划线部分缺失的代码。

### 解析

```c++
#include <stdio.h>
void test(int x[])
{
    int a = x[0]*1000 + x[1]*100 + x[2]*10 + x[3];
    int b = x[4]*10000 + x[5]*1000 + x[6]*100 + x[7]*10 + x[8];
    
    if(a*3==b) printf("%d/%d\n", a, b);
}
void f(int x[], int k)
{
    int i,t;
    if(k>=9){
        test(x);
        return;
    }
    
    for(i=k; i<9; i++){
        {t=x[k]; x[k]=x[i]; x[i]=t;}
        f(x,k+1);
        {t=x[k]; x[k]=x[i]; x[i]=t;}
    }
}
int main()
{
    int x[] = {1,2,3,4,5,6,7,8,9};
    f(x,0);    
    return 0;
}
```

## 6 【填空】牌型种数

### 题目

小明被劫持到X赌城，被迫与其他3人玩牌。
一副扑克牌（去掉大小王牌，共52张），均匀发给4个人，每个人13张。
这时，小明脑子里突然冒出一个问题：
如果不考虑花色，只考虑点数，也不考虑自己得到的牌的先后顺序，自己手里能拿到的初始牌型组合一共有多少种呢？


请填写该整数，不要填写任何多余的内容或说明文字。

### 解析

🟠 【dfs】挨个遍历每个位置，看第 x 种牌选取几张

```c++
#include <bits/stdc++.h>
using namespace std;

int sum=0,cnt=0;
void dfs(int x){
	if(sum==13){
		cnt++;return;
	}
	if(x>=14)	return;
	
	for(int i=0;i<=4;i++){			// 每种牌都有这5种可能 
		if(sum+i<=13){
			sum+=i;						// 第 x 种牌选择 i 张 
			dfs(x+1);
			sum-=i;
		} 
	}
}

int main()
{
    dfs(1);
    cout<<cnt;
    
    return 0;
}
```

## 7 【填空】手链样式

### 题目

小明有3颗红珊瑚，4颗白珊瑚，5颗黄玛瑙。

他想用它们串成一圈作为手链，送给女朋友。

现在小明想知道：如果考虑手链可以随意转动或翻转，一共可以有多少不同的组合样式呢？

### 解析

🟠 用 `next_permutation(s.begin(),s.end())` 全排列，再特殊去重

🟡 去重=去重翻转+去重旋转。去重翻转只需用 `reverse(t.begin(),t.end())` ，而去重旋转的关键是判断 s' 是否能由 s 旋转得到，即 `s' 是否是 s+s 的子串` 。

🟢 在字符串中寻找某子串： `v[i].find(s)!=string::npos`

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
	int cnt=0;
	string s="aaabbbbccccc";
	vector<string> v;
	
	do{
		// 遍历 v ，先看当前字符串能否通过已有字符串翻转、旋转获得
		int flag=0;
		for(int i=0;i<v.size();i++){
			if(v[i].find(s)!=string::npos){
				flag=1;break;
			}
		} 
		if(flag)	continue;			// 该情况已存在，继续排列
		
		// 该排列未出现过，把它的两倍及其翻转都加入 v 中
		string t=s+s;
		v.push_back(t);
		reverse(t.begin(),t.end());
		v.push_back(t);
		cnt++;
	}while(next_permutation(s.begin(),s.end()));
	
	cout<<cnt;
	return 0;
}
```

## 8 饮料换购

### 题目

乐羊羊饮料厂正在举办一次促销优惠活动。乐羊羊 C 型饮料，凭 3 个瓶盖可以再换一瓶 C 型饮料，并且可以一直循环下去(但不允许暂借或赊账)。

请你计算一下，如果小明不浪费瓶盖，尽量地参加活动，那么，对于他初始买入的 n 瓶饮料，最后他一共能喝到多少瓶饮料。

**输入描述**

输入一个整数 (0<n<1000)，表示开始购买的饮料数量。

**输出描述**

输出一个整数，表示实际得到的饮料数。

### 解析

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int n,s=0,t=0,x,y;
    cin>>n;
    s+=n;
    while(n>=3){
        s+=n/3;
        t=n%3;
        n=n/3+t;
    } 
    
    cout<<s; 
    return 0;
}
```

## 9 垒骰子

### 题目

赌圣 atm 晚年迷恋上了垒骰子，就是把骰子一个垒在另一个上边，不能歪歪扭扭，要垒成方柱体。

经过长期观察，atm 发现了稳定骰子的奥秘：有些数字的面贴着会互相排斥！

我们先来规范一下骰子：1 的对面是 4，2 的对面是 5，3 的对面是 6。

假设有 m 组互斥现象，每组中的那两个数字的面紧贴在一起，骰子就不能稳定的垒起来。

atm 想计算一下有多少种不同的可能的垒骰子方式。

两种垒骰子方式相同，当且仅当这两种方式中对应高度的骰子的对应数字的朝向都相同。

由于方案数可能过多，请输出模 $10^9+7$ 的结果。

**输入描述**

输入第一行两个整数 n, m，n 表示骰子数目；

接下来 m 行，每行两个整数 a, b ，表示 a 和 b 数字不能紧贴在一起。

其中， $0<n≤10^9, m≤36$ 。

**输出描述**

输出一行一个数，表示答案模 $10^9+7$ 的结果。

### 解析

🟠 **【动态规划】** 以每一层最上面的点数为标准遍历

🟡 **需要记录当前状态和上一个状态时，可以用 `cur=1-cur` 循环**

🟢 **【快速幂】**

🔵 **注意审题！！！题中的骰子和一般骰子不同！！！**

🟣 这样做还是超时了，有空可以用 **【矩阵乘法】** 再试试

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

// 快速幂，计算 a^n%p 
int fast(ll a,ll n,ll p){
    ll s=1;
    while(n){
        if(n&1){
            s=s*a%p;
        }
        a=(a*a)%p;n>>=1;
    }
    return s;
}

int main()
{
    int n,m,x,y,cur=0;
    ll sum=0,p=1000000007;
    int a[7][7]={0};     
	int op[7]={0,4,5,6,1,2,3};			// 注意审题！！！这个骰子和普通的不一样！！！       
    ll dp[2][7]={{0,1,1,1,1,1,1}};      // 前一层各个面朝上的可能情况，和当前层各个面朝上的可能情况 
    cin>>n>>m;
    for(int i=0;i<m;i++){            	// 输入互斥的情况 
        scanf("%d %d",&x,&y);
        a[x][y]=a[y][x]=1;
    }
    
    for(int i=2;i<=n;i++){            	// 遍历每一层                
		cur=1-cur;						// 轮流循环两层 
		memset(dp[cur],0,sizeof(dp[cur]));
            
        for(int j=1;j<=6;j++){        	// 当前层上面的点数 
            for(int k=1;k<=6;k++){    	// 前一层上面的可能点数 
                if(a[op[j]][k]==0)    dp[cur][j]=(dp[cur][j]+dp[1-cur][k])%p;
            }
        }
    }
    
    for(int i=1;i<=6;i++){				// 总的可能情况（不考虑侧面旋转）
    	sum=(sum+dp[cur][i])%p;
	}     
    
    sum=sum*fast(4,n,p)%p;  
    cout<<sum;
    
    return 0;
}
```

## 10 灾后重建

