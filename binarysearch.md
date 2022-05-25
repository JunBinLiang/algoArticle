

# BinarySearch


### 文章中的例题链接 :  
 1. [First Bad Version](https://leetcode.com/problems/first-bad-version/) <br/>
 2. [Sqrt(x)](https://leetcode.com/problems/sqrtx/) <br/>
 3. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) <br/> 
 4. [Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/) <br/>
 5. [Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) <br/>
 6. [Maximum White Tiles Covered by a Carpet](https://leetcode.com/problems/maximum-white-tiles-covered-by-a-carpet/) <br/>
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
*



### Java 

```java

```

### C++ 

```cpp

```

### Python 

```python

```

### Go 

```go

```

### Complexity :

* **Time** : 
* **Space** : 

<br/><br/><br/>
