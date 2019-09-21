# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) <br> Competitive Programmer’s Handbook <br> Ch7: Dynamic Programming
# Dynamic Programming

**Dynamic programming**
is a technique that combines the correctness
of **complete search** and the efficiency of **greedy algorithms**.
Dynamic programming can be applied 
if the problem can be divided into overlapping subproblems
that can be solved independently.
:::info
DP = Complete Search + Greedy algotithms
:::
仔細想想蠻有道理的，Greedy Algorithms只要湊到最終解就好，但是DP透過**建表**會把每個子問題(subproblems)答案都算出來。

There are two uses for dynamic programming:

- **Finding an optimal solution**:
  找到最佳子問題的解
  We want to find a solution that is
  as large as possible or as small as possible.
- **Counting the number of solutions** 
  算出全部的解，這邊我覺得改成最佳解集合包含最佳子問題必較好。(Ref: Cormen
  We want to calculate the total number of
  possible solutions.

如果要把程式競賽當職業，DP會是一個里程碑。
Understanding dynamic programming is a **milestone**
in every competitive programmer's career.
While the basic idea is simple,
the challenge is how to apply
dynamic programming to different problems.

## Coin Problem
可以先參考第六章的 : [Greedy Algorithms : Coin Problem](https://hackmd.io/gesAEePtTvOEFyGSYG8aRA#Coin-Problem)

題目:
Out task is to form the sum $n$ using as few coins as possible.
給一組硬幣 $\{1,2,5,10,20,50,100,200\}$
，若$n=520$，如何找出最少的硬幣組成? 硬幣可以重複選取。

Ans: $n = 520 = 200+200+100+20$ 

DP的本質是brute force(complete asearch)，但是因為它採用表格欄位去紀錄答案(memorization)，所以每個subproblem只要算一次就好。
The dynamic programming
algorithm is efficient because
it uses ${memoization}$ and
calculates the answer to each subproblem only once.

### Recursive Formuation
For example, if $\texttt{coins} = \{1,3,4\}$,
the first values of the function are as :

$\texttt{solve}(10)=3$,
because at least 3 coins are needed to form the sum 10.
The optimal solution is $3+3+4=10$.

\begin{array}{lcl}
\texttt{solve}(0) & = & 0 \\
\texttt{solve}(1) & = & 1 \\
\texttt{solve}(2) & = & 2 \\
\texttt{solve}(3) & = & 1 \\
\texttt{solve}(4) & = & 1 \\
\texttt{solve}(5) & = & 2 \\
\texttt{solve}(6) & = & 2 \\
\texttt{solve}(7) & = & 2 \\
\texttt{solve}(8) & = & 2 \\
\texttt{solve}(9) & = & 3 \\
\texttt{solve}(10) & = & 3 \\
\end{array}


因此我們可以依據條件寫成等式 : 
\begin{equation*}
\begin{split}
\texttt{solve}(x) = \min( & \texttt{solve}(x-1)+1, \\
                           & \texttt{solve}(x-3)+1, \\
                           & \texttt{solve}(x-4)+1).
\end{split}
\end{equation*}


擴展成通式(General Recursive Function)
\begin{equation*}
    \texttt{solve}(x) = \begin{cases}
               \infty               & x < 0\\
               0               & x = 0\\
               \min_{c \in \texttt{coins}} \texttt{solve}(x-c)+1 & x > 0 \\
           \end{cases}
\end{equation*}


```cpp
int solve(int x){
    if(x<0) return INF; // infinity
    if(x==0) return 0;
    int best = INF;
    
    for(auto c:C){
        best = min(best, solve(x-c)+1);
    }
    return best;
}
```
### Using Memoization $O(nK)$
There may be an exponential nunmber of ways to construct the sum.
剛剛上面方法會花掉exponential time去計算$x$，我們利用$memoization$把小問題的解存起來，不用每次都重算。
```cpp
bool ready[N] = {0};
int value[N] = {0};
```
只要多這三行:
```cpp
if(ready[x]) return value[x];

value[x] = best;
ready[x] = true;
```
修改後， **recursion & Memoziation**採用**top-down**方式建構答案。
```cpp
int solve(int x){
    if(x<0) return INF; 
    if(x==0) return 0;
    if(ready[x]) return value[x]; // if slove(x) is ready, stop to compute it.
    
    int best = INF;
    for(auto c:C){
        best = min(best, solve(x-c)+1);
    }
    
    // save subproblem solution of slove(x)
    value[x] = best;
    ready[x] = true;
    
    return best;
}
```

**DP or Iterative** 採用**bottom up**方式建構答案。
```cpp
value[0] = 0;

// caculate slove(x) from 1 to n 
for (int x = 1; x <= n; x++) {
    value[x] = INF;
    for (auto c : coins) {
        // check index boundary
        if (x-c >= 0) {
                    // don't take c    take c     
            value[x] = min(value[x], value[x-c]+1);
        }
    }
}
```
:::info
小作業: INF vs INT_MAX 差異在哪邊?
:::

### Constructing A Solution
有時候題目要求，印出 $solve(10) = 4+4+3$，我們要如何做呢?

Ans: 我們用一個array去記錄每個最佳子問題的第一個硬幣是誰。
     照貪婪演算法，第一個硬幣也會是最大的硬幣，所以湊起來就是最佳解的全部硬幣組合。
```cpp
int first[N];
```
把`value[x-c]+1 < value[x]`這條件從`min()`搬到`if()`上面

```cpp
value[0] = 0;
for (int x = 1; x <= n; x++) {
    value[x] = INF;
    for (auto c : coins) {
        // value[x-c]+1 < value[x]
        if (x-c >= 0 && value[x-c]+1 < value[x]) {
            value[x] = value[x-c]+1; //
            first[x] = c;
        }
    }
}
```
用這方式去印答案:
```cpp
while (n > 0) {
    cout << first[n] << "\n";
    n -= first[n];
}
```

### Couting the number of Solutions
和前面不一樣的是，這次要算出solve(x)有多少種硬幣組合方式，solve(5)為例 :

![](https://i.imgur.com/JOrXHdq.png)

Let $\texttt{solve}(x)$ denote the number of ways 
we can form the sum $x$.

For example, if $\texttt{coins}=\{1,3,4\}$,
then $\texttt{solve}(5)=6$ and the recursive formula is
\begin{equation*}
\begin{split}
\texttt{solve}(x) = & \texttt{solve}(x-1) + \\
                    & \texttt{solve}(x-3) + \\
                    & \texttt{solve}(x-4)  .
\end{split}
\end{equation*}

通式如下，要記住**不放**也算一種方法:
There is only one way to form an empty sum
\begin{equation*}
    \texttt{solve}(x) = \begin{cases}
               0               & x < 0\\
               1               & x = 0\\
               \sum_{c \in \texttt{coins}} \texttt{solve}(x-c) & x > 0 \\
           \end{cases}
\end{equation*}

```cpp
// There is only one way to form an empty sum
count[0] = 1;

int solve(x){
    for (int x=1; x<=n ; x++){
        for (auto c : Coins){
            if (x-c >= 0) count[x] += count[x-c];
        }
    }
}
```
有時候我們不想算出全部solve(x)的解，我們可以運用模數運算(modulo)去化簡:
Often the number of solutions is so large
that it is not required to calculate the exact number
but it is enough to give the answer modulo $m$
where, for example, $m=10^9+7$.
This can be done by changing the code so that
all calculations are done modulo $m$.
In the above code, it **suffices** to add the line

舉例來說 $m=10^9+7$. 把這步加在`count[x] += count[x-c]` 之後。 
```cpp
count[x] += count[x-c];
count[x] %= m;
```
Ref: [Why we like Modulo 10^9+7 (1000000007)](https://www.geeksforgeeks.org/modulo-1097-1000000007/)
:::info
這邊之後找個範例補充一下，霧煞煞。
:::

## Longest increasing subsequence $O(n^2)$
找出不連續的最長遞增子序列。
This is a maximum-length sequence of array elements
that goes from left to right,
and each element in the sequence is larger
than the previous element.
For example, in the array

![](https://i.imgur.com/ES5APlK.png)
![](https://i.imgur.com/SxF7fXj.png)

Let ${length}(k)$ denote the length of the
longest increasing subsequence
that ends at position $k$

![](https://i.imgur.com/Abf71PB.png)
- length(k) = max(lenght(k), length[i]+1)
    - 如果A[K] > A[i] 
      => `length[i] + 1 `(加入k元素) vs `length[k]` (不選k元素，因為沒有變大)

```cpp
for (int k = 0; k < n; k++) {
    length[k] = 1; // at least one
    for (int i = 0; i < k; i++) {
        if (array[i] < array[k]) {        
            length[k] = max(length[k],length[i]+1);
        }
    }
}
```
:::info
It is also possible to implement the dynamic programming calculation
more efficiently in $O(n \log n)$ time.
Can you find a way to do this?
:::
## Paths in grid $O(n^2)$
從左上走到右下，盡量得到最大的數字加總。
we only move down and right.
Each square contains a positive integer,
and the path should be constructed so
that the sum of the values along
the path is as large as possible.

![](https://i.imgur.com/EPId8jF.png)

$\texttt{sum}(y,x)$ 為該square的路徑加總的最大總和。
Let $\texttt{sum}(y,x)$ denote the maximum sum 
on a path from the upper-left corner to square $(y,x)$.


![](https://i.imgur.com/3snPKmT.png)

通式如下:
$\texttt{sum}(y,x) = \max(\texttt{sum}(y,x-1),\texttt{sum}(y-1,x))+\texttt{value}[y][x]$
if $\texttt{sum}(y,x) = 0$ , $x=0$ or $y=0$ // 因為這種path不存在，走不到。



```cpp
for (int y = 1; y <= n; y++) {
    for (int x = 1; x <= n; x++) {
        sum[y][x] = max(sum[y][x-1],sum[y-1][x])+value[y][x];
    }
}
```
## Knapsack Problem $O(nW)$
題目:有辦法用$[w_1,w_2,\ldots,w_n]$ n件物品，組合成重量x嗎?
Problem: Given a list of weights
$[w_1,w_2,\ldots,w_n]$,  $W = w_1+w_2+\ldots,w_n$
determine all sums that can be constructed using the weights.


For example, if the weights are
$[1,3,3,5]$, the following sums are possible:
![](https://i.imgur.com/AwX1pqH.png)

定義 $\texttt{possible}(x,k)$，如果可以用第$k$個物品組成重量$x$ 即為$true$
Let $\texttt{possible}(x,k)=\textrm{true}$ if we can construct a sum $x$ using the first $k$ weights,
and otherwise $\texttt{possible}(x,k)=\textrm{false}$.


等式如下:
- $\texttt{possible}(x,k) = \texttt{possible}(x-w_k,k-1) \lor \texttt{possible}(x,k-1)$
    - $\texttt{possible}(x-w_k,k-1)$ : 拿第k個物品。
    - $\texttt{possible}(x,k-1)$ : 不拿第k個物品。


先用這條件去初始化表格第一列(橘色):
\begin{equation*}
    \texttt{possible}(x,0) = \begin{cases}
               \textrm{true}    & x = 0\\
               \textrm{false}   & x \neq 0 \\
           \end{cases}
\end{equation*}

![](https://i.imgur.com/oFcwPKL.png)


```cpp
possible[0][0] = true;
for (int k = 1; k <= n; k++) {
    for (int x = 0; x <= W; x++) {
        if (x-w[k] >= 0) possible[x][k] |= possible[x-w[k]][k-1];
        possible[x][k] |= possible[x][k-1];
    }
}
```
這邊提供一種one-dimensional做法，從右到左update。
However, There is a better implementation that only uses
a one-dimensional array $\texttt{possible}[x]$
that indicates whether we can construct a subset with sum $x$.
The trick is to update the array **from right to left** for each new weight:
```shell
{1,3,3,5}

                  0  1  2  3  4  5  6  7  8  9  10  11  12 
                  X 
k=1 {1}           X  X
k=2 {1,3}         X  X     X  X
k=3 {1,3,3}       X  X     X  X     X  X
k=4 {1,3,3,5}     X  X     X  X  X  X  X  X  X      X    X             
```
```cpp
possible[0] = true;
for (int k = 1; k <= n; k++) {
    for (int x = W; x >= 0; x--) {
        if (possible[x]) possible[x+w[k]] = true;
    }
}
```

### $01$ Knapsack Problem $O(nm)$
總共有n件物品，重量從$W1....Wn$，對應的價錢是$V1....Vn$
經典的$0/1$背包問題。
![](https://i.imgur.com/IaLGOV1.png)
## Edit distance  $O(nm)$
**萊文斯坦距離**，又稱Levenshtein距離，是編輯距離的一種。 
指兩個字串之間，由一個轉成另一個所需的最少編輯操作次數。
The ${edit distance}$ or ${Levenshtein distance}$
is the **minimum number** of editing operations
needed to transform a string

- insert a character (e.g. ABC→ABC**A**) 
- remove a character (e.g. A**B**C→AC) 
- modify a character (e.g. A**B**C→A**D**C) 

$\texttt{distance}(a,b)$ 為字串長度$a$ 和字串長度$b$下的最短萊文斯坦距離。
Suppose that we are given a string $\texttt{x}$ of length $n$ 
and a string $\texttt{y}$ of length $m$,
and we want to calculate the edit distance between $\texttt{x}$ and $\texttt{y}$.
define a function $\texttt{distance}(a,b)$ that gives the
edit distance between prefixes
$\texttt{x}[0 \ldots a]$ and $\texttt{y}[0 \ldots b]$.
Thus, using this function, the edit distance
between $\texttt{x}$ and $\texttt{y}$ equals $\texttt{distance}(n-1,m-1)$.

**通式如下:**
\begin{equation*}
\begin{split}
\texttt{distance}(a,b) = \min(& \texttt{distance}(a,b-1)+1, \\
                           & \texttt{distance}(a-1,b)+1, \\
                           & \texttt{distance}(a-1,b-1)+\texttt{cost}(a,b)).
\end{split}
\end{equation*}


Here $\texttt{cost}(a,b)=0$ if $\texttt{x}[a]=\texttt{y}[b]$,
and otherwise $\texttt{cost}(a,b)=1$.

The formula considers the following ways to edit the string $\texttt{x}$: 
- (e.g. ABC→ABC**A**) 拿掉$b$內的**A**就和$a$一樣了。
  $\texttt{distance}(a,b-1)$: insert a character at the end of $\texttt{x}$

- (e.g. A**B**C→AC) 拿掉$a$內的**B**就和$b$一樣了。
  $\texttt{distance}(a-1,b)$: remove the last character from $\texttt{x}$

- (e.g. A**B**C→A**D**C)  各拿掉$a$內的**B** 和 $b$內的**D**就都一樣了。
  $\texttt{distance}(a-1,b-1)$: match or modify the last character of $\texttt{x}$

![](https://i.imgur.com/fou9e2k.png)

The lower-right corner of the table tells us that the edit distance 
between $\texttt{LOVE}$ and $\texttt{MOVIE}$ is 2.

還有因為轉折點在$I$ 和 $V$之間，我們可以知道$MOVI$ 和 $LOV$ 兩字串中，
只要從 $MOVI$ 這邊移掉 $I$ 後，
就是**萊文斯坦距離**差 $1$ 的 $MOV$ 和 $LOV$。
![](https://i.imgur.com/1bGMadZ.png)

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        for(int j=1; j<=n; j++)
            dp[0][j] = j;
        
        int cost = 0;
        for(int i=1; i<=m; i++) {
            dp[i][0] = i;
            for(int j=1; j<=n; j++) {
                word1[i-1]!=word2[j-1]? cost=1:cost=0;
                dp[i][j] = min(min(dp[i-1][j]+1, dp[i][j-1]+1), dp[i-1][j-1]+cost);
            }
        }
        
        return dp[m][n];
    }
};
```

Ref: [LeetCode] Edit Distance
http://bangbingsyb.blogspot.com/2014/11/leetcode-edit-distance.html

## Counting tilings
又稱**Domino Tiling**，二格骨牌（Domino）。
找出 $n \times m$ 的grid可以放幾種的Domino ?
![](https://i.imgur.com/V7L01tx.png)
[Pic]:one valid solution for the 4×7 grid 

![](https://i.imgur.com/au2KMin.png)

其實是有數學公式的:

$\ \prod_{a=1}^{\lceil n/2 \rceil} \prod_{b=1}^{\lceil m/2 \rceil} 4 \cdot (\cos^2 \frac{\pi a}{n + 1} + \cos^2 \frac{\pi b}{m+1})$
## Conclusion
- Optimal substructure
- overloapping subproblem
    - 2^n(exponetial) > n^k(polynomial)
    - 如果不考慮overlap就是divde and conquer
- No after affect
    - the optimal solution of a subproblem will not change when it was used to solve a bigger problem optimally.

```shell
                大->小 (Top Down)         大->小(Bottom up)
Recursive -> recursion & Memoziation -> Dynamic Programming
                不需要考慮（找mem）           要確定會不會重複算 
```
- DP精神：要考慮如何處理overlaping 沒有的話就是單純的divide and conquer(有overlap但不鳥他)
    - DP = recursion + re-use
    - Reference: [Difference between Divide and Conquer Algo and Dynamic Programming](https://stackoverflow.com/questions/13538459/difference-between-divide-and-conquer-algo-and-dynamic-programming)
