

# BinarySearch I


### 文章中的例题链接 :  
 1. [First Bad Version](https://leetcode.com/problems/first-bad-version/) <br/>
 2. [Sqrt(x)](https://leetcode.com/problems/sqrtx/) <br/>
 3. [Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) <br/>
 4. [Maximum White Tiles Covered by a Carpet](https://leetcode.com/problems/maximum-white-tiles-covered-by-a-carpet/) <br/>
<br/><br/><br/>


 ###  LC278 First Bad Version

 ### Statements :
> You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

> Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.
 
> You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

### Example :

**Input** : n = 5, bad = 4

**Output** : 4

call isBadVersion(3) -> false

call isBadVersion(4) -> true

call isBadVersion(5) -> ture



### **Constraints:**

* `1 <= bad <= n <= 2^31 - 1`



### Explanation :

* 我们可以发现此题的规律 **G G G B B B B B B B ......**
* 使用二分法
* **mid = l + (r - l) / 2** 如果 **isBadVersion(mid)** 是false，我们使指针向左移动，否则使其向右移动



### Java 

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        int res = - 1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            if(isBadVersion(mid)) {
                res = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return res;
    }
}
```

### C++ 

```cpp
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        int res = -1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            if(isBadVersion(mid)) {
                res = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return res;
    }
};
```

### Python 

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return an integer
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        l, r = 1, n
        while l < r:
            mid = (l + r) // 2
            if isBadVersion(mid):
                r = mid
            else:
                l = mid + 1;
        return l
```

### Go 

```go
/** 
 * Forward declaration of isBadVersion API.
 * @param   version   your guess about first bad version
 * @return 	 	      true if current version is bad 
 *			          false if current version is good
 * func isBadVersion(version int) bool;
 */

func firstBadVersion(n int) int {
    var l, r int = 1, n
    res := -1
    for l <= r {
        mid := l + (r - l) / 2
        if isBadVersion(mid) {
            res = mid
            r = mid - 1
        } else {
            l = mid + 1
        }       
    }
    return res
}
```

### Complexity :

* **Time** : O (log(n)) 
* **Space** : O(1) 

<br/><br/><br/>









 ###  LC69 Sqrt(x)

 ### Statements :
> Given a non-negative integer x, compute and return the square root of x.

> Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

> Note: You are not allowed to use any built-in exponent function or operator, such as pow(x, 0.5) or x ** 0.5.

### Example :

**Input** : x = 4

**Output** : 2

### **Constraints:**

* `0 <= x <= 2^31 - 1`



### Explanation :

* 定义 f(mid) : 如果 mid * mid <= x, 返回true 
* 我们可以发现此题的规律与上题一样 **G G G B B B B B B B ......**
* 使用二分法
* **mid = l + (r - l) / 2** 如果 **f(mid)** 是false，我们使指针向左移动，否则使其向右移动



### Java 

```java
class Solution {
    public int mySqrt(int x) {
        long l = 0, r = x;
        long res = -1;
        while(l <= r) {
            long mid = l + (r - l) / 2;
            long sqrt = mid * mid;
            if(sqrt <= x) {
                res = mid;
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return (int)(res);
    }
}
```

### C++ 

```cpp
class Solution {
public:
    int mySqrt(int x) {
        long long l = 0, r = x;
        long long res = -1;
        while(l <= r) {
            long long mid = l + (r - l) / 2;
            if(mid * mid <= x) {
                res = mid;
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return (int)(res);
    }
};
```

### Python 

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        l = 0
        r = x
        res = 0
        while l <= r:
            mid = l + (r - l) // 2
            if mid * mid <= x:
                res = mid
                l = mid + 1
            else:
                r = mid - 1
        return res
```

### Go 

```go
func mySqrt(x int) int {
    var l, r, res int64 = 0, int64(x), 0
    for l <= r {
        mid := l + (r - l) / 2
        if mid * mid <= int64(x) {
            res = mid
            l = mid + 1
        } else {
            r = mid - 1
        }
    }
    return int(res)
}
```

### Complexity :

* **Time** : O (log(n)) 
* **Space** : O(1) 

<br/><br/><br/>



 ###  LC668 Kth Smallest Number in Multiplication Table

 ### Statements :
> Nearly everyone has used the Multiplication Table. The multiplication table of size m x n is an integer matrix mat where mat[i][j] == i * j (1-indexed).

> Given three integers m, n, and k, return the kth smallest element in the m x n multiplication table.

### Example :

![c40573a80e33355d29df37eaec57c45](https://user-images.githubusercontent.com/45537132/170187718-10d57171-d2a9-41a3-8dbf-1ba2e6a4755b.png)


**Input** : m = 3, n = 3, k = 5

**Output** : 3

**Explanation** : The 5th smallest number is 3.

### **Constraints:**

* `1 <= m, n <= 3 * 104`
* `1 <= k <= m * n`

<br/><br/>

### Explanation :

* 二分搜索答案 mid
* 看看有多少个数字， 记录为cnt
* 如果cnt >= k， 那这个是一个可能的答案，记录一下并且指针往左移动。否则，指针往右移动



### Java 

```java
class Solution {
    public int findKthNumber(int n, int m, int k) {
        int l = 1, r = n * m;
        int res = -1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            int cnt = 0;
            for(int i = 1; i <= n; i++) {
                cnt += Math.min(mid / i, m);
            }
            if(cnt >= k) {
                res = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return res;
    }
}
```

### C++ 

```cpp
class Solution {
public:
    int findKthNumber(int n, int m, int k) {
        int l = 1, r = n * m;
        int res = -1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            int cnt = 0;
            for(int i = 1; i <= n; i++) {
                cnt += min(mid / i, m);
            }
            if(cnt >= k) {
                res = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return res;
    }
};
```

### Python 

```python
class Solution:
    def findKthNumber(self, n: int, m: int, k: int) -> int:
        res = 0
        l = 1
        r = n * m
        while l <= r:
            mid = l + (r - l) // 2
            cnt = 0
            for i in range(1, n + 1):
                cnt += min(mid // i, m)
            if cnt >= k:
                res = mid
                r = mid - 1
            else:
                l = mid + 1
        return res
```

### Go 

```go
func min(a int, b int) int {
    if a < b {
        return a
    }
    return b
}

func findKthNumber(n int, m int, k int) int {
    var l, r int = 1, n * m
    res := -1
    for l <= r {
        mid := l + (r - l) / 2
        cnt := 0
        for i := 1; i <= n; i++ {
            cnt += min((mid / i), m)
        } 
        if cnt >= k {
            res = mid
            r = mid - 1
        } else{
            l = mid + 1
        }
    }
    
    return res
}
```

### Complexity :

* **Time** :  O(n log(n * m))
* **Space** : O(1) 

<br/><br/><br/>

 ###  LC2271 Maximum White Tiles Covered by a Carpet
 
  ### Statements :
> You are given a 2D integer array tiles where tiles[i] = [li, ri] represents that every tile j in the range li <= j <= ri is colored white.
> 
> You are also given an integer carpetLen, the length of a single carpet that can be placed anywhere.
> 
> Return the maximum number of white tiles that can be covered by the carpet.


### Example :


**Input** : tiles = [[1,5],[10,11],[12,18],[20,25],[30,32]], carpetLen = 10

**Output** : 9

**Explanation** : Place the carpet starting on tile 10. <br/>
It covers 9 white tiles, so we return 9.<br/>
Note that there may be other places where the carpet covers 9 white tiles.<br/>
It can be shown that the carpet cannot cover more than 9 white tiles.<br/>

<br/><br/>

### Explanation :
* 先对所有区间进行一个排序
* 枚举每个点作为起始的点，看看他最远能够cover 到那个点 (使用二分)
* 找到最远的点后，看看从起始点到最远点的实际距离 (去掉空的) 是多少 (这里可以使用前缀和速求)


### Java 

```java
class Solution {
    public int maximumWhiteTiles(int[][] a, int len) {
        Arrays.sort(a, (x, y) -> {
            return x[0] - y[0];
        });
        
        int pre[] = new int[a.length];
        int sum = 0, res = 0;
        
        for(int i = 0; i < a.length; i++) {
            sum += (a[i][1] - a[i][0] + 1);
            pre[i] = sum;
        }
        
        for(int i = 0; i < a.length; i++) {
            int l = i, r = a.length -1;
            int pos = -1;
            while(l <= r) {
                int mid = l + (r - l) / 2;
                if(a[mid][0] <= a[i][0] + len - 1) {
                    pos = mid;
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
            
            if(a[i][0] + len - 1 >= a[pos][1]) {
                res = Math.max(res, get(pre, i, pos)); 
            } else {
                res = Math.max(res, get(pre, i, pos - 1) + (a[i][0] + len - 1 - a[pos][0] + 1)); 
            }
        }
        
        return res;
    }
    
    public int get(int a[], int l, int r) {
        if(l > r) return 0;
        if(l == 0) return a[r];
        return a[r] - a[l - 1];
    }
}
```

### C++ 

```cpp
bool COMP(const vector<int>& a, const vector<int>& b) {
    return a[0] < b[0];
}

int get(vector<int>& a, int l, int r) {
    if(l > r) return 0;
    if(l == 0) return a[r];
    return a[r] - a[l - 1];
}

class Solution {
public:
    int maximumWhiteTiles(vector<vector<int>>& a, int len) {
        sort(a.begin(), a.end(), COMP);
        vector<int> pre;
        int sum = 0, res = 0;
        for(int i = 0; i < a.size(); i++) {
            sum += (a[i][1] - a[i][0] + 1);
            pre.push_back(sum);
        }
        
        
        for(int i = 0; i < a.size(); i++) {
            int l = i, r = a.size() -1;
            int pos = -1;
            while(l <= r) {
                int mid = l + (r - l) / 2;
                if(a[mid][0] <= a[i][0] + len - 1) {
                    pos = mid;
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
            
            if(a[i][0] + len - 1 >= a[pos][1]) {
                res = max(res, get(pre, i, pos)); 
            } else {
                res = max(res, get(pre, i, pos - 1) + (a[i][0] + len - 1 - a[pos][0] + 1)); 
            }
        }
        return res;
    }
};
```

### Python 

```python
class Solution:
    def maximumWhiteTiles(self, a: List[List[int]], le: int) -> int:
        a = sorted(a, key = itemgetter(0))
        sum1 = 0
        res = 0
        pre = [0] * len(a)
        for i in range(len(a)):
            sum1 += (a[i][1] - a[i][0] + 1)
            pre[i] = sum1
            
        for i in range(len(a)):
            l = i
            r = len(a) - 1
            pos = -1
            while l <= r:
                mid = l + (r - l) // 2
                if a[mid][0] <= a[i][0] + le - 1:
                    pos = mid;
                    l = mid + 1;
                else:
                    r = mid - 1
                    
            if a[i][0] + le - 1 >= a[pos][1]:
                res = max(res, self.cal(pre, i, pos))
            else:
                res = max(res, (a[i][0] + le - 1 - a[pos][0] + 1) + self.cal(pre, i, pos - 1))
                
        return res
    
        
    def cal(self, a, l, r) -> int:
        if l > r:
            return 0
        if l == 0:
            return a[r]
        return a[r] - a[l - 1]
```

### Go (双指针写法)

```go
func max(a int, b int) int {
    if a > b {
        return a
    }
    return b
}

func maximumWhiteTiles(a [][]int, l int) int {
    sort.SliceStable(a, func(i, j int) bool {
        return a[i][0] < a[j][0]
    })
    
    res := 0
    var sum, j int = 0, 0
    for i := 0; i < len(a); i++ {
        to := a[i][0] + l - 1
        for j < len(a) && to >= a[j][0] {
            sum += (a[j][1] - a[j][0] + 1)
            j++
        }
        if to < a[j - 1][1] {
            res = max(res, sum - (a[j - 1][1] - a[j - 1][0] + 1) + (to - a[j - 1][0] + 1))
        } else {
            res = max(res, sum)
        }
        sum -= (a[i][1] - a[i][0] + 1)
    }
    return res
}
```

### Complexity :

* **Time** :  O(n log(n))
* **Space** : O(n) 

<br/><br/><br/>

