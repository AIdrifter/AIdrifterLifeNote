# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) Competitive Programming’s Handbook: Ch6: Greedy Algorithms
# Greedy Algorithms
- 選擇的local最佳解，再套用到global也要適用=> 小greddy解 屬於 大greedy解
The **local optimal choices** in greddy algorithm should also be **globally optimal**

- 最難的是證明為何現在這種策略的最佳解，可以套用到global解。
## Coin Problem
給一組硬幣 $\{1,2,5,10,20,50,100,200\}$
，若$n=520$，如何找出最少的硬幣組成? 硬幣可以重複選取。

Ans: $n = 520 = 200+200+100+20$ 

- Greedy: 從最大硬幣開始選取，直到520被湊出來，但是演算法保證正確嗎?
A simple greedy algorithm to the problem
always selects the largest possible coin,
until the required sum of money has been constructed.
- Ans: 證明如下
    - $1, 5 , 10 ,50 , 100$只會在最佳解出現一次。
    For example, if the solution would contain
coins $5+5$, we could replace them by coin $10$.
    - $2 ,20$ 不會超過3次，超過的可以用 ${5,1}$ or ${50,10}$ 取代。
    In the same way, coins 2 and 20 appear
    at most twice in an optimal solution,
    because we could replace
    coins $2+2+2$ by coins $5+1$ and
    coins $20+20+20$ by coins $50+10$.
    Moreover, an optimal solution cannot contain
    coins $2+2+1$ or $20+20+10$,
    because we could replace them by coins $5$ and $50$.
    - 由此可以發現，對每一個x而言，不可能有任何組合出來的硬幣數比這方法多。
    Using these observations,
    we can show for each coin $x$ that
    **it is not possible to optimally construct
    a sum $x$ or any larger sum by only using coins
    that are smaller than $x$**.
    For example, if $x=100$, the largest optimal
    sum using the smaller coins is  $50+20+20+5+2+2=99$.
    Thus, the greedy algorithm that always selects
    the largest coin produces the optimal solution.
