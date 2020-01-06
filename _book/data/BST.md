# BST

```c++
#include<bits/stdc++.h>
using namespace std;
struct node {
	int val,dep;
	node* left, * right, * father;
	node(int x) :val(x), left(NULL), right(NULL), father(NULL) {};
};
struct tree {
	node* root;
	tree() :root(NULL) {};

	//插入操作，成功插入返回1，否则返回0
	int insert(int x) {
		int kase = 0;
		//无根节点
		if (!root) { root = new node(x); return 1; }
		//有根节点
		node* p = root;
		node* q;
		while (p) {
			q = p;
			kase++;
			if (x < p->val)p = p->left;
			else if (x > p->val)p = p->right;
			else return 0;
		}
		p = new node(x);
		p->father = q;
		if (x > q->val) q->right = p;
		else q->left = p;
		p->dep = kase;
		return 1;
	}

	//查询操作，有则返回指向这个节点的指针，没有则返回NULL
	node* find(int x) {
		node* p = root;
		while (p) {
			if (x < p->val)p = p->left;
			else if (x > p->val)p = p->right;
			else return p;
		}
		return NULL;
	}

	//打印操作,打印某一个节点一直到根节点路上的节点
	void print(node* p) {
		while (p) {
			printf("%d\n", p->val);
			p = p->father;
		}
	}

	//中序遍历，先序后序同理
	//left=1代表左子树，0代表是右子树，2代表是根节点
	void forth_traversal(int depth,int left,node *cur) {
		if (!cur)return;
		forth_traversal(depth + 1, 1, cur->left);
		for (int i = 0; i < depth; i++)printf("  ");
		if (left == 1)printf("/");
		else if (left == 0)printf("\\");
		else printf(":");
		printf("%d\n", cur->val);
		forth_traversal(depth + 1, 0, cur->right);
	}
};
```

