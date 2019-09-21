
# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) Competitive Programmer's Handbook : <br> Ch1. Introduction <br> Ch2.Time Complexity
# Basic thechniques
# Introduction
## Programming Language
2017 code jam 有79% 參賽者使用C++ (3000 participants).
## Input and output
- C++ Template
    - include `<bits/stdc++.h>`
    - `g++ -std=c++11 -O2 -Wall test.cpp -o test`
        - `-Wall` : shows warnings about possible errors(-Wall)
    - 加速IO
        - `ios::sync_with_stdio(0); cin.tie(0);` => make io more effiient
        - `\n` is faster than `endl`, becuase `endl` need flush operation
    - `scanf()`, `printf()` 還是c的比較快
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {

    // enhance IO function
    ios::sync_with_stdio(0);
    cin.tie(0);

    freopen("input.txt","r",stdin);
    freopen("output.xtt","w",stdout);

    // possibly containing "spaces"
    string s;
    getline(cin ,s)

    // if the amount data is unknown
    while(cin>>x){
        // code
    }

    int T,N;
    scanf("%d %d",&T,&N);
    for(int i=1;i<T;i++){
        for(int j=0;j<N;j++)
    }
}
```
## Working with numbers
### Intergers
- LL is suffix
- `int * int = int`  =>  所以一定要轉型
        - `(long long)`int*int
```C++
long long x = 123456789123456789LL;

int a = 123456789;
long long res = (long long)a * a;
```

### Modular Arithmetic(模數運算)
(a+b)%m = ((a%m) + (b%m))%m
(a-b)%m = ((a%m) - (b%m))%m
(a*b)%m = ((a%m) * (b%m))%m

- 求n!%m
```cpp
long long x = 1;
for(int i=1;i<=n;i++)
    x = (x*i)%m;
```

- C++ 內 modular 為負數
    - the remainder of a negative number is either zero or negative.
```cpp
x = x%m;
if(x<0) x+= m;
```
### Floating Point numbers
- 印出小數點後九位數
```cpp
printf("%.9f\n", x);
```
- rounding errors(捨入誤差)
```cpp
double x = 0.3*3 + 0.1;
printf("%.20f\n", x) // 0.99999999999999999898
```
- IEEE 754會有誤差 所以...
    - A better way to compare floating point numbers is to assume that two numbers are equal , if the difference between them is less than $\varepsilon$, where $\varepsilon$ is a small number.
    - In practice, the numbers can be compared as follows ($\varepsilon=10^{-9}$):
    - using \texttt{double},it is possible to accurately represent all integers whose absolute value is at most $2^{53}$.

```cpp
if(abs(a-b) < 1e-9){
    // a and b are equal
}
```

## Shortening code
- type name and macro
```cpp
#define F first
#define S second
#define PB push_back
#define MP make_pair
#define REP(i,a,b) for(int i = a; i<=b; i++)

typedef long long ll;
typedef vector<int> vi;
typedef pair<int,int> pi;


ll a = 123456789, b=987654321;
cout<<a*b<<endl;
v.PB(MP(y1,x1));
v.PB(MP(y2,x2));
int d = v[i].F + v[i].S;

