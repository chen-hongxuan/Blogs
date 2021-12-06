# AC自动机

## Trie树 + KMP = AC自动机 (是个蛋)

KMP 能处理的是单个 $T$ 串匹配多个 $S$ 串，但是不能处理多个 $T$ 串匹配一个 $S$ 串；而 Trie 树是将字符串算法从单串拓展到多串的有力武器。$S$ 完美匹配一个串 $T$ 的过程，其实就是从 Trie 树的根一直向下走，到达的节点表示的字符串一定是 $S$ 的前缀。因此，就有一种暴力的做法，枚举 $T$ 开头在 $S$ 中的位置，依次插入到 Trie 树上匹配。

但是这样的做法是 $O(|S||\sum T|)$ 的，比暴力做 KMP 的过程还慢。于是，可以像 KMP 那样，在 Trie 树上增加一些边，使得每此匹配失败的时候不需要重头匹配。这个额外的边就是 fail 指针。和 KMP 一样，若 $u$ 的 fail 指向了 $v$ ，那么从根到 $v$  的字符串 $S_v$ 是从根到 $u$ 的字符串 $S_u$ 的前缀。

AC自动上的 fail 边看似比 KMP 麻烦，但其实构造远离比 KMP 是一样的，甚至更为直观。首先，假设 $u$ 转移字符 $c$ 到节点 $v$ ，那么 $fail_v$ 也是由 $fail_u$ 转移字符 $c$ 得到的。这个结论是显然正确的。

于是，构造 $fail$ 指针的代码如下：

```cpp
void getfail(){
	static int q[LENT], qh = 1, qt = 1;
	for(int i = 0; i < MAXC; ++i) actrie[0].son[i] = 1;
	q[++qt] = 1;
	while(qh != qt){
		if(++qh == LENT) qh = 0;
		int u = q[qh], ufai = actrie[u].fail;
		for(int i = 0; i < MAXC; ++i){
			int v = actrie[u].son[i];
			if(!v) actrie[u].son[i] = actrie[ufai].son[i];
			else {
				actrie[v].fail = actrie[ufai].son[i];
				addside(v, actrie[v].fail);
				if(++qt == MAXN) qt = 0;
				q[qt] = v;
			}
		}
	}
}
```

## 时间复杂度

时间复杂度正确性取决于匹配过程。匹配 $S$ 的过程就是在自动机上从根开始向下走的过程，走到一个节点的就标记着一个节点已经被找过。这时候，这个节点的 $fail$ 也要被标记，它的 $fail$ 的 $fail$ 也要被标记， …..。这时候，如果暴力统计，时间复杂度在随机数据上是对的，但是当所有每个节点的 $fail$ 组成了一条长链，那么时间复杂度就是 $O(|S||\sum T|)$ 的。所以，就需要根据 $fail$ 边建图 (DAG) ，最后跑一次拓扑统计。这样做的时间复杂度是 $O(|S| + | \sum T |)$ 。

[**【模板】AC自动机（二次加强版）**](https://www.luogu.com.cn/problem/P5357)  [100](https://www.luogu.com.cn/record/list?pid=P5357&user=235868) 