:::info
其實筆者覺得這coin greedy algorithms的證明不是很嚴謹，之後會再補上。
:::
- Q1: 此greedy algorithms 有存在反例(counterexample)嗎?
- Ans : $\{1,3,4\}$ and the target sum
is 6, the greedy algorithm produces the solution
$4+1+1$ while the optimal solution is $3+3$.
## Scheduling
- 給n個event，同時給each event的start time, end time，如果event不能平行處理，找出盡可能可以完成的最多event**數量**。
Given $n$ events with their starting and ending
times, find a schedule
that includes as many events as possible.
It is not possible to select an event partially.
For example, consider the following events:
![](https://i.imgur.com/3JMSJ76.png)
For example, we can select events $B$ and $D$
as follows:
![](https://i.imgur.com/k1UEWqz.png)
### 盡可能選擇最短的
select as ${short}$ events as possible.
馬上找到反例。
![](https://i.imgur.com/9o0v436.png)
If we select the short event, we can only select one event.
However, it would be possible to select both long events.
### 盡可能選擇下一個begin time最接近的事件
select the next possible
event that ${begins}$ as ${early}$ as possible.
This algorithm selects the following events:

![](https://i.imgur.com/zux5mnw.png)
看起來不錯，但是找到反例。
However, we can find a counterexample
also for this algorithm.
For example, in the following case,
the algorithm only selects one event:
![](https://i.imgur.com/0mjjpqt.png)

### 盡可能選擇下一個end time最早的
select the next possible event that ${ends}$ as ${early}$ as possible.
:::info
這個才是解，但是書上證明看不是很懂，之後再補，原文如下:
:::
It turns out that this algorithm
${always}$ produces an optimal solution.
The reason for this is that it is always an optimal choice
to first select an event that ends
as early as possible.
After this, it is an optimal choice
to select the next event
using the same strategy, etc.,
until we cannot select any more events.

One way to argue that the algorithm works
is to consider
what happens if we first select an event
that ends later than the event that ends
as early as possible.
Now, we will have at most an equal number of
choices how we can select the next event.
Hence, selecting an event that ends later
can never yield a better solution,
and the greedy algorithm is correct.
## Tasks and deadlines
不同task都有其執行時間，與deadline，如果提前完成可以獲得分數(deadline day - finish day)，要如何獲得最高分呢?
Let us now consider a problem where
we are given $n$ tasks with durations and deadlines
and our task is to choose an order to perform the tasks.
For each task, we earn $d-x$ points
where $d$ is the task's deadline
and $x$ is the moment when we finish the task.
What is the largest possible total score
we can obtain?


![](https://i.imgur.com/HUcJZhh.png)

The optimal solution:
![](https://i.imgur.com/suxb0fw.png)

Surprisingly, the optimal solution to the problem
does not depend on the deadlines at all,
but a correct greedy strategy is to simply

不去管deadline，而是根據task的duration去遞增排序，從最小的duration開始挑選。
perform the tasks **sorted by their durations**
in **increasing order**.

Proof:
The reason for this is that if we ever perform
two tasks one after another such that the first task
takes longer than the second task,
we can obtain a better solution if we swap the tasks.
For example, consider the following schedule:

![](https://i.imgur.com/SeSQWD0.png)

Here $a>b$, so we should swap the tasks:
![](https://i.imgur.com/v9knQQq.png)

$x$ 可以獲得b的分數, 但是$y$獲得a的分數是更多的
Now $X$ gives $b$ points less and $Y$ gives $a$ points more,
so the total score increases by $a-b > 0$.
In an optimal solution,
for any two consecutive tasks,
it must hold that the shorter task comes
before the longer task.
Thus, the tasks must be performed
sorted by their durations.
## Minimizing  sums
找出 $x$ 使得該等式最小。
Give $n$ numbers $a_1,a_2,\ldots,a_n$
and our task is to find a value $x$
that minimizes the sum
$|a_1-x|^c+|a_2-x|^c+\cdots+|a_n-x|^c.$

We focus on the cases $c=1$ and $c=2$.
### Case c=1
- median: 中位數即為最佳解。
$[1,2,9,2,6]$ =>  $[1,2,2,6,9]$   , $2$為中位數。
- 如果$a_1,a_2,\ldots,a_n$為偶數個元素，則中位數會有兩個，兩個都可以是最佳解。
### Case c=2
- 即求Average of the numbers: **mean** 又稱平均數。

Proof:
$(a_1-x)^2+(a_2-x)^2+\cdots+(a_n-x)^2$
= $nx^2 - 2x(a_1+a_2+\cdots+a_n) + (a_1^2+a_2^2+\cdots+a_n^2)$  

令 $a_1+a_2+\cdots+a_n = s$，因為$(a_1^2+a_2^2+\cdots+a_n^2)>0$ 所以不用考慮。
= $nx^2-2xs$
= $x(nx - 2s)$

所以$y=0$ 時
=> $x=0$ or $x=2s/n$
This is a parabola opening **upwards** with roots $x=0$ and $x=2s/n$,

把左右兩點x座標相加，求頂點座標x，二次曲線(curve of second order)開口向下。

$(0 + 2s/n )/2 = s/n$

And the minimum value is the average of the roots $x=s/n$.

![](https://i.imgur.com/9oVBGyX.png)

## Data Compression
A ${binary code}$ assigns for each character of a string a ${codeword}$ that consists of bits. 
We can ${compress}$ the string using the binary code
by replacing each character by the corresponding codeword.

codeword: 密碼字
- We require that no codeword is a **prefix** of another codeword.
 這樣同一組binary code會有兩個以上的string，就沒辦法解壓縮(還原)了。
 下面表格 ${1011}$ 會有 $C$ 或是 $AB$ 兩個答案。
 ![](https://i.imgur.com/MYjlAHe.png)

- constant-length通常比variable-length還要長，下面介紹常見的variable-length: Huffman Coding 。

### Huffman Coding
- 用來壓縮string的演算法，建立binary tree，同時計算每個字元出現的次數，從出現次數小的先合併(因為樹高也長)，往左為0，往右為1。
- string s = ${AABACDACA}$

- 建立weight node
![](https://i.imgur.com/wv9BCxM.png)

- 挑兩個最小的weight node 合併，合併後的parent node要相加。
  {5,2,1,1} => {5,2,2}
![](https://i.imgur.com/wGhAa9p.png)

- {5,2,2} => {5 ,4}
![](https://i.imgur.com/yhUccJX.png)

- {5 ,4} => {9}
![](https://i.imgur.com/arm7BQS.png)

最後建立出來的codewords如下:

![](https://i.imgur.com/Csq9eEM.png)


# Reference
3.2 Job Sequencing with Deadlines - Greedy Method:
https://www.youtube.com/watch?v=zPtI8q9gvX8

 