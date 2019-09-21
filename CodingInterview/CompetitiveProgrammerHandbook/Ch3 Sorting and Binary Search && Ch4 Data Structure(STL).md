# [AIdrifter CS 浮生筆錄](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/rypeUnYSb?type=view#Competitive-Programmer%E2%80%99s-Handbook) Competitive Programming's Handbook : Ch3 Sorting and Binary Search && Ch4 Data Structure(STL)
# Sorting
## Sorting theory
### Basic  $O(n^2)$
- bubble sort
```C++
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n-1; j++) {
        if (array[j] > array[j+1]) {
            swap(array[j],array[j+1]);
        }
    }
}
```
- Inversion
    - 元素放在錯的位置 排序需要修正到對的位置
      The number of inversions indicates
      how much work is needed to sort the array.
      On the other hand, if the array elements are in the
      **reverse order**, the number of inversions is the largest possible:
    $1+2+\cdots+(n-1)=\frac{n(n-1)}{2} = O(n^2)$

- worst case  $O(n^2)$
    - :::info
        如果只能swap**連續元素** => worst case會到 $O(n^2)$
        :::
      Swapping a pair of consecutive elements that are
      in the wrong order removes exactly one inversion
      from the array. Hence, if a sorting algorithm can only
      swap **consecutive elements**, each swap removes
      at most one inversion, and the time complexity
      of the algorithm is at least $O(n^2)$.
### Advanced O(nlogn)
:::info
高等sort不需要限定只能交換連續元素 => worst case可以到 $O(n \log n)$
:::
- It is possible to sort an array efficiently
  in $O(n \log n)$ time using algorithms
  that are not limited to **swapping consecutive elements**.
  
