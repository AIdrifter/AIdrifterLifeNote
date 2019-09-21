# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook)  <br>  Competitive Programming's Handbook <br> Ch10: Bit manipulation
# Bit manipulation
電腦採用二進制表示法，讓我們看看有哪些Bit operations，還有哪些演算法運用吧。
## Bit representation
32bit整數的表示方法:

$00000000000000000000000000101011$

$=> b_k 2^k + \ldots + b_2 2^2 + b_1 2^1 + b_0 2^0$

$=> 1 \cdot 2^5 + 1 \cdot 2^3 + 1 \cdot 2^1 + 1 \cdot 2^0 = 43.$

### INT32 singend
$-2^{n-1}$ and $2^{n-1}-1$.
$=> -2^{31}$ and $2^{31}-1$

### UINT32 unsinged
$0$ and $2^n-1$.
=> $0$ and $2^{32}-1$.
### Equation
A signed number $-x$ equals an unsigned number $2^n-x$.
$x=-43$ equals the unsigned number $y=2^{32}-43$:

```cpp
int x = -43;
unsigned int y = x;
cout << x << "\n"; // -43
cout << y << "\n"; // 2^32 - 43 = 4294967253
```
### Overflow
如果發生overflow，會直接從$2^{n-1}-1$ 變成 $-2^{n-1}$，也就是$01111$ -> $10000$。
If a number is larger than the upper bound
of the bit representation, the number will overflow.
In a signed representation.
The next number after $2^{n-1}-1$ is $-2^{n-1}$
$01111$ -> $10000$
```cpp
int x = 2147483647
cout << x << "\n"; // 2147483647
x++;
cout << x << "\n"; // -2147483648
```
## Bit Operations
### AND OR XOR
因為$AND$ $OR$ 實在是太簡單了，這邊就不寫上來了，要特別記住XOR運算為:
**相同元素會得到 0元素**  => A $xor$ A = 0