REP(i,1,n){
    search(i);
}
```
## Mathematics
### Sum formula
$\sum_{x=1}^n x = 1+2+3+\ldots+n = \frac{n(n+1)}{2}$

and

$\sum_{x=1}^n x^2 = 1^2+2^2+3^2+\ldots+n^2 = \frac{n(n+1)(2n+1)}{6}$
### Arithmetic progression(等差數列)
$3+7+11+15=\frac{4 \cdot (3+15)}{2} = 36$
and
$\underbrace{a + \cdots + b}_{n \,\, \textrm{numbers}} = \frac{n(a+b)}{2}$

### Geometric progression（等比數列）

$a + ak + ak^2 + \cdots +  ak^{n-1} = \frac{a(1-k^n)}{k-1}$

special case
$1+2+4+8+\ldots+2^{n-1}=2^n-1.$
### harmonic sum(調和數)
$\sum_{x=1}^n \frac{1}{x} = 1+\frac{1}{2}+\frac{1}{3}+\ldots+\frac{1}{n} = \log_2(n)+1$

$1+\frac{1}{2}+\frac{1}{3}+\frac{1}{4}+\frac{1}{5}+\frac{1}{6} \le
1+\frac{1}{2}+\frac{1}{2}+\frac{1}{4}+\frac{1}{4}+\frac{1}{4}$

### functions
- celing floor max min
- Fibonacci numbers
    - Binets's Formula

### Set Theory
- operation
    - intersection
    - union
    - difference
- Number
$\mathbb{N}$ (natural numbers),
$\mathbb{Z}$ (integers),
$\mathbb{Q}$ (rational numbers) and
$\mathbb{R}$ (real numbers).

### Logic
- **Truth Table**
The value of a logical expression is either
${true}$ (1) or ${false}$ (0).
The most important logical operators are
$\lnot$ ($negation$),
$\land$ ($conjunction$),
$\lor$ ($disjunction$),
$\Rightarrow$ ($implication$)
$\Leftrightarrow$ ($equivalence$).
The following table shows the meanings of these operators:

    - To evaluate a if an argument form is valid, the trusty old truth table can be used to evaluate if $(p_1 \land p_2 \land ... \land p_n) \to c$ is indeed a tautology. Using the argument form described above:
    $$
    \begin{align*}
    &p\to q\\
    &p \\
    \hline
    \therefore ~&q
    \end{align*}
    $$
    The truth table:

    | $p$  | $q$  | $p\to q$ | $((p \to q) \land p) \to q$ |
    | :--: | :--: | :------: | :-------------------------: |
    |  1   |  1   |    1     |              T              |
    |  1   |  0   |    0     |              T              |
    |  0   |  1   |    1     |              T              |
    |  0   |  0   |    1     |              T              |


- A **predicate** is an expression that is true or false
depending on its parameters.
we can define a predicate $P(x)$
that is true exactly when $x$ is a prime number.
Using this definition, $P(7)$ is true but $P(8)$ is false.

- A **quantifier**
    - $\forall$ (**for all**) and $\exists$ (**there is**).

### Logarithms
The logarithm of a product is
$log_k(ab) = \log_k(a)+\log_k(b),$

and consequently,
$log_k(x^n) = n \cdot \log_k(x)$

In addition, the logarithm of a quotient is
$log_k\Big(\frac{a}{b}\Big) = \log_k(a)-\log_k(b)$

Another useful formula is
$log_u(x) = \frac{\log_k(x)}{\log_k(u)}$

The **natural logarithm** $\ln(x)$ of a number $x$
is a logarithm whose base is $e \approx 2.71828$.
Another property of logarithms is that
the number of digits of an integer $x$ in base $b$ is
$\lfloor \log_b(x)+1 \rfloor$.
For example, the representation of
$123$ in base $2$ is 1111011 and
$\lfloor \log_2(123)+1 \rfloor = 7$.
## Content and resources
IOI- Ihe International Olympiad in Informations
ICPC - The International Collegiate Programming Contest

# Time Complexity
Asymptopic Notation: Big O

## Caculation rules
- Order of magnitude
    - 我們忽略係數 : O(3n) = O(n)
    - 選最大的single phase 因為是bottleneck : O(n^3)
    - Several Variables : O(mn)
- O(n^3) + O(nm) + O(n) = O(n^3)
```cpp
// O(3*n)
for (int i = 1; i <= 3*n; i++) {
    // code
}

// O(n^3)
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
        for (int k = 1; k <= n; j++) {
        // code
        }
    }
}

