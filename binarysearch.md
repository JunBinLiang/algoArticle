

# BinarySearch


### 文章中的例题链接 :  
 1. [First Bad Version](https://leetcode.com/problems/first-bad-version/) <br/>
 2. [Sqrt(x)](https://leetcode.com/problems/sqrtx/) <br/>
 3. [Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/) <br/>
 4. [Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) <br/>
 5. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) <br/>
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

* 我们可以发现此题的规律是 **G G G B B B B B B B ......**
* 使用二分法
* **mid = l + (r - l) / 2** 如果 **isBadVersion(mid)** 是false，我们使指针向左移动，否则使其向右移动



### Java (Credit to 66brother)

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

### C++ (Credit to 66brother)

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

### Python (Credit to liketheflower)

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

### Go (Credit to 66brother)

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


