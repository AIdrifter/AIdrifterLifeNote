# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) <br> Competitive Programming’s Handbook  <br> Ch8 Amortized Analysis 平攤分析 
# Amortized Analysis
用來分析該演算法操作有可能會**變化**的時間複雜度，有的演算法操作，可能會有極端case: 例如$O(1)$~$O(n)$，但是我們分析 **(該操作全部做完後的總複雜度)/ n次**，而不是只看單次。
**Amortized analysis** can be used to analyze
algorithms that contain operations whose
time complexity varies(變化).
The idea is to estimate the total time used to
all such operations during the
execution of the algorithm, **instead of focusing on individual operations**.

實際運用 : 預估時間複雜度**不均勻 unevenly**的資料結構操作，但是該種操作全部做完的複雜度是有限的，

Amortized analysis is often used to
estimate the number of **operations performed on a data structure**.
The operations may be distributed **unevenly** so
that most operations occur during a
certain phase of the algorithm, but the total
**number of the operations is limited**.

## vector push_back()
考慮一個隨元素個數增加而增長的動態陣列，比如Java的ArrayList或者C++的std::vector。如果我們的陣列大小從4開始，那麼來向其中增加四個元素的時間就是一個常數。然而，若要將第五個元素加入其中，那麼會花費更多時間，因為我們此時必須要建立一個兩倍於目前陣列大小的陣列（8個元素），把老元素拷貝到新陣列中，然後增加一個新元素。接下來的三次加入操作也同樣會花費常數時間，然後在陣列被填滿後則又需要一輪新的加倍擴充。

一般地，如果我們考慮任意一個任意的n大小的陣列並對其進行n + 1次加入操作。我們注意到，所有的加入操作都是常數時間的，除了最後一個，它會花費 ${\displaystyle O(n)}$ O(n)時間在大小加倍上。因為我們進行了n + 1次加入操作，我們可以將陣列加倍的時間平攤到所有的加入操作上，因此得到加入操作的平均時間是:

${\displaystyle {\tfrac {nO(1)+O(n)}{n+1}}=O(1)}$

它是一個常數。

