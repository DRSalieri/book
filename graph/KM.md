# KM算法

```c++
#include<bits/stdc++.h>
using namespace std;

#define MAXN 305
#define INF 0x3f3f3f3f

int line[MAXN][MAXN];   // 存图
int ex_left[MAXN];  // 每个左点的期望值
int ex_right[MAXN]; // 每个右点的期望值
bool used_left[MAXN];   // 记录每一轮匹配匹配过的左点
bool used_right[MAXN];  // 记录每一轮匹配匹配过的右点
int match[MAXN];// 记录每个右点匹配到的左点 如果没有则为-1
int slack[MAXN];// 记录每个右点能够被匹配到的最小期望值

int N;
//搜寻左点，此处与匈牙利算法大差不差
bool find(int x)
{
	//左点标记上
	used_left[x] = true;
	//遍历每一个右点
	for (int i = 0; i < N; i++) {
		if (used_right[i]) continue; // 每一轮匹配 每个右点只尝试一次
		int gap = ex_left[x] + ex_right[i] - line[x][i];  //gap代表
		if (gap == 0) {  // 如果符合要求
			used_right[i] = true;
			if (match[i] == -1 || find(match[i])) {// 没有匹配 或者 可以换人
				match[i] = x;
				return true;
			}
		}
		else {
			slack[i] = min(slack[i], gap);  // 更新slack
		}
	}
	return false;
}
//KM算法
int KM()
{
	//初始化 match 与 左右期望值
	memset(match, -1, sizeof (match));
	memset(ex_right, 0, sizeof (ex_right));  
	for (int i = 0; i < N; ++i) {
		ex_left[i] = line[i][0];
		for (int j = 1; j < N; ++j) {
			ex_left[i] = max(ex_left[i], line[i][j]);
		}
	}
	// 遍历每一个左点
	for (int i = 0; i < N; ++i) {
		fill(slack, slack + N, INF);// 初始化slack
		while (1) {
			// 初始化 标记数组
			memset(used_left, false, sizeof (used_left));
			memset(used_right, false, sizeof (used_right));
			//成功配对 退出
			if (find(i)) break;
			// 不能成功配对
			//d代表左边将要减去的期望
			int d = INF;
			//更新d为 参与选拔的右点 中 slack的最小值
			for (int j = 0; j < N; ++j)
				if (!used_right[j]) d = min(d, slack[j]);
			for (int j = 0; j < N; ++j) {
				//左点降低期望
				if (used_left[j]) ex_left[j] -= d;
				// 右点升高期望
				if (used_right[j]) ex_right[j] += d;
				// 未参与选拔的右点，他们的能被匹配到的可能性更大了
				else slack[j] -= d;
			}
		}
	}
	// 匹配完成 求出所有组合的权值和
	int res = 0;
	for (int i = 0; i < N; ++i)
		res += line[match[i]][i];
	return res;
}
//主循环
int main()
{
	while (scanf("%d", &N)) {
		for (int i = 0; i < N; ++i)
			for (int j = 0; j < N; ++j)
				scanf("%d", &line[i][j]);
		printf("%d\n", KM());
	}
	return 0;
}
```

