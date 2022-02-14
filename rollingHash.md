# 字符串神器 ： 滚动哈希 (Rolling Hash) 
<br/>
今天给大家介绍的是**字串符(string)** 算法中特别好用的一种 : **滚动哈希 (Rolling Hash)** <br/>
我们将会聊到滚动哈希的基本概念，用法，以及会通过几道题的实例去巩固对该算法的了解<br/>
**:bulb:注** ： 作者会试着用最简单移动的言语去使读者明白，不会包含太复杂的证明与理论。 练习题的题解会使用4种不同的语言进行解答 (Java, C++, Go, Python)<br/><br/>

### 文章中的例题链接 :

 1. [Leetcode 1392. Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/)
 2. [Leetcode 2156. Find Substring With Given Hash Value](https://leetcode.com/problems/find-substring-with-given-hash-value/)
 <br/><br/> <br/><br/>


## 什么是滚动哈希 (Rolling Hash) :
简单一点就是把一个 **字串符(string)** 通过一个**hash function**给 **hash** 成一个值 <br/><br/>


## 如何hash 一个string ?
1. 我们用 **s** 来表示当前的string
2. 我们还会定义两个值 **base** 和 **mod**. **base** 可以选择任何数字。 **mod** 要选择一个尽量比较大并且是一个**prime number**，例题我们会用1000000007 作为我们的 **mod**
3. **定义** :  **hash(s)** = s[0] * base ^ (n - 1) + s[1] * base ^ (n - 2) + s[2] * base ^ (n - 3) ... s[n - 1] * base ^ (0) 
4. 因为数字特别大的关系，我们需要取模 
<br/><br/>



## 滚动哈希 (Rolling Hash)的用处/好处是什么？
1. 当我们定义好以上的 **hash(s)** 后，我们可以使用 **O(1)** 的时间复杂度去计算出任意 s的 **子字符串 (substring)** 的hash值
2. 定义s的子字符串 ： **s[i : j]**  (0 <= i, j < len(s))
3. 有时候我们需要比较substring 是否是一样的，如果单纯的使用一些语言的substring()函数，一般都是**O(j - i + 1) => O(n)** 的复杂度，这种时候如果我们直接去比较substring的哈希值，在复杂度上就会快很多 
4. **Conflict** : 因为我们对hash 函数要取模的关系，这样有可能会导致两个不一样的string 但却一样的hash值。对此我们有两种解决方案
	1. 如果两个string的hash 不一样，我们可以直接无视，他们肯定是不一样的string。 但如果是一样的话，我们要用substring() 函数再进行一次额外check
	2. 我们可以使用**double hash**，利用多一个hash 函数(**使用不同的base 和 mod**)可以减少 **conflict** 的概率。 但是**conflict**依然还是有几率会发生

<br/><br/>

## 如何计算子字串符(substring)的hash值？
1. 我们首先会define 一个数组 **h[]**
2. 假设 **s = "bacde"**， **h** 如下：
	1. h[0] = 'b' 	
	2. h[1] = 'b' * base ^ 1 + 'a'
	3. h[2] = 'b' * base ^ 2 + 'a' ^ base ^ 1 + 'c'
	4. h[3] = 'b' * base ^ 3 + 'a' ^ base ^ 2 + 'c' * base ^ 1 + 'd'
	5. h[4] = 'b' * base ^ 4 + 'a' ^ base ^ 3 + 'c' * base ^ 2 + 'd' * base ^ 1 + 'e'
3. 假设我们想计算 **hash(s[2 : 3] == "cd")**。 如果使用我们对于hash的定义，我们知道 **hash("cd") = 'c' * base ^ 1 + 'd'**
4. 对于计算 **hash(s[l : r])**， 我们可以用公式 ： **hash[r] - hash[l - 1] * base ^ (r - l + 1)**
5. **例子** ： **hash(s[2 : 3])** = hash[3] - hash[1] * base ^ 2 = ('b' * base ^ 3 + 'a' ^ base ^ 2 + 'c' * base ^ 1 + 'd') - (('b' * base ^ 1 + 'a') * base ^ 2) = **'c' * base ^ 1 + 'd'**
<br/><br/>


## 核心代码
**:bulb:注** ： 这里只使用java，其它语言以及实际的详细操作可参考以下的例题！！ <br/><br/>

* 计算**h[]** 数组
```
		//定义mod 和 base
        int mod = 1000000007;
        int base = 26;
        
        //使用pow数组帮我们记录 base ^ i, pow[i] = base ^ i
        
        long pow[] = new long[s.length()];
        pow[0] = 1;
        for(int i = 1; i < pow.length; i++) {
            pow[i] = pow[i - 1] * base;
            pow[i] %= mod;
        }
        

        //hash[0] = s.charAt(0)
        //hash[1] = h[0] * base + s.charAt(1)
        //hash[2] = hash[1] * base + s.charAt(2)
        
        long hash[] = new long[s.length()];
        long h = 0;
        for(int i = 0; i < s.length(); i++) {
            h = h * base + (s.charAt(i));
            h %= mod;
            hash[i] = h;
        }
```
<br/>

* 计算**hash(s[i : j])**
```
	//s[l : r]
    //res = h[r] - h[l - 1] * base^ (r - (l - 1))
    public long gethash(long hash[],long pow[], int left,int right) {
        if(left == 0) {
            return hash[right];
        }
        long res = (hash[right] - (hash[left - 1] * pow[right - left + 1] % mod) + mod) % mod;
        if(res < 0)res += mod;
        return res % mod;
    } 
```



	


 <br/><br/><br/><br/> <br/><br/>
## Leetcode 1392. Longest Happy Prefix
### Statements :

> A string is called a **happy prefix** if is a **non-empty** prefix which is also a suffix (excluding itself).
>
> Given a string `s`, return _the **longest happy prefix** of_ `s`. Return an empty string `""` if no such prefix exists.



### Example :

**Input** : s = "level"

**Output** : "l"

**Explanation** : s contains 4 prefix excluding itself ("l", "le", "lev", "leve"), and suffix ("l", "el", "vel", "evel"). The largest prefix which is also suffix is given by "l".



### **Constraints:**

* `1 <= s.length <= 10^5`
* `s` contains only lowercase English letters.

### **Explanation:**
* 对于每一个prefix **s[0 : i]** (0 <= i < n - 1), 我们可以找出其对应的postfix **s[n - i - 1: n - 1]**
* 我们可以直接对比prefix 和 postfix 的 hash 值
* 找出最长的prefix 的切点，记录为 **cut**
* 返回 s[0 : cut]


### Java

```
class Solution {
    int mod = 1000000007;
    int base = 26;
    public String longestPrefix(String s) {
        long pow[] = new long[s.length()];
        long hash[] = new long[s.length()];
        
        pow[0] = 1;
        for(int i = 1; i < pow.length; i++) {
            pow[i] = pow[i - 1] * base;
            pow[i] %= mod;
        }
        
        long h = 0;
        for(int i = 0; i < s.length(); i++) {
            h = h * base + (s.charAt(i));
            h %= mod;
            hash[i] = h;
        }

        int cut = -1;
        for(int i = 0; i < s.length() - 1; i++) {
            long h1 = gethash(hash, pow, 0, i);
            long h2 = gethash(hash, pow, s.length() - (i + 1), s.length() - 1);
            if(h1 == h2) {
                cut = i;
            }
        }
        return cut == -1 ? "" : s.substring(0, cut + 1);
    }
    
    //s[l : r]
    public long gethash(long hash[],long pow[], int left,int right) {
        if(left == 0) {
            return hash[right];
        }
        long res = (hash[right] - (hash[left - 1] * pow[right - left + 1] % mod) + mod) % mod;
        if(res < 0)res += mod;
        return res % mod;
    } 
}
```
### C++

```cpp
class Solution {
public:
    int mod = 1000000007;
    int base = 26;
    string longestPrefix(string s) {
        int n = s.size();
        vector<long long> pow(n);
        vector<long long> hash(n);
        
        pow[0] = 1;
        for(int i = 1; i < n; i++) {
            pow[i] = pow[i - 1] * base;
            pow[i] %= mod;
        }
        
        long long h = 0;
        for(int i = 0; i < s.size(); i++) {
            h = h * base + s[i];
            h %= mod;
            hash[i] = h;
        }
        
        int cut = -1;
        for(int i = 0; i < s.size() - 1; i++) {
            long long h1 = gethash(hash, pow, 0, i);
            long long h2 = gethash(hash, pow, s.size() - (i + 1), s.size() - 1);
            if(h1 == h2) {
                cut = i;
            }
        }
        return cut == -1 ? "" : s.substr(0, cut + 1);
    }
    
    long long gethash(vector<long long>& hash,vector<long long>& pow, int left,int right) {
        if(left == 0) {
            return hash[right];
        }
        long long res = (hash[right] - (hash[left - 1] * pow[right - left + 1]% mod) + mod) % mod;
        if(res < 0)res += mod;
        return res % mod;
    } 
};
```

### Go

```go
const mod, base int64 = 1000000007, 26

func getHash(hash []int64, pow[] int64, left int, right int) int64 {
    if left == 0 {
        return hash[right]
    }
    res := (hash[right] - ((hash[left - 1] * pow[right - left + 1])) % mod)
    res += mod
    return res % mod
}

func longestPrefix(s string) string {
    n := len(s)
    pow := make([]int64, n + 1)
    hash := make([]int64, n)  
   
    pow[0] = 1
    for i := 1; i < len(pow); i++ {
        pow[i] = pow[i - 1] * base
        pow[i] %= mod
    }
    
    h := int64(0)
    for i := 0; i < n; i++ {
        h = h * base + (int64)(s[i])
        h %= mod
        hash[i] = h
    }
    
    cut := -1
    for i := 0; i < n - 1; i++ {
        h1 := getHash(hash, pow, 0, i)
        h2 := getHash(hash, pow, n - i - 1, n - 1)
        if h1 == h2 {
            cut = i
        }
    }
    
    if cut == -1 {
        return ""
    }
    return s[0 : cut + 1]
}
```
### Python

```python
class Solution:
    def longestPrefix(self, s: str) -> str:
        base = 26
        mod = 1000000007
        n = len(s)
        pow = [0] * n
        hash = [0] * n
        
        pow[0] = 1
        for i in range(1, n):
            pow[i] = pow[i - 1] * base
            pow[i] %= mod
            
        h = 0
        for i in range(n):
            h = h * base + ord(s[i])
            h %= mod
            hash[i] = h
            
        cut = -1
        for i in range(n - 1):
            h1 = self.get(hash, pow, 0, i)
            h2 = self.get(hash, pow, n - i - 1, n - 1)
            if h1 == h2:
                cut = i
        if cut == -1:
            return ""
        else:
            return s[0 : cut + 1]
        
    def get(self, hash, pow, left, right) -> str:
        mod = 1000000007
        if left == 0:
            return hash[right]
        res = (hash[right] - (hash[left - 1] * pow[right - left + 1] % mod) + mod) % mod;
        if res < 0:
            res += mod
        return res % mod

        
```
### Complexity :

* **Time** : O(n)&#x20;
* **Space** : O(n)&#x20;

<br/><br/>

## Leetcode 2156. Find Substring With Given Hash Value
### Statements :

> The hash of a **0-indexed** string `s` of length `k`, given integers `p` and `m`, is computed using the following function:
>
> * `hash(s, p, m) = (val(s[0]) * p0 + val(s[1]) * p1 + ... + val(s[k-1]) * pk-1) mod m`.
>
> Where `val(s[i])` represents the index of `s[i]` in the alphabet from `val('a') = 1` to `val('z') = 26`.
>
> You are given a string `s` and the integers `power`, `modulo`, `k`, and `hashValue.` Return `sub`, _the **first**  **substring** of_ `s` _of length_ `k` _such that_ `hash(sub, power, modulo) == hashValue`.
>
> The test cases will be generated such that an answer always **exists**.
>
> A **substring** is a contiguous non-empty sequence of characters within a string.



### Example :

**Input** : s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0

**Output** : "ee"

**Explanation** : The hash of "ee" can be computed to be hash("ee", 7, 20) = (5 \* 1 + 5 \* 7) mod 20 = 40 mod 20 = 0. "ee" is the first substring of length 2 with hashValue 0. Hence, we return "ee".



### **Constraints:**

* `1 <= k <= s.length <= 2 * 10^4`
* `1 <= power, modulo <= 10^9`
* `0 <= hashValue < modulo`
* `s` consists of lowercase English letters only.
* The test cases are generated such that an answer always **exists**.



### Explanation :

* 对于 **hash(s, p, m)** ，它跟我们以上定义的 **hash(s)** 函数非常相似 
* 但是仔细发现这里的hash 跟我们以上定义的是相反的，所以当我们简历 h[] 数组的时候，只要按 string s 的倒叙来定义即可



### Java 

```java
class Solution {
    public String subStrHash(String s, int base, int mod, int k, int hashValue) {
        int n = s.length();

        long pow[] = new long[n + 1];
        long hash[] = new long[n];
        pow[0] = 1;
        for(int i = 1; i < pow.length; i++) {
            pow[i] = pow[i - 1] * base;
            pow[i] %= mod;
        }

        long h = 0;
        for(int i = 0; i < s.length(); i++) {
            h = h * base + (s.charAt(n - i - 1) - 'a' + 1);
            h %= mod;
            hash[i] = h;
        }
        
        int index = -1;
        for(int i = 0; i < n; i++) {
            if(i + 1 < k) continue;
            long subHash = gethash(hash, pow, i - k + 1, i, mod);
            if(subHash == hashValue) {
                index = n - i - 1;
            }
        }
        return s.substring(index, index + k);
    }
    
    public long gethash(long hash[],long pow[], int left,int right, int mod) {
        if(left == 0) {
            return hash[right];
        }
        long res = (hash[right] - (hash[left - 1] * pow[right - left + 1] % mod) + mod) % mod;
        if(res < 0)res += mod;
        return res % mod;
    } 
}
```

### C++ 
```cpp
class Solution {
public:
    string subStrHash(string s, int base, int mod, int k, int hashValue) {
        int n = s.size();
        vector<long long> pow(n);
        vector<long long> hash(n);
        
        pow[0] = 1;
        for(int i = 1; i < n; i++) {
            pow[i] = pow[i - 1] * base;
            pow[i] %= mod;
        }
        
        long long h = 0;
        for(int i = 0; i < s.size(); i++) {
            h = h * base + (s[n - i - 1] - 'a' + 1);
            h %= mod;
            hash[i] = h;
        }
        
        int index = -1;
        for(int i = 0; i < n; i++) {
            if(i + 1 < k) continue;
            long long subHash = gethash(hash, pow, i - k + 1, i, mod);
            if(subHash == hashValue) {
                index = n - i - 1;
            }
        }
        return s.substr(index, k);
    }
    
    long long gethash(vector<long long>& hash,vector<long long>& pow, int left,int right, int mod) {
        if(left == 0) {
            return hash[right];
        }
        long long res = (hash[right] - (hash[left - 1] * pow[right - left + 1]% mod) + mod) % mod;
        if(res < 0)res += mod;
        return res % mod;
    }
};
```

### Go 
```go
func getHash(hash []int64, pow[] int64, left int, right int, mod int64) int64 {
    if left == 0 {
        return hash[right]
    }
    res := (hash[right] - ((hash[left - 1] * pow[right - left + 1])) % mod)
    res += mod
    return res % mod
}

func subStrHash(s string, power int, modulo int, k int, hashValue int) string {
    base := int64(power)
    mod := int64(modulo)
    
    n := len(s)
    pow := make([]int64, n + 1)
    hash := make([]int64, n)  
   
    pow[0] = 1
    for i := 1; i < len(pow); i++ {
        pow[i] = pow[i - 1] * base
        pow[i] %= mod
    }
    
    h := int64(0)
    for i := 0; i < n; i++ {
        h = h * base + (int64)(s[n - i - 1] - 'a' + 1)
        h %= mod
        hash[i] = h
    }
    
    index := -1
    for i := 0; i < n; i++ {
        if i + 1 < k {
            continue
        }
        subHash := getHash(hash, pow, i - k + 1, i, mod)
        if subHash == int64(hashValue) {
            index = n - i - 1
        }
    }
    
    return s[index : index + k]
}
```

### Python

```python
class Solution:
    def subStrHash(self, s: str, base: int, mod: int, k: int, hashValue: int) -> str:
        n = len(s)
        p = [0] * n
        ha = [0] * n
        
        p[0] = 1
        for i in range(1, n):
            p[i] = p[i - 1] * base
            p[i] %= mod
        
        h = 0
        for i in range(n):
            h = h * base + int(ord(s[n - i - 1])) - 97 + 1
            h %= mod
            ha[i] = h
        
        index = -1
        for i in range(n):
            if i + 1 < k:
                continue
            subHash = self.gethash(ha, p, i - k + 1, i, mod)
            if subHash == hashValue:
                index = n - i - 1
        return s[index : index + k]
    
    def gethash(self, ha, p, l, r, mod):
        if l == 0:
            return ha[r]
        res = ha[r] - (ha[l - 1] * p[r - l + 1]) % mod
        res += mod
        return res % mod
```

### Complexity :

* **Time** : O(n)&#x20;
* **Space** : O(n)&#x20;