![](https://i.imgur.com/k8xTKxM.png)

## Two pointers methods
用兩個指標，用左指標或右指標只能往單一方向，下面有兩個範例。
In the **two pointers method**,
two pointers are used to
iterate through the array values.
Both pointers can move to one direction only,
### Subarray sum $O(n)$
找出連續的subarry，其元素總合為x的解是否存在:
consider a problem where we are
given an array of $n$ positive integers
and a target sum $x$,
and we want to find a subarray whose sum is $x$
or report that there is no such subarray.

$x = 8 = {2+5+1}$
![](https://i.imgur.com/sTNUrw9.png)

The initial subarray contains the values
1, 3 and 2 whose sum is 6:
![](https://i.imgur.com/7RQtxRi.png)

Then, the left pointer moves one step to the right. The right pointer does not move, because otherwise the subarray sum would **exceed** x.
![](https://i.imgur.com/0rsNAmQ.png)

Again, the left pointer moves one step to the right, and this time the right pointer moves three steps to the right. The subarray sum is 2+5+1=8, so a subarray whose sum is x has been found.
![](https://i.imgur.com/g8BFTs1.png)


```cpp
vector<int> v = {1,3,2,5,1,1,2,100};
int x = 100;
auto l = v.begin();
auto r = v.begin();
int sum = 0;
while(l != v.end()){
  
    if (r !=v.end() && sum < x + *r)
        sum += *r, r++;
    else
        sum -= *l, l++;
          
    if (sum == x)
         return true;
}
return false;
```

時間複雜度由左右指標移動次數決定，雖然不清楚右指標會移動幾次，但是不會移超過n步。
$左:O(n) + 右:O(n) = O(2n) = O(n)$

The running time of the algorithm depends on
the number of steps the right pointer moves.
While there is no useful upper bound on how many steps the
pointer can move on a **single** turn.
we know that the pointer moves **a total of**
$O(n)$ steps during the algorithm,
because it only moves to the right.

Since both the left and right pointer
move $O(n)$ steps during the algorithm,
the algorithm works in $O(n)$ time.

### 2 sum problem
google的面試經典題:
在array中，找出兩元素合為x的解。
given an array of $n$ numbers and a target sum $x$, 
find two array values such that their sum is $x$,
or report that no such values exist.

1. 先sortd array $O(nlogn)$
2. 用two pointers methods，如果 $x > l+r$，l指標向右移動，反之指標r向左移動 $O(n)$
```cpp
bool isPairSum(vector<int> A, X) 
{ 
    sort(A.begin(), A.end());

    auto l = A.begin(); 
    auto r = A.end() - 1; 
  
    while (l < r) { 
        if (*l + *r == X) 
            return true; 
        else if (*l + *r < X) 
            l++; 
        else
            r--; 
    } 
    return false; 
} 
```
:::info
進階:
1. 其實也可以用BST來做。
https://www.geeksforgeeks.org/find-pair-given-sum-bst/

2. **3sum** problem可以到 $O(n^3)$， 想看看要怎麼做?
:::
## Nearest smaller elements  $O(n)$
Finding for each array element the **nearest smaller element**
找出該陣列中兩個最小的元素。

i.e., the first smaller element that precedes the element in the array.
It is possible that no such element exists,
in which case the algorithm should report this.
Next we will see how the problem can be
efficiently solved using a stack structure.

解法:
用stack維護，從左到右去檢查元素，如果比top()大就`push()`，反則就`pop()`直到x比top大。

因為3,4都比1大，直接push上去。
![](https://i.imgur.com/pbmMgnG.png)

因為2比3,4都還小，將它們都pop出來。
![](https://i.imgur.com/Z9D2zmP.png)

時間複雜度由stack操作決定，push為 $O(1)$，但是pop就不一定了，但是每個元素只會add一次，而每個元素也是至多pop一次，因此全部的stack操作也是花$O(1) * n = O(n)$

The efficiency of the algorithm depends on
the total number of stack operations.
If the current element is larger than
the top element in the stack, it is directly
added to the stack, which is efficient.
However, sometimes the stack can contain several
larger elements and it takes time to remove them.
Still, each element is added \emph{exactly once} to the stack
and removed \emph{at most once} from the stack.
Thus, each element causes $O(1)$ stack operations,
and the algorithm works in $O(n)$ time.
## Sliding window minimum $O(n)$
假設windows size為4，找出全部sliding windows內的最小元素。

The sliding window minimum can be calculated
using a similar idea that we used to calculate
the nearest smaller elements.
We maintain a queue
where each element is larger than
the previous element,
and the **first element** always corresponds to the **minimum element inside the window**.

每次移動要加入新元素的時候，我們把queue中的後端元素移除，直到剩下的元素都比新元素小。
After each window move,
we remove elements from the end of the queue
until the last queue element
is smaller than the new window element,
or the queue becomes empty.

也要把前端元素移除，因為他已經不在windows內了。
We also remove the first queue element
if it is not inside the window anymore.
Finally, we add the new window element
to the end of the queue.

Suppose that the size of the sliding window is 4. At the ﬁrst window position, the smallest value is 1:
![](https://i.imgur.com/3pRqIWP.png)


Then the window moves one step right. The new element 3 is smaller than the elements 4 and 5 in the queue, so the elements 4 and 5 are removed from the queue and the element 3 is added to the queue. The smallest value is still 1.
![](https://i.imgur.com/38stY3D.png)


After this, the window moves again, and the smallest element 1 does not belong to the window anymore. Thus, it is removed from the queue and the smallest value is now 3. Also the new element 4 is added to the queue.
![](https://i.imgur.com/tw9dmXn.png)



The next new element 1 is smaller than all elements in the queue. Thus, all elements are removed from the queue and it will only contain the element 1:
![](https://i.imgur.com/MTlMvHV.png)


Finally the window reaches its last position. The element 2 is added to the queue, but the smallest value inside the window is still 1.
![](https://i.imgur.com/VZ4WLVA.png)



因為實際上，每個元素也只會加入deque一次，和移除一次，所以時間複雜度是 $O(n)$
Since each array element is added to the queue exactly once and
removed from the queue at most once,
the algorithm works in $O(n)$ time.

:::info
**deque**
- 與vector不同的是deque的動態數組首尾都開放，因此能夠在首尾進行快速地插入和刪除操作。
- deque的邏輯結構：
![](https://i.imgur.com/FdEvFvd.png)
:::

## Others
其實還有另外三個子類別做法，我們這邊範例主要都是用**聚合分析**:
- 聚合分析: 決定 n 個操作序列的耗費上界T(n)，然後計算平均耗費為 T(n) / n。
- 記帳方法: 確定每個操作的耗費，結合它的直接執行時間及它在對執行時中未來操作的影響。通常來說，許多短操作增量累加成「債」，而通過減少長操作的次數來「償還」。
- 勢能方法: 類似記帳方法，但通過預先儲蓄「勢能」而在需要的時候釋放
## Ref
[Algorithm - 平攤分析Amortized Analysis | Mr. Opengate](
https://mropengate.blogspot.com/2015/06/algorithm-amortized-analysis.html)

[Wiki 平攤分析](https://zh.wikipedia.org/wiki/%E5%B9%B3%E6%91%8A%E5%88%86%E6%9E%90)

[花花醬:LeetCode 239. Sliding Window Maximum ](https://www.youtube.com/watch?v=2SXqBsTR6a8)