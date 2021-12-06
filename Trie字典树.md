# Trie字典树

## 字典树

### 构成

字典树，就是像字典一样的树。平时我们查字典的时候，先查第一个字母，再查第二个字母，再查第三个字母，依次类推，在  $O(|S|)$ 的时间内查出了一个单词。

同样地，我们可以仿照这个思想构造一棵字典树，从根开始的路径代表了一个前缀；当到达一个有 ```end``` 标记的时候，就代表查到了一个串。字典树的基本思想是比较简单的。同样，字典树可以将单串的字符串算法加强为多串。

### 空间复杂度

字典树的空间复杂度最大是 $O(\sum |S|)$ 的。因此，要计算好时空复杂度，做到时空平衡。

[**P3879 [TJOI2010]阅读理解**](https://www.luogu.com.cn/problem/P3879)  [code](https://www.luogu.com.cn/record/list?pid=P3879&user=235868)

这一道题就是卡空间的~~好题~~。如果从母串开始构建Trie树，就会光荣的得到 MLE 0分的好成绩。

### 代码

```cpp
struct trie_node{
	int son[26]; vector<int> end;
}trie[MAXM * 20];
int pool = 1, rt = 1;
void insert(char *s, int id){
	int n = strlen(s + 1), p = rt;
	for(int i = 1; i <= n; ++i){
		if(trie[p].son[s[i] - 'a'] == 0) trie[p].son[s[i] - 'a'] = ++pool;
		p = trie[p].son[s[i] - 'a'];
	}
	trie[p].end.push_back(id);
}
```



## 字典树性质和应用

### 字典序

看到字典序，应该直接想字典树。将字符串从首位到低位依次插入到字典树中，就可以较为容易地处理大小关系。

[**P3065 [USACO12DEC]First! G**](https://www.luogu.com.cn/problem/P3065)  [code](https://www.luogu.com.cn/record/list?pid=P3065&user=235868)

这一题就是先将每一个字符串插入到字典树当中，然后枚举一个字符串 $ S$，该串 $S$ 能成为某个字典下最大的字符串，当且仅当从根一直走下去，每一位上都能比其他儿子大。于是，得到了一本字典上的字符大小关系，建图判断是否有环就醒了。

### 树

字典树本身是一棵树，于是很多树上问题的解法可以应用到字典树上。

[**P4683 [IOI2008] Type Printer**](https://www.luogu.com.cn/problem/P4683)  [code](https://www.luogu.com.cn/record/list?pid=P4683&user=235868)

这一题先插入到字典树上，然后问题转换为从根走完所有叶子节点的最短距离。答案就是 dfs 时经过的距离减去最深节点的深度，在 dfs 的过程中先走最深的儿子，压栈弹栈的时候记录下操作序列，最后再反过来输出就可以了。

## 非字符串的字典树

字典树其实不一定只能用来处理字符串问题，但我只知道一种变形。

### 0/1 trie树

01 trie树是解决亦或问题的其中一把利器 （另一把是线性基）。将一个数的二进制从高位到低位插入到 trie 树上，就构成了 01 trie树。

[**P4551 最长异或路径**](https://www.luogu.com.cn/problem/P4551)  [code](https://www.luogu.com.cn/record/list?pid=P4683&user=235868)

首先，由于一个 $x \operatorname{xor} x = 0 $ ，因此直接将树的条件拆开，每一个节点的权值变成它从根一直亦或下来的亦或和，然后原问题就转换成了在一个数集内，找两个数亦或和最大。

考虑直接枚举其中一个数，然后在01 trie树上每次走相反的方向。最后到达的节点就是亦或最大的数。

