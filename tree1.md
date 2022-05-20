
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
  
###  Tree Diameter

 - 树的直径 ：设**dis(u, v)** 是 vertex u 到 vertex v 的最短距离。 **Tree Diameter = max(dis(u, v))**
 - 如何求树的直径？使用  [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree) 作为例子

<br/>
 
### Java
```
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

```
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
```
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

### Python

```
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
