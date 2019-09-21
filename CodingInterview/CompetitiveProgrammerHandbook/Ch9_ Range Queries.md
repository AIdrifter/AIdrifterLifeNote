# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) <br>  Competitive Programming's Handbook <br> Ch9: Range Queries 
# Range Queries
這邊我們會介紹有效的Data Structure去做range query，常見有以下三種:

- $\texttt{sum}_q(a,b)$: calculate the sum of values in range $[a,b]$
- $\texttt{min}_q(a,b)$: find the minimum value in range $[a,b]$
- $\texttt{max}_q(a,b)$: find the maximum value in range $[a,b]$

在n個元素中，做q次query，如果用列舉法，總共的時間複雜度是: $O(nq)$
```cpp
int sum(int a, int b) {
    int s = 0;
    for (int i = a; i <= b; i++) {
        s += array[i];
    }
    return s;
}
```
下面介紹還有哪些有效的方法:
## Static array queries
array內元素不會被update。
We first focus on a situation where
the array is ${static}$, i.e.,
the array values are never updated between the queries.
In this case, it suffices to construct
a static data structure that tells us
the answer for any possible query.
### one-dimensional Sum Queries 
利用Prefix Sum Array(前缀和)快速算出結果。

這是原本的array:

![](https://i.imgur.com/j6Uk5QH.png)


這是prefix sum array:

![](https://i.imgur.com/RKTSf89.png)

其公式為:
\begin{array}{lcl}
\texttt{sum}_q(a,b) = \texttt{sum}_q(0,b) - \texttt{sum}_q(0,a-1)
\end{array}
By defining $\texttt{sum}_q(0,-1)=0$,

我們先以 $range[3,6]$ 作為範例:

![](https://i.imgur.com/3CZ7TKZ.png)

In this case $\texttt{sum}_q(3,6)=8+6+1+4=19$.

![](https://i.imgur.com/94XBjHx.png)

Thus, $\texttt{sum}_q(3,6)=\texttt{sum}_q(0,6)-\texttt{sum}_q(0,2)=27-8=19$.

### two-dimensional preﬁx sum array
這邊我們可以延伸到二維的範例:

![](https://i.imgur.com/leEdVPx.png)

The sum of the gray subarray can be calculated
using the formula

A:黃     B:橘   C:綠   D:藍

\begin{array}{lcl}
S(A) - S(B) - S(C) + S(D)
\end{array}


$S(X)$  為從左上角到位置$X$的框框範圍。
where $S(X)$ denotes the sum of values in a rectangular
subarray from the **upper-left corner**
to the position of $X$.

## Minimum queries $O(nlogn)$
我們採用sparse table做法。
Sparse Table 的預處理是建立一個二維陣列 $\textrm{min}_q(a,b)$，其中 $\textrm{min}_q(a,b)$ 代表編號在 $(a, b)$ 範圍內所有數的最小值。這樣一來空間複雜度 preprocessing $O(N log N)$。

The idea is to precalculate all values of
$\textrm{min}_q(a,b)$ where
$b-a+1$ (the length of the range) is a power of two.
For example, for the array
![](https://i.imgur.com/lk9SRsY.png)

這邊可以當作建table的樹高為$O(logn)$，以這題來說小table元素範圍分別為 1 ,2 ,4 ...
The number of precalculated values is $O(n \log n)$,
because there are $O(\log n)$ range lengths
that are powers of two.
The values can be calculated efficiently
using the recursive formula
\begin{array}{lcl}
\texttt{min}_q(a,b) = \min(\texttt{min}_q(a,a+w-1),\texttt{min}_q(a+w,b))
\end{array}
Let $k$ be the largest power of two that does not exceed $b-a+1$.
We can calculate the value of $\texttt{min}_q(a,b)$ using the formula
\begin{array}{lcl}
\texttt{min}_q(a,b) = \min(\texttt{min}_q(a,a+k-1),\texttt{min}_q(b-k+1,b))
\end{array}
In the above formula, the range $[a,b]$ is represented
as the union of the ranges $[a,a+k-1]$ and $[b-k+1,b]$, both of length $k$.

an example, consider the range $[1,6]$:
![](https://i.imgur.com/nke1pZ2.png)

The length of the range is 6,
and the largest power of two that does
not exceed 6 is 4.
Thus the range $[1,6]$ is
the union of the ranges $[1,4]$ and $[3,6]$:
![](https://i.imgur.com/nBg275y.png)
Since $\texttt{min}_q(1,4)=3$ and $\texttt{min}_q(3,6)=1$,
we conclude that $\texttt{min}_q(1,6)=1$.

:::info
- sparse table無法有效更新序列的數值。
- 實作過於複雜而不實用。
:::

## Binary Index Tree $(nlogn)$
又稱Fenwick Tree，可以當作動態更新value的prefix sun array，可惜的是並沒有支援minimum queries。
can be seen as a dynamic **variant** of a prefix sum array.
It supports two $O(\log n)$ time operations on an array:
processing a range sum query and updating a value.

比起前面的prefix sum array更加強大，可以用來做minmum queries和maximum queries，也可以動態update元素的值，讓我來們來看看公式定義吧:

### Sum value queries: O(logn)
Let $p(k)$ denote the largest power of two that divides $k$.
We store a binary indexed tree as an array $\texttt{tree}$
such that

\begin{array}{lcl}
\texttt{tree}[k] = \texttt{sum}_q(k-p(k)+1,k)
\end{array}

i.e., each position $k$ contains the sum of values
in a range of the original array whose length is $p(k)$
and that ends at position $k$.
For example, since $p(6)=2$, $\texttt{tree}[6]$
contains the value of $\texttt{sum}_q(5,6)$.

原Array
![](https://i.imgur.com/yT2jeST.png)

對應的binary index tree array(Fenwick Tree)
![](https://i.imgur.com/dbNlMzA.png)

其實是長這個樣子的
![](https://i.imgur.com/AYnhaaa.png)


$\texttt{sum}_q(1,k)$可以用$O(nlogn)$算出答案，我們用range[1,7] 來看:

Using a binary indexed tree,
any value of $\texttt{sum}_q(1,k)$
can be calculated in $O(\log n)$ time,
because a range $[1,k]$ can always be divided into
$O(\log n)$ ranges whose sums are stored in the tree.

For example, the range $[1,7]$ consists of
the following ranges:
![](https://i.imgur.com/SE3c7MU.png)

Thus, we can calculate the corresponding sum as follows:

\begin{array}{lcl}
\texttt{sum}_q(1,7)=\texttt{sum}_q(1,4)+\texttt{sum}_q(5,6)+\texttt{sum}_q(7,7)=16+7+4=27
\end{array}
為了計算$\texttt{sum}_q(a,b)$在 $a>1$ 的case，我們可以利用之前prefix sum的公式:
To calculate the value of $\texttt{sum}_q(a,b)$ where $a>1$,
we can use the same trick that we used with prefix sum arrays:

\begin{array}{lcl}
\texttt{sum}_q(a,b) = \texttt{sum}_q(1,b) - \texttt{sum}_q(1,a-1)
\end{array}
因為$\texttt{sum}_q(1,b)$ 和 $\texttt{sum}_q(1,a-1)$ 都是$O(logn)$，所以時間複雜度為$O(logn)$。

### Update value $O(logn)$
假設要升級位置3的話，需要花掉$O(logn)$去升級。
Then,after updating a value in the original array,several values in the binary indexed tree should be updated. For example, if the value at position 3 changes, the sums of the following ranges change:

![](https://i.imgur.com/lcpLEfJ.png)

Since each array element belongs to $O(\log n)$
ranges in the binary indexed tree,
it suffices to update $O(\log n)$ values in the tree.

### Implementation $O(logn)$
我們可以利用bit operations去有效的計算:
The key fact needed is that we can
calculate any value of $p(k)$ using the formula

\begin{array}{lcl}
p(k) = k \& -k.
\end{array}

The following function calculates the value of $\texttt{sum}_q(1,k)$:
```cpp
int sum(int k) {
    int s = 0;
    while (k >= 1) {
        s += tree[k];
        k -= k&-k;
    }
    return s;
}
```

The following function increases the
array value at position $k$ by $x$
($x$ can be positive or negative):
```cpp
void add(int k, int x) {
    while (k <= n) {
        tree[k] += x;
        k += k&-k;
    }
}
```

## Segment Tree
:::info
- 優: 可以支援全部的類型，sum(), minimum(), maximum()
- 缺: memory消耗比較多，也不太好實做。
:::
A **segment tree** is a data structure
that supports two operations:
processing a range query and
updating an array value.
Segment trees can support
sum queries, minimum and maximum queries and many other
queries so that both operations work in $O(\log n)$ time.

Compared to a binary indexed tree,
the advantage of a segment tree is that it is
a more general data structure.
**While binary indexed trees only support sum queries,
segment trees also support other queries**.
On the other hand, a segment tree requires more
memory and is a bit more difficult to implement.

### Structure
先假設array size為2的指數次方，array index自0開始。
we assume that the size
of the array is a **power of two** and **zero-based
indexing** is used, because it is convenient to build
a segment tree for such an array.
If the size of the array is not a power of two,
we can always append extra elements to it.


![](https://i.imgur.com/WsNqpw4.png)

![](https://i.imgur.com/pQIr5gN.png)
屬於 bottom-up implementation，每個internal node為其左右子點相加。

### Add $O(logn)$
我們用range[2,7]來看:

![](https://i.imgur.com/XPBbU6b.png)

![](https://i.imgur.com/3wvzlIs.png)
因為range[2,7] 是用，range[2,3] + range[4,7]組成的，兩個node都在tree高$O(logn)$內，所以時間複雜度為 $O(logn)$。
When the sum is calculated using nodes
located as high as possible in the tree,
at most two nodes on each level
of the tree are needed.
Hence, the total number of nodes
is $O(\log n)$.

### Update $O(logn)$
為了升級child node，我們要把child node上面的parent node全部都一起升級，時間複雜度為$O(logn)$
After an array update,
we should update all nodes
whose value depends on the updated value.
This can be done by traversing the path
from the updated array element to the top node
and updating the nodes along the path.

The following picture shows which tree nodes
change if the array value 7 changes:

![](https://i.imgur.com/TYiQDqX.png)

The path from bottom to top
always consists of $O(\log n)$ nodes,
so each update changes $O(\log n)$ nodes in the tree.

### Implementation
We store a segment tree as an **array of 2n** elements where n is the size of the original array and a power of two. The tree nodes are stored from top to bottom:
tree[1] is the top node, tree[2] and tree[3] are its children, and so on. Finally, the values from tree[n] to tree[2n−1] correspond to the values of the original array on the bottom level of the tree. 
![](https://i.imgur.com/xg4pkHB.png)

![](https://i.imgur.com/oYV8Qmr.png)

- The parent of $\texttt{tree}[k]$ is $\texttt{tree}[\lfloor k/2 \rfloor]$,
- The children are $\texttt{tree}[2k]$ and $\texttt{tree}[2k+1]$.

這代表左子點一定是偶數，右子點一定是竒數。
Note that this implies that the position of a node
is even if it is a left child and odd if it is a right child.

以下操作條件都是tree已經建立好的前提，所以tree的建立也很重要。

The following function calculates the value of $\texttt{sum}_q(a,b)$:
The function maintains a range that is initially [a+n,b+n]. Then, at each step, the range is moved one level higher in the tree, and before that, the values of the nodes that do not belong to the higher range are added to the sum. 
```cpp
int sum(int a, int b) {
    a += n; b += n;
    int s = 0;
    while (a <= b) {
        if (a%2 == 1) s += tree[a++];
        if (b%2 == 0) s += tree[b--];
        a /= 2; b /= 2;
    }
    return s;
}
```
The following function increases the array value at position k by x:
First the function updates the value at the bottom level of the tree. After this, the function updates the values of all internal tree nodes, until it reaches the top node of the tree. 
```cpp
void add(int k, int x) {
    k += n;
    tree[k] += x;
    for (k /= 2; k >= 1; k /= 2) {
        tree[k] = tree[2*k]+tree[2*k+1];
    }
}
```

Both the above functions work in $O(logn)$ time, because a segment tree of n elements consists of $O(logn)$ levels, and the functions move one level higher in the tree at each step.


### Other queries
要求minimum queries只要把parent node從sum()改成minimum()即可。
The operations can be implemented like previously, but instead of sums, minima are calculated. 
![](https://i.imgur.com/TAB7fCG.png)

我們也可以利用binary search 去找到最小值1，他在原本array的位置。
The structure of a segment tree also allows us to use **binary search** for locating array elements. For example, if the tree supports minimum queries, we can ﬁnd the position of an element with the smallest value in $O(logn)$ time. 
![](https://i.imgur.com/KJtkNbT.png)


## Addtional techniques
### Index Compression
假設index超過$10^9$，array是會爆掉的，因為memory實在花太多，所以利用index compression去壓縮index，要注意我們必須要保留其index order，所以 if $a<b$, then $c(a)<c(b)$.
The idea is to replace each original index $x$
with $c(x)$ where $c$ is a function that
compresses the indices.
We require that the order of the indices
does not change, so if $a<b$, then $c(a)<c(b)$.
This allows us to conveniently perform queries
even if the indices are compressed.

For example, if the original indices are
$555$, $10^9$ and $8$, the new indices are:


\begin{array}{lcl}
c(8) & = & 1 \\
c(555) & = & 2 \\
c(10^9) & = & 3 \\
\end{array}

### Range Updates
如果我們要在range[a,b]這範圍全部加x，使用difference array是一個不錯的方式:
只需要換兩個元素，就可以完成升級。

這是difference array:
For example, the value 2 at position 6 in the original array corresponds to the sum 3−2+4−3=2 in the difference array.
![](https://i.imgur.com/oZuy65u.png)

range[1,4] 加5，A[1] 加5 ，A[5] 減5。
For example, if we want to increase the original array values between positions 1 and 4 by 5, it sufﬁces to 
- increase the difference array value at position 1 by 5
- decrease the value at position 5 by 5. The result is as follows:

![](https://i.imgur.com/jnKbKmY.png)


## Ref
[花花酱 Fenwick Tree / Binary Indexed Tree - 刷题找工作 SP3](https://www.youtube.com/watch?v=WbafSgetDDk)
[花花酱 Segment Tree 线段树 - 刷题找工作 SP14](https://www.youtube.com/watch?v=rYBtViWXYeI)
[Competitive Programmer's Handbook](https://github.com/pllk/cphb)