![](https://i.imgur.com/OwVMsMe.png)

### NOT : x = -x-1
The ${not}$ operation $x$ produces a number where all the bits of $x$
have been inverted.
The formula $x = -x-1$ holds, for example, $29 = -30$.

![](https://i.imgur.com/sdMmZ16.png)
### Bit Shifts
$14 < < 2 = 56$, correspond to 1110 and 111000.
$49 > > 3 = 6$, orrespond to 110001 and 110.
:::info
邏輯移位(Logical shift) VS 算術移位(Arithmetic shift)
- 簡單來說Logical shift全部補0
- Arithmetic shift，右移需要考慮到**singed**的屬性,假若最高位是1,則填補1,最高位是0,則填補0.
- 邏輯跟算數的差異在於被平移的數字以 singed 還是 unsigned 來表示 , 如果被平移的數是unsigned的,則會進行邏輯平移,如果被平移的數是signed則會進行算數平移.

:::
### Applications
檢查x第k位數是不是1， 
`x & (1<<k)`
```cpp
for (int i = 31; i >= 0; i--) {
    if (x&(1<<i)) cout << "1";
    else cout << "0";
}
```
設x第k個位數為1
`x |(1<<K)`

設x第k個位數為0
`x & ~(1<<K)`

設x最後一個bit為0
`x & (x-1)`

設x全部的bit為0，最後一個bit維持原樣(非32bit，而是倒數最後一個為1的)。
sets all the one bits to zero, except for the last one bit.
`x & -x`
```
  1001 -7
& 0111  7
--------
  0001  1
  
  1010 -6
& 0110  6
--------
  0010  2
```

把x最後一個1，後面的位元全部翻轉，例如 x=-12。
The formula $x$ | $(x-1)$ inverts all the bits after the last one bit.
```
    *
  10100 -12
| 00011 -13
-------- 
  10111

```

判斷正整數x是不是2的次方。
A positive number $x$ is a power of two exactly when $x$ \& $(x-1) = 0$.
```
    *
  01000  8
& 00111 -7
-------- 
      0

```
### Additional functions
The g++ compiler provides the following
functions for counting bits:

${\_\_builtin\_clz}(x)$:
the number of zeros at the beginning of the number
x左邊有幾個0。

${\_\_builtin\_ctz}(x)$:
the number of zeros at the end of the number
x右邊有幾個0。

${\_\_builtin\_popcount}(x)$:
the number of ones in the number
x總共有幾個1。

${\_\_builtin\_parity}(x)$:
the parity (even or odd) of the number of ones
x的位元數1，總共是奇數個1，還是偶數個1。

```cpp
int x = 5328; // 00000000000000000001010011010000
cout << __builtin_clz(x) << "\n"; // 19
cout << __builtin_ctz(x) << "\n"; // 4
cout << __builtin_popcount(x) << "\n"; // 5
cout << __builtin_parity(x) << "\n"; // 1
```
:::info
While the above functions only support $\texttt{int}$ numbers,
there are also $\texttt{long long}$ versions of
the functions available with the suffix $\texttt{ll}$.
**__builtin_clz(x) => __builtin_clzll(x)**
:::

## Representing Sets
用bit表示方法，來表示set是一個聰明的做法，舉例來說:

The bit representation of the set $\{1,3,4,8\}$ is
$[00000000000000000000000100011010]$
which corresponds to the number $2^8+2^4+2^3+2^1=282$.

### Set Implementation
用or去加入元素到set內，用`__builtin_popcount(x)`計算出set內有多少元素。
```cpp
int x = 0;
x |= (1<<1);
x |= (1<<3);
x |= (1<<4);
x |= (1<<8);
cout << __builtin_popcount(x) << "\n"; // 4

for (int i = 0; i < 32; i++) {
    if (x&(1<<i)) cout << i << " ";
}
// output: 1 3 4 8

```

### Set Operations
![](https://i.imgur.com/YZJvuQ4.png)

For example, the following code first constructs
the sets $x=\{1,3,4,8\}$ and $y=\{3,6,8,9\}$,
and then constructs the set $z = x \cup y = \{1,3,4,6,8,9\}$:

```cpp
int x = (1<<1)|(1<<3)|(1<<4)|(1<<8);
int y = (1<<3)|(1<<6)|(1<<8)|(1<<9);
int z = x|y;
cout << __builtin_popcount(z) << "\n"; // 6
```
### Iterating Through subsets
- 列舉出全部set內元素的組合方式。
```cpp
for (int b = 0; b < (1<<n); b++) {
    // process subset b
}
```

- 列舉出全部set內**元素個數為k**的組合方式。
```cpp
for (int b = 0; b < (1<<n); b++) {
    if (__builtin_popcount(b) == k) {
        // process subset b
    }
}
```
:::info
[FIXME] (b=(b-x)&x)
:::
The following code goes through the subsets of a set x :
```cpp
int b = 0;
do {
    // process subset b
} while (b=(b-x)&x);
```
```
x = {4,2,1}
    
  01011  11 (x)
& 10101 -11 (b-x)
-------- 
      1

```

## Bit optimizations
有哪些演算法可以用bit operation來優化呢?
### Hamming distance
漢明碼距離為，兩字串a,b間，位置1-1 map，有幾個位數不一樣呢?
$\texttt{hamming}(a,b)$ between two strings $a$ and $b$ of equal length is
the number of positions where the strings differ.
For example,

$\texttt{hamming}(01101,11001)=2$

題目: 給一列字串，求字串中最小的漢明距:  $O(n^2 k)$
時間複雜度為 $C(n,2) * k$ 
$=> n(n-1)/2 * k$ 
Consider the following problem: 
Given a list of $n$ bit strings, each of length $k$,
calculate the minimum Hamming distance
between two strings in the list.
For example, the answer for $[00111,01101,11110]$ is 2, because

$\texttt{hamming}(00111,01101)=2$,
$\texttt{hamming}(00111,11110)=3$, and
$\texttt{hamming}(01101,11110)=3$.

可以直接用$O(k)$方式去compare:
```cpp
int hamming(string a, string b) {
    int d = 0;
    for (int i = 0; i < k; i++) {
        if (a[i] != b[i]) d++;
    }
    return d;
}
```
或是利用xor特性，只有相異元素會被保留下來，時間複雜度從 $O(n^2 k) => O(n^2)$
```cpp
int hamming(int a, int b) {
    return __builtin_popcount(a^b);
}
```
### Counting subgrids
找出全部的subgrids，subgrid存在條件為corner為黑色。
Given an $n \times n$ grid whose
each square is either black (1) or white (0),
calculate the number of subgrids whose all corners are black.
![](https://i.imgur.com/iigG6MC.png)
contains two such subgrids:
![](https://i.imgur.com/pideNUC.png)

There is an $O(n^3)$ time algorithm for solving the problem:
go through all $O(n^2)$ pairs of rows and for each pair
$(a,b)$ calculate the number of columns that contain a black
square in both rows in $O(n)$ time.
The following code assumes that $\texttt{color}[y][x]$
denotes the color in row $y$ and column $x$:
```cpp
int count = 0;
for (int i = 0; i < n; i++) {
    if (color[a][i] == 1 && color[b][i] == 1) count++;
}
```
Then, those columns
account for $\texttt{count}(\texttt{count}-1)/2$ subgrids with black corners,
because we can choose any two of them to form a subgrid.

To optimize this algorithm, we divide the grid into blocks
of columns such that each block consists of $N$
consecutive columns. Then, each row is stored as
a list of $N$-bit numbers that describe the colors
of the squares. Now we can process $N$ columns at the same time
using bit operations. In the following code,
```cpp
int count = 0;
for (int i = 0; i <= n/N; i++) {
    count += __builtin_popcount(color[a][i]&color[b][i]);
}
```
The resulting algorithm works in $O(n^3/N)$ time.

## Dynamic Programming
利用bit operations可以很好的優化集合內元素的操作，下面是一些例子:
Bit operations provide an efficient and convenient
way to implement dynamic programming algorithms
whose states contain subsets of elements,
### Optimal Selection
一天只能買一個產品，要怎麼買，才可以花最少錢買到全部的產品？
We are given the prices of $k$ products
over $n$ days, and we want to buy each product **exactly once**.
However, we are allowed to buy at most one product in a day.
What is the minimum total price?
![](https://i.imgur.com/R1ZW2Ap.png)
In this scenario, the minimum total price is $5$:

$\texttt{price}[x][d]$ : 產品x第d天的價格，$\texttt{price}[2][3] = 7$.
Let $\texttt{price}[x][d]$ denote the price of product $x$ on day $d$.
For example, in the above scenario $\texttt{price}[2][3] = 7$.

$\texttt{total}(S,d)$ : 買到產品{0,1..s}第d天最便宜的價格。
Let $\texttt{total}(S,d)$ denote the minimum total
price for buying a subset $S$ of products by day $d$.

Using this function, the solution to the problem is
$\texttt{total}(\{0 \ldots k-1\},n-1)$.


First, $\texttt{total}(\emptyset,d) = 0$,
because it does not cost anything to buy an empty set,
and $\texttt{total}(\{x\},0) = \texttt{price}[x][0]$,

because there is one way to buy one product on the first day.
Then, the following recurrence can be used:


\begin{equation*}
\begin{split}
\texttt{total}(S,d) = \min( & \texttt{total}(S,d-1),  不拿產品x　在第d天 \\
& \min_{x \in S} (\texttt{total}(S \setminus x,d-1)+\texttt{price}[x][d]))    拿產品x　在第d天
\end{split}
\end{equation*}

K個產品，共n天。
```cpp
int total[1<<K][N];
```
先把d=0，也就是第0天先初始化。
```cpp
for (int x = 0; x < k; x++) {
    total[1<<x][0] = price[x][0];
}
```

`s^(1<<x)` 代表集合$s$不拿產品x
```cpp

for (int d = 1; d < n; d++) {
    for (int s = 0; s < (1<<k); s++) {
        total[s][d] = total[s][d-1];
        for (int x = 0; x < k; x++) {
            if (s&(1<<x)) {
                total[s][d] = min(total[s][d],
                                    total[s^(1<<x)][d-1]+price[x][d]);
            }
        }
    }
}
```
The time complexity of the algorithm is $O(n 2^k k)$.
![](https://i.imgur.com/csotCJY.png)

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::min

using namespace std;

int main() {

    int k = 3;
    int n = 8;
    int price[3][8] = {{6,9,5,2,8,9,1,6},{8,2,6,2,7,5,7,2},{5,3,9,7,3,5,1,4}};

    int total[1<<3][8] = {0};

    for (int x = 0; x < k; x++) {
        total[1<<x][0] = price[x][0];
    }

    for (int d = 1; d < n; d++) {
        for (int s = 0; s < (1<<k); s++) {
            total[s][d] = total[s][d-1];
            for (int x = 0; x < k; x++) {
                if (s&(1<<x)) {
                    total[s][d] = min(total[s][d],
                                        total[s^(1<<x)][d-1]+price[x][d]);
                }
            }
        }
    }
    return 0;
}

```
### From Permutations to subsets
通常排列會花掉 $n!$ 的cost，遠本 $2^n$還大，所以如何有效地列舉排列的set，是我們要學習的地方：
The benefit of this is that　$n!$, the number of permutations,　is much larger than $2^n$

題目：電梯最大可承重$x$，總共要$n$個人搭電梯，總共要幾趟才能坐完電梯呢？
As an example, consider the following problem:
There is an elevator with maximum weight $x$,
and $n$ people with known weights
who want to get from the ground floor　to the top floor.
What is the minimum number of rides needed
if the people enter the elevator in an optimal order?

![](https://i.imgur.com/cIVnhBD.png)


在這case，以weight x=10為例，總共要兩趟，分別是$\{0,2,3\}$ 和　$\{1,4\}$ 兩組。
In this case, the minimum number of rides is 2.
One optimal order is $\{0,2,3,1,4\}$,
which partitions the people into two rides:
first $\{0,2,3\}$ (total weight 10),
and then $\{1,4\}$ (total weight 9).

The problem can be easily solved in $O(n! n)$ time
by testing all possible permutations of $n$ people.
However, we can use dynamic programming to get
a more efficient $O(2^n n)$ time algorithm.

The idea is to calculate for each subset of people
two values: the minimum number of rides needed and
the minimum weight of people who ride in the last group.

Let $\texttt{weight}[p]$ denote the weight of person $p$.
We define two functions:
- $\texttt{rides}(S)$ is the minimum number of rides for a subset $S$,
- $\texttt{last}(S)$ is the minimum weight of the last ride.

For example, in the above scenario

$\texttt{rides}(\{1,3,4\})=2 \hspace{10px} \textrm{and}
\hspace{10px} \texttt{last}(\{1,3,4\})=5$

because the optimal rides are $\{1,4\}$ and $\{3\}$,
and the second ride has weight 5.
Of course, our final goal is to calculate the value
of $\texttt{rides}(\{0 \ldots n-1\})$.

We can calculate the values
of the functions recursively and then apply
dynamic programming.
The idea is to go through all people
who belong to $S$ and optimally
choose the last person $p$ who enters the elevator.
Each such choice yields a subproblem
for a smaller subset of people.
If $\texttt{last}(S \setminus p)+\texttt{weight}[p] \le x$,
we can add $p$ to the last ride.
Otherwise, we have to reserve a new ride
that initially only contains $p$.

To implement dynamic programming,
we declare an array
```cpp
pair<int,int> best[1<<N];
```
that contains for each subset $S$
a pair $(\texttt{rides}(S),\texttt{last}(S))$.
We set the value for an empty group as follows:
```cpp
best[0] = {1,0};
```
Then, we can fill the array as follows:

```cpp
for (int s = 1; s < (1<<n); s++) {
    // initial value: n+1 rides are needed
    best[s] = {n+1,0};
    for (int p = 0; p < n; p++) {
        if (s&(1<<p)) {
            auto option = best[s^(1<<p)];
            if (option.second+weight[p] <= x) {
                // add p to an existing ride
                option.second += weight[p];
            } else {
                // reserve a new ride for p
                option.first++;
                option.second = weight[p];
            }
            best[s] = min(best[s], option);
        }
    }
}
```
Note that the above loop guarantees that
for any two subsets $S_1$ and $S_2$
such that $S_1 \subset S_2$, we process $S_1$ before $S_2$.
Thus, the dynamic programming values are calculated in the
correct order.

### Counting subsets
在集合中，每組subset都被設定一個整數值，請計算全部的subset的value總和。
Our last problem in this chapter is as follows:
Let $X=\{0 \ldots n-1\}$, and each subset $S \subset X$
is assigned an integer $\texttt{value}[S]$.
Our task is to calculate for each $S$

$\texttt{sum}(S) = \sum_{A \subset S} \texttt{value}[A]$

![](https://i.imgur.com/Euzdcdc.png)


i.e., the sum of values of subsets of $S$.
\begin{equation*}
\begin{split}
\texttt{sum}(\{0,2\}) &= \texttt{value}[\emptyset]+\texttt{value}[\{0\}]+\texttt{value}[\{2\}]+\texttt{value}[\{0,2\}] \\ 
                      &= 3 + 1 + 5 + 1 = 10.
\end{split}
\end{equation*}

列舉法:$O(2^{2n})$
DP: $O(2^n n)$

$\texttt{partial}(S,k)$ : 0,1..k 個元素可以從$S$中移除。
Let $\texttt{partial}(S,k)$ denote the sum of
values of subsets of $S$ with the restriction
that only elements $0 \ldots k$ may be removed from $S$.

$\texttt{partial}(\{0,2\},1)=\texttt{value}[\{2\}]+\texttt{value}[\{0,2\}]$

這邊又是**小黑的故事**，可以拿k或不拿k。
because we may only remove elements $0 \ldots 1$.
We can calculate values of $\texttt{sum}$ using
values of $\texttt{partial}$, because $\texttt{sum}(S) = \texttt{partial}(S,n-1)$

The base cases for the function are
$\texttt{partial}(S,-1)=\texttt{value}[S]$

because in this case no elements can be removed from $S$.
Then, in the general case we can use the following recurrence:
\begin{equation*}
    \texttt{partial}(S,k) = \begin{cases}
               \texttt{partial}(S,k-1) & k \notin S \\
               \texttt{partial}(S,k-1) + \texttt{partial}(S \setminus \{k\},k-1) & k \in S
           \end{cases}
\end{equation*}
Here we focus on the element $k$.
If $k \in S$, we have two options: we may either keep $k$ in $S$
or remove it from $S$.

```cpp
int sum[1<<N];
```
that will contain the sum of each subset.
The array is initialized as follows:
```cpp
for (int s = 0; s < (1<<n); s++) {
    sum[s] = value[s];
}
```
Then, we can fill the array as follows:
```cpp
for (int k = 0; k < n; k++) {
    for (int s = 0; s < (1<<n); s++) {
        if (s&(1<<k)) sum[s] += sum[s^(1<<k)];
    }
}
```