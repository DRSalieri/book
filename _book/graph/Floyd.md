# Floyd

```c++
for (int k = 0; k <= n; k++) {
    for (int i = 0; i <= n; i++) {
	    for (int j = 0; j <= n; j++) {
		    if (e[i][j] > e[i][k] + e[k][j])
			    e[i][j] = e[i][k] + e[k][j];
	    }
    }
}
```

