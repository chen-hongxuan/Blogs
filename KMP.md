# KMP

我怎么连KMP都不会了                                                                                                                                                                                

*注: 文中 ```nxt``` 即 $p$

## KMP算法

KMP ( Knuth-Morris-Pratt ) 实现了在一个母串 $S$ 内匹配出子串 $T$ ，时间复杂度是 $O(|S| + |T|)$ 。

KMP算法是从暴力匹配优化而来的。原先暴力匹配的时候，先枚举 $T$ 在 $S$ 上的起始点，然后再依次匹配，检查是否为可行位置。但是这样匹配的时间复杂度最坏是 $O(|S||T|)$ 的，因为检查一个位置可能检查了一整个 $T$ 串。

优化的关键点在于，检查了以 $i$ 位置开头的匹配后，不需要重头开始匹配，因为我们已经知道 $S[i...i+j-1] = T[1...j]$ 。假设有一个数组 $p_i$

表示 $T[1...p_i] = T[i-p_i+1...i]$，那么还有 $S[i...j] = T[1...p_i]$ ，因此直接令 $j = p_i$ ，然后继续匹配下去。同理，可以证明中间不会再存在解。

预处理 $p$ 的部分可以看做 $T$ 自己与自己的匹配 $T$ 。

**预处理**

```cpp
for(int i = 2, j = 0; i <= len2; ++i){
	while(j && s2[i] != s2[j + 1]) j = nxt[j];
	if(s2[i] == s2[j + 1]) ++j; nxt[i] = j;
}
```

**匹配**

```cpp
for(int i = 1, j = 0; i <= len1; ++i){
	while(j && s1[i] != s2[j + 1]) j = nxt[j];
	if(s1[i] == s2[j + 1]) ++j;
	if(j == len2){
		printf("%d\n", i - j + 1);
		j = nxt[j];
	}
}
```

[**【模板】KMP字符串匹配**](https://www.luogu.com.cn/problem/P3375)   [code](https://www.luogu.com.cn/record/43153295)



## ```nxt```数组意义

KMP的考点远不止于此。```nxt```数组除了做 KMP 匹配外，还能做很多奇奇怪怪的东西。

###  找循环节

由于 ```nxt[i]``` 表示 $T[1...p_i]$ 与 $T[i-p_i+1...i]$ 完全相同，于是有 $T[1...i-p_i+1]=T[i-p_i+1...2(i-p_1+1)]$，有 $T[k(i-p_i) + 1...(k+1)(i-p_i) + 1]$ 均相等。于是，只要 $ p_n | n$ ，就有 $n-p_n$ 是最小循环节。

[**P4391 [BOI2009]Radio Transmission 无线传输**](https://www.luogu.com.cn/problem/P4391)  [code](https://www.luogu.com.cn/record/list?pid=P4391&user=235868)

这一题要求找某个最下循环串，并且输入的串可能是实际串的子串，因此直接输出 $n-p_n$ 即可，原理同上。

### 特殊限制

当 “前缀等于后缀” 的字符串要求长度小于某个数的时候，可以给 KMP 增加限制条件，不满足就一直跳。

[**P2375 [NOI2014] 动物园**](https://www.luogu.com.cn/problem/P2375)   [code](https://www.luogu.com.cn/record/list?pid=P2375&user=235868)

这一题要先预处理出 nxt 数组，然后统计每一个位置上跳多少次到达 0 记为```cnt``` 。 最后做一次变种 KMP, ```num```直接等于 ```cnt``` 。

##  变种 KMP

###  匹配成功后的 ```j```位置

一般来说，允许重叠匹配的KMP在匹配成功后 ```j=nxt[j]```，来进行下一次匹配；不能重叠就 ```j=0``` 。但其实 ```j``` 的位置还可以有其他选择。

[ **[USACO15FEB]Censoring S** ](https://www.luogu.com.cn/problem/P4824)  [code](https://www.luogu.com.cn/record/list?pid=P4824&user=235868)

这一题还是KMP，匹配完以后的 ```j``` 用一个栈来维护。最后栈内元素就是最终的字符串。注意栈内维护的是下标，不是字符。