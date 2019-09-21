# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbookb) <br>Competitive Programming's Handbook <br> Ch5 :Complete Search
# Complete Search 窮舉法
- 常見有暴搜法 (brute force)，列舉 (straight-forward))
- 如果搞不定在讓**greedy algorithms** or **dynamic programming**出馬。



## Generating subsets
先從如何簡單產生set有可能開始 - **組合**。
for example, the subsets of $\{0,1,2\}$ are
$\emptyset$, $\{0\}$, $\{1\}$, $\{2\}$, $\{0,1\}$, $\{0,2\}$, $\{1,2\}$ and $\{0,1,2\}$.
### Recursive
- `n = 3`, k自0開始。
```cpp
void search(int k) {
    if (k == n) {
        // process subset
    } else {
        search(k+1);
        subset.push_back(k);
        search(k+1);
        subset.pop_back();
    }
}
```
```cpp
int n = 3;
vector<int> subset;
void search(int k) {
    cout<<" search(" << k<< ")";
    if (k == n) {
        cout<<endl;
        for(auto s: subset)
            cout<<s<<" ";
        cout<<endl;
    } else {
        search(k+1);
        cout<<" push(" << k<< ")";
        subset.push_back(k);
        search(k+1);
        cout<<" pop()";
        subset.pop_back();
    }
}
```
```shell
 search(0) search(1) search(2) search(3)

 push(2) search(3)
2
 pop() push(1) search(2) search(3)
1
 push(2) search(3)
1 2
 pop() pop() push(0) search(1) search(2) search(3)
0
 push(2) search(3)
0 2
 pop() push(1) search(2) search(3)
0 1
 push(2) search(3)
0 1 2
 pop() pop() pop()

```
![](https://i.imgur.com/wuh9omQ.png)


### Bit representation
For example, the bit representation of 25(b)
is 11001, which corresponds to the subset $\{0,3,4\}$.
```cpp
for (int b = 0; b < (1<<n); b++) {
    vector<int> subset;
    for (int i = 0; i < n; i++) {
        if (b&(1<<i)) subset.push_back(i);
    }
}
```
## Generating permutations
### Recursive
```cpp
void search() {
    if (permutation.size() == n) {
        // process permutation
    } else {
        for (int i = 0; i < n; i++) {
            if (chosen[i]) continue;
            chosen[i] = true;
            permutation.push_back(i);
            search();
            chosen[i] = false;
            permutation.pop_back();
        }
    }
}
```
### next_permutation
```cpp
vector<int> permutation;
for (int i = 0; i < n; i++) {
    permutation.push_back(i);
}
do {
    // process permutation
} while (next_permutation(permutation.begin(),permutation.end()));
```

## Backtracking 回溯法
- 回溯算法知道回頭：當一條路不同的時候，它會退**回到上一個岔路**，而不是從頭開始。

Consider the problem of calculating the number
of ways $n$ queens can be placed on an $n \times n$ chessboard so that
no two queens attack each other.
For example, when $n=4$, there are two possible solutions:

![](https://i.imgur.com/2LtETLg.png)

At the bottom level, the three first configurations are illegal(在第二排，前三個皇后都互吃), because the queens attack each other. However, the fourth configuration is valid and it can beextended to a complete solution by placing two more queens to the board(在第二排的第4個是合法的，他還可以變最後解). There is only one way to place the two remaining queens

![](https://i.imgur.com/ozYwJhi.png)

```cpp
void search(int y) {
    if (y == n) {
        count++;
        return;
    }
    for (int x = 0; x < n; x++) {
        if (column[x] || diag1[x+y] || diag2[x-y+n-1]) continue;
        column[x] = diag1[x+y] = diag2[x-y+n-1] = 1;
        search(y+1);
        column[x] = diag1[x+y] = diag2[x-y+n-1] = 0;
    }
}
```
he array $\texttt{column}$ keeps track of columns that contain a queen,
and the arrays $\texttt{diag1}$ and $\texttt{diag2}$ keep track of diagonals.
It is not allowed to add another queen to a column or diagonal that already contains a queen. 
For example, the columns and diagonals of the $4 \times 4$ board are numbered as follows:

![](https://i.imgur.com/t6ufJWO.png)



## Pruning the search
- 回溯算法：當一條路不同的時候，它會退**回到上一個岔路**，而不是從頭開始；
- 回溯算法 + **剪枝**：當發現一條路有問題的時候，**它就會放棄走下去**，這樣就可以避免走一些不必要的路。

![](https://i.imgur.com/mBeVX2X.png)

原文網址：https://kknews.cc/news/mkr6gv9.html


We can often optimize backtracking by pruning the search tree.
The idea is to add ''intelligence'' to the algorithm
so that it will notice as soon as possible
**if a partial solution cannot be extended to a complete solution.**

Let us consider the problem
of calculating the number of paths
in an $n \times n$ grid from the upper-left corner
to the lower-right corner
從左上到右下， 在7*7的gird下，如果又不要**碰撞**，一共有111712的走法。
下面會介紹各種解法。
![](https://i.imgur.com/Ip1NmYg.png)
### Basic Algorithm
- 直接用backtracking 硬幹。
    - running time: 483 seconds
    - number of recursive calls: 76 billion
### Optimization 1
利用對稱的概念，$down = right$ , $left = up$。
In any solution, we first move one step down or right.
There are always two paths that are symmetric
![](https://i.imgur.com/sDbbZ1c.png)
- running time: 244 seconds
- number of recursive calls: 38 billion

### Optimization 2
如果沒有走完全程，直接到終點(右下)，直接放棄這個解，不用再亂走。

![](https://i.imgur.com/bzAkdTl.png)
- running time: 119 seconds
- number of recursive calls: 20 billion
### Optimization 3
如果撞到牆壁後，把gird拆成兩邊，而另外一邊無法訪問完，直接pruning這個解。

![](https://i.imgur.com/SMTLp6y.png)
- running time: 1.8 seconds
- number of recursive calls: 221 million

### Optimization 4
如果撞到自己走過的路(和貪食蛇很像)，直接pruning當下解。

![](https://i.imgur.com/k9k2ktG.png)

- running time: 0.6 seconds
- number of recursive calls: 69 million

### Conclusion
- the running time of the original algorithm was 483 seconds, and now after the optimizations, the running time is only 0.6 seconds
- This is a usual `phenomenon` in backtracking, because the search tree is usually large and even simple observations can effectively prune the search.
- 對於search tree來說，越前面的step如果可以控制，可以少走很多冤路，所以盡量想辦法往前pruning。
  Especially useful are optimizations that occur during the **first steps** of the algorithm,
i.e., at the top of the search tree.

## 剪枝法 vs 回朔法 vs 分支與限制
- 回溯 Backtracking
    - 採用 “深先搜尋法” (Depth-First Search; DFS)  對狀態空間樹中每一個節點進行檢查
    - 為**遞迴**的應用概念，因此可利用**Stack**保存走訪過程中間所走過的點。

- 分枝與限制 Branch and Bound
    - 策略採用 “廣先搜尋法” (Breadth-First Search; BFS) 對狀態空間樹中每一個節點進行檢查
    - 為**迴圈**的應用概念，因此可利用**Queue**保存走訪過程中間所走過的點


上述的兩個策略，皆透過 “邊界函數” (Bounding Function) 來刪除一些不必要的子樹搜尋動作，以提昇搜尋效率。

- 剪枝
    - 當搜尋到 “不可行” 的節點 (即：沒前途(nonpromising)節點) 時，則不用再去搜尋該節點以下之所有分枝節點。 此即為修剪 (**Pruning**)
    - 當搜尋到 “可行” 的節點 (即：有前途(promising)節點) 時，則可以再續繼往下搜尋該節點以下之分枝節點。
    - 可以得到修剪過的狀態空間樹 (Pruned State Space Tree)。


## Meet in the middle
**把 seach sapce 拆成兩塊，最後在合併**
is a technique
where the search space is divided into
two parts of about equal size.
A separate search is performed
for both of the parts,
and finally the results of the searches are combined.

可以有效把exponential下降，是個非常好的方法。
we can turn a factor of $2^n$
into a factor of $2^{n/2}$ using the meet in the
middle technique.

### Question: Does the sum exist in array ?
題目給一陣列$[2,4,5,9]$，令$x=15$，要如何判斷陣列中元素可否組成 $15$ 呢?
$15 = 2 + 4 + 9$
如果採用列舉出set的全部組合(Complete Search)，需要花掉$O(2^n)$的時間。
For example, given the list $[2,4,5,9]$ and $x=15$,
we can choose the numbers $[2,4,9]$ to get $2+4+9=15$.
However, if $x=10$ for the same list,
it is not possible to form the sum.
A simple algorithm to the problem is to
go through all subsets of the elements and
check if the sum of any of the subsets is $x$.
The running time of such an algorithm is $O(2^n)$,
because there are $2^n$ subsets.

**Meet in the middle** : 把陣列拆成$A=[2,4]$ and $B=[5,9]$.創造兩陣列 :
$S_A=[0,2,4,6]$ and $S_B=[0,5,9,14]$
再從兩陣列中去找組合，最後找到 $S_A$ ($6 = 2 +4$) + $S_B$($9$) = 15
時間複雜度從 $O(2^n)$ 降成 $2*O(2^{n/2})$
# Reference
backtracking:
http://programming-study-notes.blogspot.com/2014/03/backtracking.html

Complete Search:
https://m80126colin.github.io/blog/articles/%E7%BF%BB%E8%AD%AF/usaco/usaco-1-2-1-text/

回朔法:
https://kknews.cc/zh-tw/news/mkr6gv9.html

# Note
類似 $n!$ 種問題還有這些:
- N皇后(N-Queen)問題
- 旅行銷售員問題(Traveling Salesman Problem; TSP)
- 漢米爾頓迴路(Hamiltonian Circuits)
- 圖形著色(Graph-Coloring)


