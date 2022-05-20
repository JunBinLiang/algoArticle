

# Tree  I
学习 **Tree** 的几种经典用法  (讲章1）

 - Tree Diameter
 - Tree Coordinate
 - Post Order Traversal
 - Tree Interval DP

  
### 文章中的例题链接 :  

  Tree Diameter<br/>
  
 [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree) <br/>
 [Longest Path With Different Adjacent Characters](https://leetcode.com/problems/longest-path-with-different-adjacent-characters)<br/>
<br/>

Tree Coordinate<br/>
[  Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value)<br/>
[ Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree)<br/>
<br/>

Post Order Traversal<br/>
[ Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum)<br/>
[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)<br/>
[Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree)<br/>


<br/>

Tree Interval DP<br/>
[Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree)<br/>
[Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees)
  <br/>


  <br/>  <br/><br/>  <br/>
  
##  Tree Diameter

 - 树的直径 ：设**dis(u, v)** 是 vertex u 到 vertex v 的最短距离。 **Tree Diameter = max(dis(u, v))**
 - 如何求树的直径？使用  [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree) 作为模板
 - 假设路径经过 vertex u, 路径长度 = 1 + dis(u, v1) + dis(u, v2)。这里我们只需要使 dis(u, v1), dis(u, v2) 最大化即可
 
<br/>
 
### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int res = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        dfs(root);
        return res - 1;
    }
    
    public int dfs(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int l = dfs(root.left);
        int r = dfs(root.right);
        res = Math.max(res, l + r + 1);
        return Math.max(l, r) + 1;
    }
}
```


<br/>
### C++

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int res = 0;
    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return res - 1;
    }
    
    int dfs(TreeNode* root) {
        if(root == NULL) {
            return 0;
        }
        int l = dfs(root -> left);
        int r = dfs(root -> right);
        res = max(res, l + r + 1);
        return max(l, r) + 1;
    }
};
```

<br/>


### GO
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
var res int

func max(a int, b int) int {
    if a > b {
        return a
    }
    return b
}

func dfs(root *TreeNode) int {
    if root == nil {
        return 0
    }
    l := dfs(root.Left)
    r := dfs(root.Right)
    res = max(res, l + r + 1)
    return max(l, r) + 1   
}

func diameterOfBinaryTree(root *TreeNode) int {
    res = 0
    dfs(root)
    return res - 1
}

```
<br/>
### Python

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def __init__(self):
        self.res = 0
    
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.res = 0
        self.dfs(root)
        return self.res - 1
    
    def dfs(self, root: Optional[TreeNode]) -> int:
        if root == None:
            return 0
        l = self.dfs(root.left)
        r = self.dfs(root.right)
        self.res = max(self.res, l + r + 1)
        return max(l, r) + 1
  ```
  
 ### Complexity :

* **Time** : O(n)&#x20;
* **Space** : O(n)&#x20; 
  
  
 <br/><br/><br/>
 ##  Tree Coordinates

 -  道理非常简单，定义root 的 **(x, y) coordinates** 为 (0, 0) (其实任何数字都可以)
 - dfs(root.left, x - 1, y + 1)   **(走左边的变化)**
 - dfs(root.righ, x + 1, y + 1) **(走右边的变化)**

<br/>

 ###  Find Bottom Left Tree Value
 
 ### Statements :
> Given the root of a binary tree, return the leftmost value in the last row of the tree.

![3ce396eb99823a0098c290770de3009](https://user-images.githubusercontent.com/45537132/169601803-1c82bc16-3a21-43a7-918f-77fe49c8c9ee.png)

### **Explanation:**
* 这里我们就可以使用 **(x, y) coordinates**
* 我们要找的 **Bottom Left**  点拥有最大的 y, 然后在所有相同y值的点中选出x值最小的那个即可

### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int X = -1, Y = -1, res = -1;
    public int findBottomLeftValue(TreeNode root) {
        dfs(root, 0, 0);
        return res;
    }
    
    public void dfs(TreeNode root, int x, int y) {
        if(root == null) {
            return;
        }
        dfs(root.left, x - 1, y + 1);
        dfs(root.right, x + 1, y + 1);
        if(y >= Y) {
            if(y > Y) {
                Y = y;
                X = x;
                res = root.val;
            } else {
                if(x < X) {
                    X = x;
                    res = root.val;
                }
            }
        }
    }
}
```
<br/>

### C++

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int X = -1, Y = -1, res = -1;
    int findBottomLeftValue(TreeNode* root) {
        dfs(root,0 , 0);
        return res;
    }
    
    void dfs(TreeNode* root, int x, int y) {
        if(root == NULL) {
            return;
        }
        dfs(root -> left, x - 1, y + 1);
        dfs(root -> right, x + 1, y + 1);
        if(y >= Y) {
            if(y > Y) {
                Y = y;
                X = x;
                res = root -> val;
            } else {
                if(x < X) {
                    X = x;
                    res = root -> val;
                }
            }
        }
    }
};
```
<br/>

### GO
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

var res, dep, l int = 0, 0, 0

func dfs(root *TreeNode, x int, y int) {
    if root == nil {
        return
    }
    
    if y > dep {
        dep = y
        l = x
        res = root.Val
    } else if y == dep {
        if x < l {
            l = x
            res = root.Val
        }
    }
    
    dfs(root.Left, x - 1, y + 1)
    dfs(root.Right, x + 1, y + 1)
}

func findBottomLeftValue(root *TreeNode) int {
    res = -1
    dep = -1
    l = -1
    
    dfs(root, 0, 0)
    return res
}

```
<br/>

### Python

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.res = -1
        self.X = -1
        self.Y = -1
    
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.dfs(root, 0, 0)
        return self.res
        
    def dfs(self, root, x, y):
        if root == None:
            return
        self.dfs(root.left, x - 1, y + 1)
        self.dfs(root.right, x + 1, y + 1)
        if y >= self.Y:
            if y > self.Y:
                self.Y = y
                self.X = x
                self.res = root.val
            else:
                if x < self.X:
                    self.X = x
                    self.res = root.val
  ```
  
 ### Complexity :

* **Time** : O(n)&#x20;
* **Space** : O(n)&#x20; 
