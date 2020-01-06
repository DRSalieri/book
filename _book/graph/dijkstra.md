# 堆优化Dijkstra

链式前向星存图+pq优化

```c++
#include<bits/stdc++.h>
using namespace std;
#define node_num 2505
#define edge_num 12405
#define INF 100000000
class node {
public:
	//这里的dis并不是主要的dis，只是用来比较，完善pq的
	int id, dis;
	bool operator <(const node& a)const {
		return a.dis < dis;
	}
};
int pre[edge_num];
int to[edge_num];
int val[edge_num];
int tot;   //从1开始
int last[node_num];
int ndis[node_num];
int used[node_num];
void add(int u, int v , int w) {
	tot++;
	to[tot] = v;
	val[tot] = w;
	pre[tot] = last[u];
	last[u] = tot;
}
int n, m, s, t;
int a, b, c;
int main() {
	priority_queue<node> qu;
	scanf("%d%d%d%d", &n, &m, &s, &t);
	fill(ndis, ndis + n + 1, INF);
	ndis[s] = 0;
	ndis[0] = 0;
	/*数据输入*/
	for (int i = 0; i < m; i++) {
		scanf("%d%d%d", &a, &b, &c);
		add(a, b, c);
		add(b, a, c);
	}
	qu.push(node{ s,ndis[s] });
	/*主循环*/
	while (!qu.empty()) {
		/*弹出元素*/
		int x = qu.top().id;
		int ptr = last[x];
		qu.pop();
		/*判断终止*/
		if (x == t)break;
		if (used[x])continue;
		used[x] = 1;
		/*遍历松弛*/
		while (ptr) {
			int y = to[ptr];
			if (!used[y]&&ndis[y] > ndis[x] + val[ptr]) {
				ndis[y] = ndis[x] + val[ptr];
				qu.push(node{ y,ndis[y] });
			}
			ptr = pre[ptr];
		}
	}
	printf("%d", ndis[t]);
}
```