- Sorting lower bound
    - 在compare and swap 概念下 有可能突破O(nlogn)速度嘛？
      Is it possible to sort an array faster than in O(nlogn) time?
      It turns out that this is not possible when we restrict ourselves to sorting algorithms that are based on comparing array elements.
    - **Decision tree**
      ![](https://i.imgur.com/FnekLN0.png)
      
      leaf為最後的排序全部結果 
      
      3!種可能 = {1,2,3} {1,3,2} {3,1,2}  {2,1,3} {2,3,1} {3,2,1}
      
      說明了對於基於比較的排序算法，我們**至少需要 Ω(nlogn)次比較**，不然最下面那支的葉子節點的結果我們就得不到。當然，最重要的的是，我們這裡得出的結論是對於**任意的排序算法**，任意的哦！
      Here ''$x<y?$'' means that some elements $x$ and $y$ are compared.
      If $x<y$, the process continues to the left, and otherwise to the right.
      The results of the process are the possible ways to sort the array, a total of $n!$ ways.
      For this reason, the height of the tree must be at least
        - $log_2(n!) = \log_2(1)+\log_2(2)+\cdots+\log_2(n).$
          => $log_2(n!) \ge (n/2) \cdot \log_2(n/2)$
        
   


- merge Sort
![](https://i.imgur.com/sOu6jAz.png)
![](https://i.imgur.com/2q6pqIb.png)


### linear Sorting O(n)
- counting sort
    -  用array index當作value ，用array 欄位紀錄出現次數
       **counting sort** that sorts an array in
       $O(n)$ time assuming that every element in the array
       is an integer between $0 \ldots c$ and $c=O(n)$.
       Counting sort is a very efficient algorithm
    - 只有在range C 不能很大的時候用 => array 元素不可以太多
      but it can only be used when the constant $c$
      is small enough, so that the array elements can
      be used as indices in the bookkeeping array.

## Sorting in C++
```cpp
/* increasing or ascending */
vector<int> v = {4,2,5,3,5,8,7};
int a[] = {4,2,5,3,5,8,7};

sort(v.begin(),v.end());
sort(a,a+7);

// desending
sort(v.rbegin(),v.rend());

// string
string s = "monkey";
sort(s.begin(), s.end());
```
### Comparsion operators
pairs，先比first元素，在比second 元素，如果想要自定義有兩種作法:
1. user-defined structure : 自己寫運算子重載
2. comparsion functions : 自己寫compare function

### User-defined structure
For example, the following struct **{P}** contains the x and y coordinates of a point.
The comparison operator **<** is defined so that
the points are sorted primarily by the x coordinate and secondarily by the y coordinate.
```cpp
struct P {
    int x, y;
    bool operator<(const P &p) {
        if (x != p.x) return x < p.x;
        else return y < p.y;
    }
};
```
### Comparison functions
- It is also possible to give an external
**comparison function** to the sort function
as a **callback function**.
```cpp
bool comp(string a, string b) {
    if (a.size() != b.size()) return a.size() < b.size();
    return a < b;
}
sort(v.begin(), v.end(), comp);
```

## Binary Search
### homemade
- 如果元素已經全部排序好，這是最好的方法。
  The algorithm halves the size of the region at each step,
so the time complexity is $O(\log n)$.

- 正常版
```cpp
int a = 0, b = n-1;
while (a <= b) {
    int k = (a+b)/2;
    if (array[k] == x) {
        // x found at index k
    }
    if (array[k] > x) b = k-1;
    else a = k+1;
}
```
- **控制 jump 長度b的大小**
    - **b** : jump的長度，會逐步遞減:
      At each step, the jump length will be halved:
 first $n/4$, then $n/8$, $n/16$, etc., until
finally the length is 1.
    - 如果[k+b] <= x 代表x在右邊  k=n/2 (now loop, condition success)
      如果[k+b] > x  代表x在左邊  k=n/4 (next loop)
```cpp
int k = 0;
for (int b = n/2; b >= 1; b /= 2) {
    while (k+b < n && array[k+b] <= x) k += b;
}
if (array[k] == x) {
    // x found at index k
}
```
### C++ functions
- 這些function都是base binary search去實作的
    - `lower_bound()` returns a pointer to the first array element whose value is **at least** x **(<)**.
    - `upper_bound()` returns a pointer to the first array element whose value is **larger than** x **(>)**.
    - equal_range returns both above pointers.

```cpp
//               0   1  2  3  4  5  6
vector<int> V = {10,10,10,20,20,20,30};

auto l = lower_bound(V.begin(), V.begin()+V.size(), 19) - V.begin();
auto u = upper_bound(V.begin(), V.end(), 20) - V.begin();

// l(3) u(6)
printf(" l(%lu) u(%lu)",l,u);
```

- binary Serach
    - `{10,10,10,20,20,20,30}`
    - 如果是x=20就會剛好找到，如果是x=19就找不到。 
```cpp
auto k = lower_bound(A, A+n, x) - A;
if (k<n && A[k] == x){
    // x found at index k
}
```

- 找出value為x的總共有幾個元素:
```cpp
auto l = lower_bound(A, A+n, x) - A;
auto u = upper_bound(A, A+n, x) - A;
cout<< b - a<< "\n";
```
```cpp
auto r = equal_range(A, A+n, x);
cout<< r.second - r.first <<"\n";
```

### Finding the smallest solution
We are given a function ${ok}(x)$
that returns ${true}$ if $x$ is a valid solution and ${false}$ otherwise.
In addition, we know that ${ok}(x)$ is ${false}$
when $x<k$ and ${true}$ when $x \ge k$.
The situation looks as follows:
這邊可以和前面的`while(k+b < n && A[k+b]<=x ) k+=b` 比較:
![](https://i.imgur.com/zOvKr4Z.png)

```cpp
int x = -1;
// The initial jump length z has to be large enough
// Time Complexity: o(log(z))
for (int b = z; b >= 1; b /= 2) {
    while (!ok(x+b)) x += b;
}
int k = x+1;
```
### Finding the maximum Value
可以幫我們找到最大y座標時候的x(接近)。
Binary search can also be used to find the maximum value for a function that is first increasing and then decreasing.
$f(x)<f(x+1)$ when $x<k$, and
$f(x)>f(x+1)$ when $x \ge k$.
![](https://i.imgur.com/sreAPlU.png)
```cpp
int f(int x){
    return -(2*x-1)*(x+2);
}
int x = -3;
for (int b = 5; b>=1; b/=2){
    while( f(x+b) < f(x+b+1))  x += b;
}
int k = x+1;
// k is -1
cout<<k<<"\n";

```
Note that unlike in the ordinary binary search,
here it is not allowed that **consecutive values** of the function are equal.

# Data Structures
## Dynamic arrays
### Vector
- C++ 赫赫有名的vector，支援以下操作:
    - init
    - push_back()
    - pop_back()
    - erase() // be careful ...
    - back()
    - insert()
```cpp
// size 10, initial value 0
vector<int> v(10);

// size 10, initial value 5
vector<int> v1(10, 5);

// A shorter way to iterate through a vector is as follows
for (auto x : v) {
    cout << x << "\n";
}

v.push_back(5); // 0 ... 0 5
v.push_back(2); // 0 ... 0 5 2
cout << v.back() << "\n"; // 2
v.pop_back();
cout << v.back() << "\n"; // 5
v.insert(v.begin()+9, 10); // 0 ... 10 5 
```
### String
- ${substr}(k,x)$ returns the substring that begins at position $k$ and has length $x$,
- $\texttt{find}(\texttt{t})$ finds the position of the first occurrence of a substring $t$.
```cpp
string a = "hatti";

// there is special syntax for strings
// that is not available in other data structures.
// Strings can be combined using the {+} symbol.
string b = a+a; 

cout << b << "\n"; // hattihatti
b[5] = 'v';
cout << b << "\n"; // hattivatti
string c = b.substr(3,4);
cout << c << "\n"; // tiva
```
## Set
`<set>`
- balanced binary tree : operations work $O(\log n)$
- maintains the order of the elements
- count只有**0 or 1**，sets is that all their elements aredistinct.
- set會用ordered去存，所以最後一個元素必為largest element。
    
`<unordered_set>`
 - hahsing : operations work $O(1)$ time on average.
 - unorder_set並沒有用orderd去存，所以速度快。

```cpp
set<int> s;
s.insert(3);
s.insert(2);
s.insert(5);
cout << s.count(3) << "\n"; // 1
cout << s.count(4) << "\n"; // 0
s.erase(3);
s.insert(4);
cout << s.count(3) << "\n"; // 0
cout << s.count(4) << "\n"; // 1
```
```cpp
set<int> s = {2,5,6,8};
cout << s.size() << "\n"; // 4
for (auto x : s) {
    cout << x << "\n";
}

```
- multiset and unordered_multiset
    - count 可以突破1
```cpp
multiset<int> s;
s.insert(5);
s.insert(5);
s.insert(5);
cout << s.count(5) << "\n"; // 3

// reombe only one instance 
s.erase(s.find(5));
cout << s.count(5) << "\n"; // 2

// remove all einstances of an element
s.erase(5);
cout << s.count(5) << "\n"; // 0
```


## Map
- A mapis a generalized array that consists of key-value-pairs.
- the keys in a map can be of any data type and they do nothave to be consecutive values

`<map>` : balanced binary tree: $O(\log n)$
`<unodered_map>`: hashing : $O(1)$


```cpp
// if the value of a key is requested but the map does not contain it, 
// the keyis automatically added to the map with a default value.
map<string,int> m;
cout << m["aybabtu"] << "\n"; // 0

// check key exist
if (m.count("aybabtu")) {
    // key exists
}

// traversal 
for (auto x : m) {
    cout << x.first << " " << x.second << "\n";
}
```

## Iterators and ranges
Note the asymmetry in the iterators:
$\texttt{s.begin()}$ points to an element in the data structure,
while $\texttt{s.end()}$ points outside the data structure.
Thus, the range defined by the iterators is $emph{half-open}$.
![](https://i.imgur.com/LBseQ41.png)
```cpp
sort(v.begin(), v.end());
reverse(v.begin(), v.end());
random_shuffle(v.begin(), v.end());

sort(a, a+n);
reverse(a, a+n);
random_shuffle(a, a+n);
```

```cpp
auto it = s.find(x);
if (it == s.end()) {
    // x is not found
}
```

- finds the element nearest to $x$
- assumes that the set is not empty, and goes through all possiblecases using an iteratorit.
```cpp
auto it = s.lower_bound(x);
if (it == s.begin()) {
    cout << *it << "\n";
} else if (it == s.end()) {
    it--;
    cout << *it << "\n";
} else {
    int a = *it; it--;
    int b = *it;
    if (x-b < a-x) cout << b << "\n";
    else cout << a << "\n";
}
```

## Bitset
- A bitsetis an array whose each value is either 0 or 1.
- require less memory than ordinaryarrays,  because  each  element  in  a  bitset  only  uses  one  bit  of  memory
```cpp
// count 1
bitset<10> s(string("0010011010"));
cout << s.count() << "\n"; // 4

// logic operation
bitset<10> a(string("0010110110"));
bitset<10> b(string("1011011000"));
cout << (a&b) << "\n"; // 0010010000
cout << (a|b) << "\n"; // 1011111110
cout << (a^b) << "\n"; // 1001101110

```

## Deque
- 比vecotr多了`push_front()` 和 `pop_front()` 操作。
```cpp
deque<int> d;
d.push_back(5); // [5]
d.push_back(2); // [5,2]
d.push_front(3); // [3,5,2]
d.pop_back(); // [3,5]
d.pop_front(); // [5]
```

## Stack
- adding an element to the top => `push()`
- removing an element from the top => `pop()`
- It is only possible to access the top element of a stack => `top()`
```cpp
stack<int> s;
s.push(3);
s.push(2);
s.push(5);
cout << s.top(); // 5
s.pop();
cout << s.top(); // 2
```
## Queue
- It is only possible to access the first and last element of a queue.
```cpp
queue<int> q;
q.push(3);
q.push(2);
q.push(5);
cout << q.front(); // 3
q.pop();
cout << q.front(); // 2
```

## Priority Queue
- A priority queuemaintains a set of elements. 
- **Heap** is faster than balanced binary tree(ordered set)
- By default, the elements in a C++ priority queue are sorted in **decreasing order**, and it is possible to find and remove the largest element in the queue.
```cpp
priority_queue<int> q;
q.push(3);
q.push(5);
q.push(7);
q.push(2);
cout << q.top() << "\n"; // 7
q.pop();
cout << q.top() << "\n"; // 5
q.pop();
q.push(6);
cout << q.top() << "\n"; // 6
q.pop();
```
- 如果要改成decreased order
    - `greather<int>`
```cpp
priority_queue<int,vector<int>,greater<int>> q;
```

## Policy-Based data structure
`<index_set>`
- logarithmatic time
- 可以直接透過額外的inedx去access key。
- The speciality of this set is that we have access to the indices that the elementswould have in a sorted array.
```cpp
#include <ext/pb_ds/assoc_container.hpp>
using namespace __gnu_pbds; 

indexed_set s;
s.insert(2);
s.insert(3);
s.insert(7);
s.insert(9);

//  index: 0   1   2   3
//  key  : 2   3   7   9 
//  val  : 0   0   0   0
auto x = s.find_by_order(2);
cout << *x << "\n"; // 7

cout << s.order_of_key(7) << "\n"; // 2


// If the element does not appear in the set,
// we get the position that the element would have in the set:
cout << s.order_of_key(6) << "\n"; // 2
cout << s.order_of_key(8) << "\n"; // 3
```

## Comparsion Sorting
Let us consider a problem where
we are given two lists $A$ and $B$
that both contain $n$ elements.
Our task is to calculate the number of elements
that belong to both of the lists.
For example, for the lists
$A = [5,2,8,9]$ $B = [3,2,9,5]$
the answer is 3 because the numbers **2, 5 and 9** belong to both of the lists.

${Algorithm 1}$

We construct a set of the elements that appear in $A$,
and after this, we iterate through the elements
of $B$ and check for each elements if it also belongs to $A$.
This is efficient because the elements of $A$ are in a set.
Using the $set$ structure,
the time complexity of the algorithm is $O(nlog n)$.
```cpp
int count = 0;
set<int> s;
for(auto a:A)   // n * log(n)
    s.insert(a);

for(auto b:B)   //  n * O(1) ??
   if(s.count(b)) count++;

return count;
```

${Algorithm 2}$

It is not necessary to maintain an ordered set,
so instead of the ${set}$ structure
we can also use the ${unordered\_set}$ structure.
This is an easy way to make the algorithm
more efficient, because we only have to change
the underlying data structure.
The time complexity of the new algorithm is $O(n)$.
```cpp
int count = 0;
unordered_set<int> s;
for(auto a:A)   // n * O(1)
    s.insert(a);

for(auto b:B)   //  n * O(1)
   if(s.count(b)) count++;

return count;
```

${Algorithm 3}$

Instead of data structures, we can use sorting.
First, we sort both lists $A$ and $B$.
After this, we iterate through both the lists
at the same time and find the common elements.
The time complexity of sorting is $O(n \log n)$,
and the rest of the algorithm works in $O(n)$ time,
so the total time complexity is $O(n \log n)$.
```cpp
sort(A.begin(), A.end());   // O(nlogn)
sort(B.begin(), B.end());   // O(nlogn)
int count;

auto a_itr = A.begin();
auto b_itr = B.begin();

 // O(n)
while(a_itr != A.end() || b_itr !=  B.end()){
    if(*a_itr > *b_itr) b_itr++; 
    if(*a_itr < *b_itr) a_itr++;
    if(*a_itr == *b_itr) count++;
    return count;
}

```
- 因為unodered_set是用hash table去實作，所以algorith2一定比algotirhm1快，但是涉及到set的操作，即便時間複雜度都是$O(nlogn)$，sort還是快上許多，因為他操作的資料結構沒這麼複雜，不需要去維護balanced binary tree(unordered_set)， 或是hash table(set)。

![](https://i.imgur.com/tlr4G3d.png)


# Reference
Lower Bound: 
http://www.zitaoliu.com/cs/algorithm/2011/04/20/introduction-to-algorithm-problem-complexity-and-adversarial-lower-bound/

C++ Standard Libraries(STL): 
https://hackmd.io/ww2WwMVDRZWo9CMm2FgtYA?view
