# 匈牙利算法

```c++
#include<bits/stdc++.h>
using namespace std;
#define INF 100000000
#define maxn 1005
int n, m, e;
int mp[maxn][maxn];
int u, v;
int used[maxn];
int girl[maxn];
int kase;
//对左侧某个元素配对
bool find(int x) {
	for (int i = 1; i <= m; i++) {
		if (mp[x][i] && !used[i]) {
			used[i] = 1;
			if (girl[i] == 0 || find(girl[i])) {
				girl[i] = x;
				return true;
			}
		}
	}
	return false;
}
int main() {
	scanf("%d%d%d", &n, &m, &e);
	for (int i = 0; i < e; i++) {
		scanf("%d%d", &u, &v);
		mp[u][v] = 1;
	}
	for (int i = 1; i <= m; i++) {
		memset(used, 0, sizeof(used));
		if (find(i))kase++;
	}
	printf("%d", kase);
}
```

