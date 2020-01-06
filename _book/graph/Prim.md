# Prim

pq优化+链式前向星存图  

时间复杂度O(E*log V)

```c++
#include<bits/stdc++.h>
using namespace std;
#define node_num 2505
#define edge_num 14000
#define INF 10000000
class node {
public:
	int id, dis;
	bool operator < (const node& a)const {
		return a.dis < dis;
	}
};
int result;
priority_queue<node> qu;
int to[edge_num];
int val[edge_num];
int pre[edge_num];
set<int> edges;

int tot;
int used[node_num];
int last[node_num];

int n, m, s, t;
int a, b, c;

void add(int a, int b, int c) {
	tot++;
	to[tot] = b;
	val[tot] = c;
	pre[tot] = last[a];
	last[a] = tot;
}

int main() {
	scanf("%d%d", &n, &m);
	for (int i = 0; i < m; i++) {
		scanf("%d%d%d", &a, &b, &c);
		add(a, b, c);
		add(b, a, c);
	}
	qu.push(node{ 1,0 });
	while (!qu.empty()) {
		int x = qu.top().id;
		int vv = qu.top().dis;
		int ptr = last[x];
		qu.pop();
		if (used[x])continue;
		used[x] = 1;
		result += vv;
		edges.insert(x);
		while (ptr) {
			int y = to[ptr];
			qu.push(node{ y,val[ptr] });
			ptr = pre[ptr];
		}
	}
	printf("%d", result);
}
```



