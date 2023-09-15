# 【CSP】202212题解


## 1 现值计算

🔗 **题目：[现值计算](http://118.190.20.162/view.page?gpid=T160)**

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){

	double i,ans,s;
	int n,t;
	cin>>n>>i>>ans;
	i+=1;s=i;
	for(int j=1;j<=n;j++){
		scanf("%d",&t);
		ans += t*1.0/s;
		s*=i;
	}
	cout<<ans;
	return 0;
}
```

## 2 训练计划

🔗 **题目：[训练计划](http://118.190.20.162/view.page?gpid=T159)**

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	// 输入 
	int n,m,mt=0;
	int p[105]={0},t[105]={0},b[105]={0},e[105]={0},f[105]={0};
	scanf("%d %d",&n,&m);
	for(int i=1;i<=m;i++){
		scanf("%d",&p[i]);
		f[p[i]]=1;
	}
	
	// 计算最早开始时间 
	for(int i=1;i<=m;i++){
		scanf("%d",&t[i]);
		b[i] = e[p[i]]+1;
		e[i] = b[i]+t[i]-1;
		if(e[i]>mt)	mt=e[i];
	}
	for(int i=1;i<=m;i++)
		printf("%d ",b[i]);
		
	// 如果不能在规定时间完成，退出
	if(mt>n)	return 0;
	// 否则，继续计算并输出最晚开始时间 
	fill(e,e+m+1,400);
	for(int i=m;i>=1;i--){
		if(f[i]!=1)
			b[i] = n-t[i]+1;
		else b[i] = e[i]-t[i]+1;	
		e[p[i]] = min(e[p[i]],b[i]-1);
	}
	printf("\n");
	for(int i=1;i<=m;i++)
		printf("%d ",b[i]);
	
	return 0;
} 
```

## 3 JPEG解码

🔗 **题目：[JPEG解码](http://118.190.20.162/view.page?gpid=T158)**

🟠 这种题目难度不是很大，但题目很长，需要把要求一步步分解，耐心计算即可。

🟡 蛇形矩阵填充，由于本题矩阵大小固定，可以 **用矩阵 idx\[8][8] 来存储对应位置填充的元素下标** ，就可以很方便的完成存储。

🔵 在循环里面 i++ 换成 ++i 更快一点。

```c++
#include<bits/stdc++.h>
using namespace std;
#define PI acos(-1)

int Q[8][8],M[8][8]={0},S[64]={0},n,t;
double a[8]={sqrt(0.5),1,1,1,1,1,1,1},N[8][8]={0}; 
int idx[8][8]={{0,1,5,6,14,15,27,28},
				{2,4,7,13,16,26,29,42},
				{3,8,12,17,25,30,41,43},
				{9,11,18,24,31,40,44,53},
				{10,19,23,32,39,45,52,54},
				{20,22,33,38,46,51,55,60},
				{21,34,37,47,50,56,59,61},
				{35,36,48,49,57,58,62,63}};

int main(){
	for(int i=0;i<8;i++)			// 读入量化矩阵 
		for(int j=0;j<8;j++)
			cin>>Q[i][j];
	cin>>n>>t;
	for(int i=0;i<n;i++)
		cin>>S[i];
	for(int i=0;i<8;i++)			// 读入扫描数据，乘以量化矩阵Q 
		for(int j=0;j<8;j++)
			M[i][j]=S[idx[i][j]],Q[i][j]*=M[i][j];	
	if(t==0){						// t=0 输出扫描后的图像矩阵 
		for(int i=0;i<8;i++){
			for(int j=0;j<8;j++)
				cout<<M[i][j]<<" ";
			cout<<endl;
		}
		return 0;
	}
	
	if(t==1){						// t=1 输出量化后的图像矩阵 
		for(int i=0;i<8;i++){
			for(int j=0;j<8;j++)
				cout<<Q[i][j]<<" ";
			cout<<endl;
		}
		return 0;
	}	
	
	for(int i=0;i<8;i++){
		for(int j=0;j<8;j++){
			for(int u=0;u<8;u++){
				for(int v=0;v<8;v++){
					N[i][j] += a[u]*a[v]*Q[u][v]*cos((i+0.5)*u*PI/8)*cos((j+0.5)*v*PI/8);
				}
			}
			N[i][j] = (int)(N[i][j]/4+0.5+128);
			if(N[i][j]>255)	N[i][j]=255;
			else if(N[i][j]<0)	N[i][j]=0;
			cout<<N[i][j]<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```