for (int i = 1; i <= n; i++) {
    // code
}
```
- Recursion
    - 以前離散的recursicve function
- f(n) = f(n-1)+1, n>=1
- f(n) = o(n)
```cpp
void f(int n) {
    if (n == 1) return;
    f(n-1);
}
```
- g(n) = 2g(n-1) + 1, n>=1
- g(n) = O(2^n)
```cpp
void g(int n) {
    if (n == 1) return;
    g(n-1);
    g(n-1);
}
```
## Complexity Classess
- 除了O(2^n) 和 O(n!) 其他都是polynomial time
- 非polynomial time
    - NP-hard problems are an important set of problems, for which no polynomial algorithms known.


$O(1)$
The running time of a **constant-time}algorithm**
does not depend on the input size.
A typical constant-time algorithm is a direct
formula that calculates the answer.

$O(\log n)$
A **logarithmic** algorithm often halves
the input size at each step.
The running time of such an algorithm
is logarithmic, because
$log_2 n$ equals the number of times
$n$ must be `divided by 2 to get 1`.

$O(\sqrt n)$
A **square root algorithm** is slower than
$O(\log n)$ but faster than $O(n)$.
A special property of square roots is that
$\sqrt n = n/\sqrt n$, so the square root $\sqrt n$ lies,
in some sense, in the middle of the input.

$O(n)$
A **linear** algorithm goes through the input
a constant number of times.
This is often the best possible time complexity,
because it is usually necessary to `access each
input element` at least once before
reporting the answer.

$O(n \log n)$
This time complexity often indicates that the
algorithm sorts the input,
because the time complexity of efficient
`sorting` algorithms is $O(n \log n)$.
Another possibility is that the algorithm
uses a data structure where each operation
takes $O(\log n)$ time.

$O(n^2)$
A **quadratic** algorithm often contains
two nested loops.
It is possible to go through all `pairs` of
the input elements in $O(n^2)$ time.

$O(n^3)$
A **cubic** algorithm often contains
three nested loops.
It is possible to go through all `triplets` of
the input elements in $O(n^3)$ time.

$O(2^n)$
This time complexity often indicates that
the algorithm iterates through all
**subsets** of the input elements.
For example, the subsets of $\{1,2,3\}$ are
$\emptyset$, $\{1\}$, $\{2\}$, $\{3\}$, $\{1,2\}$,
$\{1,3\}$, $\{2,3\}$ and $\{1,2,3\}$.

$O(n!)$
This time complexity often indicates that
the algorithm iterates through all
**permutations** of the input elements.
For example, the permutations of $\{1,2,3\}$ are
$(1,2,3)$, $(1,3,2)$, $(2,1,3)$, $(2,3,1)$,
$(3,1,2)$ and $(3,2,1)$.
## Estimating efficiency
- 根據不同的input size，其實可以大概推估演算法所需的複雜度。
- 實際跑測試，不能忘記constant factors，O(n)下除以2，或是*2都是相差兩倍的時間。

| $input size$  | $required time complexity$  |
| :--: | :--: |
|  $n \le 10$  |  $O(n!)$   |
|  $n \le 20$   |  $O(2^n)$   |
|  $n \le 500$  |  $O(n^3)$   |
|  $n \le 5000$   | $O(n^2)$   |
|  $n \le 10^6$  | $O(n \log n)$ or $O(n)$   |
|  $n$ is large   | $O(1)$ or $O(\log n)$  |
## Maximum subarray sum
題目條件
- 求出最大的連續元素相加總合
- 元素可以為負
- an empty subarray is allowed, so the maximum subarray sum is alwasys at least 0
![](https://i.imgur.com/oAxaVI8.png)


### Brute Force　$O(n^3)$
固定起始點**a**，**b**為結束距離，用k去紀錄`{0,n}`中的n個元素。
```console
{0,0} {0,1}  ...  ... {0,n}
      {1,1} {1,2} ... {1,n}
                      {n,n}
```
```cpp
int best = 0;
for (int a = 0; a < n; a++) {
    for (int b = a; b < n; b++) {
        int sum = 0;
        for (int k = a; k <= b; k++) {
            sum += array[k];
        }
        best = max(best,sum);
    }
}
cout << best << "\n";
```
### Prefix sum　$O(n^2)$
固定起始點**a**，**b**為結束距離，但是不需要用k去重算上次算好的結果，`{0,n} = {0,n-1} + {n,n}`
```cpp
int best = 0;
for (int a = 0; a < n; a++) {
    int sum = 0;
    for (int b = a; b < n; b++) {
        sum += array[b];
        best = max(best,sum);
    }
}
```
### One loop $O(n)$
Consider the subproblem of finding the maximum-sum subarray
that ends at position $k$.
There are two possibilities:

- The subarray only contains the element at position $k$.
- The subarray consists of a subarray that ends
at position $k-1$, followed by the element at position $k$.

In the latter case, since we want to
find a subarray with maximum sum,
the subarray that ends at position $k-1$
should also have the maximum sum.
Thus, we can solve the problem efficiently
by calculating the maximum subarray sum
for each ending position from left to right.


```cpp
int best = 0, sum = 0;
for (int k = 0; k < n; k++) {
    sum = max(array[k],sum+array[k]);
    best = max(best,sum);
}
cout << best << "\n";
```


