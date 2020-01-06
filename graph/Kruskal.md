# Kruskal

pq优化+结构体存边+并查集

时间复杂度O(E*log V)

```c++
#include<bits/stdc++.h>
using namespace std;
#define INF 100000001
#define maxn 105
#define maxe 200005
/*存点*/
int f[maxn];
/*存边*/
struct edge {
	int from, to, val;
	bool operator <(const edge& a)const {
		return a.val < val;
	}
};
/*其他*/
priority_queue<edge> qu;
int tot=0;
int result;
/*并查集操作*/
int find(int x) {
	if (f[x] == x)return x;
	return f[x] = find(f[x]);
}
inline void eunion(int x, int y) {
	f[find(x)] = find(y);
}
int n,temp;
int u, v, w;
int main() {
	scanf("%d", &n);
	/*初始化老大*/
	for (int i = 0; i < maxn; i++)f[i] = i;
	/*存边*/
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			scanf("%d", &temp);
			if(i!=j)qu.push(edge{ i,j,temp });
		}
	}
	/*存边完毕*/
	while (!qu.empty()) {
		u = qu.top().from;
		v = qu.top().to;
		w = qu.top().val;
		qu.pop();
		if (find(u) == find(v))continue;
		eunion(u, v);
		result += w;
		tot++;
		if (tot == n - 1)break;
	}
	printf("%d", result);
}
```

