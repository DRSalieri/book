# Dinic

手写队列+链式前向星存图+炸点、多路增广优化(有时间写一下当前弧优化)

```c++
//dinic算法 手写队列 链式前向星存图 炸点，多路增广优化（无当前弧优化）
#include<bits/stdc++.h>
using namespace std;
#define maxn 105
#define maxe 10005
#define INF 1e9
int aim;
//手写队列
int dl[100000];
//链式前向星存图
//边
int to[maxe];
int pre[maxe];
int flow[maxe*2];//算进去反边
int tot=1;  //这里要注意，边从2开始存，因为我们使用了异或的性质来保持边与反边相邻，边为偶数，反边为奇数
//点
int last[maxn];
int deep[maxn];

//bfs标记deep
bool bfs(int s, int t) {
	//深度初始化
	memset(deep, 0, sizeof(deep));
	deep[s] = 1;
	//队列初始化
	dl[1] = s;
	int head = 0, tail = 1;
	while (head != tail) {
		//读入队列首元素
		head++;
		int v = dl[head];
		//遍历其所有边
		for (int ptr = last[v]; ptr; ptr = pre[ptr]) {
			//若 1.容量不为0 2.深度未定义(为0)
			if (flow[ptr] && !deep[to[ptr]]) {
				//深度录入
				deep[to[ptr]] = deep[v] + 1;
				//该节点入列
				tail++;
				dl[tail] = to[ptr];
			}
		}
	}
	//用于判断是否已经流完了
	return deep[t];
}
//fl指当前该节点的容量最小值
inline int dfs(int now, int fl) {
	//流到头了
	if (now == aim)return fl;
	//f用来记录其所有子节点能够达到的最小值
	int f = 0;
	//遍历这个节点的每一个边，这里是"多路增广"优化
	for (int ptr = last[now]; ptr; ptr = pre[ptr]) {
		//若 1.容量不为0 2.深度为后继
		if (flow[ptr] && deep[to[ptr]] == deep[now] + 1) {
			//记录这一次bfs的最小值
			int x = dfs(to[ptr], min(fl, flow[ptr]));
			//更新边与反边的容量
			flow[ptr] -= x;
			flow[ptr ^ 1] += x;
			//更新所有子节点能够达到的最小值
			f += x;
			//更新fl，表示剩余的容量，因为一次bfs中一个节点可能流向两个子节点
			fl -= x;
		}
	}
	//炸点优化，若这个节点已经流完了(f=0)，我们就给他炸掉，deep设为-2，表示这个bfs再也不可能到这个节点了
	if (!f)deep[now] = -2;
	return f;
}

//外层循环
int maxflow(int s, int t) {
	aim = t;
	int ret = 0;
	while (bfs(s, t)) {
		ret += dfs(s, INF);
	}
	return ret;
}

//存边
void add(int u, int v, int w) {
	tot++;
	to[tot] = v;
	flow[tot] = w;
	pre[tot] = last[u];
	last[u] = tot;
	tot++;
	to[tot] = u;
	flow[tot] = 0;
	pre[tot] = last[v];
	last[v] = tot;
}

//主循环
int n, m;
int u, v, w;
int main() {
	scanf("%d%d", &n, &m);
	for (int i = 0; i < m; i++) {
		scanf("%d%d%d", &u, &v, &w);
		add(u, v, w);
	}
	printf("%d", maxflow(1, 5));
}
```

