---
title: "Trees"
description: "2023"
date: "2023-01-22"
tags:
- datastructures
---

In this article, we will solve trees-related problems that are commonly encountered in interviews.

**1. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/).**
```java
public TreeNode invertTree(TreeNode root) {
    if(root == null) {
        return null;
    }
    TreeNode tempNode = root.left;
    root.left = root.right;
    root.right = tempNode;
    invertTree(root.left);
    invertTree(root.right);
    return root;
}
```
Time complexity: O(n) The given code performs a depth-first search traversal of the binary tree, and visits each node exactly once. Therefore, the time complexity of the code is O(n), where n is the number of nodes in the binary tree.

Space complexity: O(n) The space complexity of the code depends on the maximum depth of the binary tree. In the worst case, if the binary tree is skewed (i.e., all the nodes are in a straight line), the maximum depth of the tree would be n (the number of nodes), and the space complexity of the code would be O(n) due to the recursive function calls. However, in the best case, if the binary tree is balanced, the maximum depth of the tree would be log(n), and the space complexity of the code would be O(log(n)).

---

**2. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/).**
```java
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return  1 + Math.max(left,right);
    }
```
Time complexity: O(n) where n is the number of nodes in the binary tree, since the code visits each node exactly once.

Space complexity: O(h), where h is the height of the binary tree, since the code uses a recursive approach, and the maximum depth of the recursive call stack is equal to the height of the binary tree. In the worst case, where the binary tree is skewed (i.e., all the nodes are in a straight line), the height of the tree would be n (the number of nodes), and the space complexity of the code would be O(n). However, in the best case, where the binary tree is balanced, the height of the tree would be log(n), and the space complexity of the code would be O(log(n)).

---

**3. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/).**
```java
public int diameterOfBinaryTree(TreeNode root) {
    int leftDepth  = depth(root.left);
    int rightDepth = depth(root.right);
    return Math.max(Math.max(diameterOfBinaryTree(root.left), diameterOfBinaryTree(root.right)), leftDepth + rightDepth);
}

private int depth(TreeNode root){
    if(root == null){
        return 0;
    }
    
    int left = depth(root.left);
    int right = depth(root.right);

    return 1 + Math.max(left, right);
}
```

Time complexity:  O(n^2^) wwhere n is the number of nodes in the binary tree, since for each node, the code visits all the nodes in its left and right subtrees to compute the diameter.

Space complexity: O(h), where h is the height of the binary tree, since the code uses a recursive approach, and the maximum depth of the recursive call stack is equal to the height of the binary tree. In the worst case, where the binary tree is skewed (i.e., all the nodes are in a straight line), the height of the tree would be n (the number of nodes), and the space complexity of the code would be O(n). However, in the best case, where the binary tree is balanced, the height of the tree would be log(n), and the space complexity of the code would be O(log(n)).

Optimized Verison : We can optimize by computing the diameter of the binary tree in a single pass, instead of recursively computing the depth of each node multiple times.

```java
public int diameterOfBinaryTree(TreeNode root) {
    int[] max = {0};
    depth(root, max);
    return max[0];
}

private int depth(TreeNode root, int[] max) {
    if (root == null) {
        return 0;
    }
    
    int left = depth(root.left, max);
    int right = depth(root.right, max);
    
    max[0] = Math.max(max[0], left + right);
    
    return 1 + Math.max(left, right);
}
```
Time complexity:  O(n) where n is the number of nodes in the binary tree.

Space complexity: O(h), where h is the height of the binary tree.

--- 

**4. [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/).**
```java
public boolean isBalanced(TreeNode root) {        
    if(root == null){
        return true;
    }
    int leftHeight = height(root.left);
    int rightHeight = height(root.right);

    if(Math.abs(leftHeight - rightHeight) > 1){
        return false;
    }

    return isBalanced(root.left) && isBalanced(root.right); 
}

private int height(TreeNode root){
    if(root == null){
        return 0;
    }

    int left = height(root.left);
    int right = height(root.right);

    return 1 + Math.max(left,right);
}
```
Time complexity:  O(n log n) in the worst case, where n is the number of nodes in the binary tree. This is because the height of each node is computed recursively using the height method, which has a time complexity of O(n) in the worst case. Since the height method is called for each node in the binary tree, the overall time complexity of isBalanced is O(n log n), because the binary tree has a maximum of log n levels.

Space complexity:  O(n) in the worst case, where n is the number of nodes in the binary tree. This is because the height method is called recursively for each node in the binary tree, and the maximum depth of the recursive call stack is equal to the height of the binary tree, which is O(n) in the worst case. Additionally, the method uses a constant amount of extra space to store the heights of the left and right subtrees of each node. Therefore, the overall space complexity of isBalanced is O(n).

Optimized version : 
```java
public boolean isBalanced(TreeNode root) {        
    return checkBalance(root) != -1;
}

private int checkBalance(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int left = checkBalance(root.left);
    if (left == -1) {
        return -1;
    }
    int right = checkBalance(root.right);
    if (right == -1) {
        return -1;
    }
    if (Math.abs(left - right) > 1) {
        return -1;
    }
    return 1 + Math.max(left, right);
}
```

The time complexity of the optimized isBalanced method is O(n), where n is the number of nodes in the binary tree, because each node is visited only once and its height is computed in constant time. The space complexity is O(n) as well, because the maximum depth of the recursive call stack is equal to the height of the binary tree, which is O(n) in the worst case.

---

**5. [Same Tree](https://leetcode.com/problems/same-tree/).**
```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    }
    if (p == null || q == null || p.val != q.val) {
        return false;
    }
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```
Time complexity:  O(n), where n is the total number of nodes in the tree. This is because the function performs a recursive traversal of the entire tree, visiting each node once.

Space complexity: O(n), where n is the height of the tree. This is because the function uses a call stack to keep track of the recursive calls, and the maximum size of the call stack is proportional to the height of the tree. In the worst case, when the tree is completely unbalanced and resembles a linked list, the space complexity of the function becomes O(n). However, in a balanced tree, the space complexity is closer to O(log n), where log n is the height of the tree.

--- 

**6. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/).**
```java
public boolean isSubtree(TreeNode s, TreeNode t) {
    if (s == null) {
        return false;
    }
    if (isSameTree(s, t)) {
        return true;
    }
    return isSubtree(s.left, t) || isSubtree(s.right, t);
}

public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    }
    if (p == null || q == null || p.val != q.val) {
        return false;
    }
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```
Time complexity:  O(m * n), where m and n are the number of nodes in the trees s and t, respectively. This is because in the worst case, the function performs an isSameTree check for each node in s with t, which takes O(n) time. However, in the average case, the time complexity is closer to O(m), as the function will terminate early once it finds a matching subtree.

Space complexity:  O(max(m, n)), as the function uses a call stack to keep track of the recursive calls, and the maximum size of the call stack is proportional to the height of the trees. In the worst case, when the trees are completely unbalanced and resemble linked lists, the space complexity becomes O(m) or O(n), whichever is greater.

Optimized version : 
```java
public boolean isSubtree(TreeNode s, TreeNode t) {
    String sPreOrder = getPreOrder(s);
    String tPreOrder = getPreOrder(t);
    return sPreOrder.indexOf(tPreOrder) != -1;
}

private String getPreOrder(TreeNode node) {
    if (node == null) {
        return "null";
    }
    String left = getPreOrder(node.left);
    String right = getPreOrder(node.right);
    return "#" + node.val + " " + left + " " + right;
}
```
```
sPreOrder: "#3 #4 #1 null null #2 null null #5 null null"
tPreOrder: "#4 #1 null null #2 null null"
```
The substring "#4 #1 null null #2 null null" is found in the string "#3 #4 #1 null null #2 null null #5 null null", so we know that t is a subtree of s. Therefore, the function returns true.

Note that if t were not a subtree of s, then the function would have returned false. The pre-order traversal string of t would not be a substring of the pre-order traversal string of s, so the indexOf method would return -1.

Time complexity: O(m + n), where m and n are the number of nodes in the trees s and t, respectively. This is because the function only needs to traverse each tree once to get the pre-order traversal string, and the string matching operation takes O(m + n) time in the worst case

Space complexity: O(m + n), as it needs to store the pre-order traversal strings of both trees.

---

**7. [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/).**
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || p == null || q == null) {
            return null;
        }
        
        if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        
        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        
        return root;
    }
}
```
Time complexity:  O(log N) in the best case and O(N) in the worst case, where N is the number of nodes in the BST. In the best case, when the BST is balanced, the time complexity of the algorithm is O(log N) since we eliminate half of the tree at each level. In the worst case, when the BST is skewed, the time complexity is O(N) since we may have to traverse the entire tree.

Space complexity:  O(log N) in the best case and O(N) in the worst case. In the best case, when the BST is balanced, the space complexity of the algorithm is O(log N) since we only use a constant amount of space for each level of the recursive call stack. In the worst case, when the BST is skewed, the space complexity is O(N) since we may have to store all N nodes on the call stack.

--- 
**8. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/).**
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> levelNodes = new ArrayList<>();
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                levelNodes.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            result.add(levelNodes);
        }
        return result;
    }
}
```
Time complexity: O(N), where N is the number of nodes in the binary tree, since we need to visit each node once.

Space complexity: O(N) since we may need to store all N nodes in the queue at once in the worst case.

--- 

**9. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/).**
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                if (i == levelSize - 1) {
                    result.add(node.val);
                }
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return result;
    }
}
```
Time complexity: O(N), where N is the number of nodes in the binary tree, since we need to visit each node once.

Space complexity: O(N) since we may need to store all N nodes in the queue at once in the worst case.

---

**10. [Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/).**
```java
class Solution {
    public int goodNodes(TreeNode root) {
        return countGoodNodes(root, Integer.MIN_VALUE);
    }
    
    private int countGoodNodes(TreeNode node, int maxSoFar) {
        if (node == null) return 0;
        
        int count = 0;
        if (node.val >= maxSoFar) {
            count++;
            maxSoFar = node.val;
        }
        
        count += countGoodNodes(node.left, maxSoFar);
        count += countGoodNodes(node.right, maxSoFar);
        
        return count;
    }
}
```
Time complexity:  O(n), where n is the number of nodes in the binary tree. This is because we traverse each node once in a depth-first manner.

Space complexity:O(h), where h is the height of the binary tree. This space is used for the recursive call stack. In the worst case, where the binary tree is skewed and has a height equivalent to the number of nodes (h ≈ n), the space complexity would be O(n). However, in a balanced binary tree, the space complexity would be O(log n), where log n is the height of the tree.

--- 

**11. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/).**
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        // Use long instead of int to handle edge cases where value equals Integer.MIN_VALUE or Integer.MAX_VALUE
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean isValidBST(TreeNode node, long minVal, long maxVal) {
        if (node == null) {
            return true;
        }
        if (node.val <= minVal || node.val >= maxVal) {
            return false;
        }
        return isValidBST(node.left, minVal, node.val) && isValidBST(node.right, node.val, maxVal);
    }
}
```
Time complexity: O(n), where n is the number of nodes in the tree. This is because we visit each node exactly once during the in-order traversal.

Space complexity: O(n), where n is the number of nodes in the tree. This is because we need to store the recursive call stack during the traversal, which can be as large as the height of the tree, and in the worst case the height of the tree can be n.

---

**12. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/).**
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> list = new ArrayList<>();
        inorder(root, list);
        return list.get(k - 1);
    }

    private void inorder(TreeNode root, List<Integer> list) {
        if (root == null) return;
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```
Time complexity: O(n), where n is the number of nodes in the tree. This is because we visit each node exactly once during the in-order traversal.

Space complexity: O(n),as it uses additional space to store the in-order traversal sequence in the list.

---

**13. [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/).**
```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return buildTreeHelper(preorder, inorder, 0, 0, inorder.length - 1);
    }
    
    private TreeNode buildTreeHelper(int[] preorder, int[] inorder, int preStart, int inStart, int inEnd) {
        if (preStart > preorder.length - 1 || inStart > inEnd) {
            return null;
        }
        
        TreeNode root = new TreeNode(preorder[preStart]);
        int inIndex = 0;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == root.val) {
                inIndex = i;
            }
        }
        
        root.left = buildTreeHelper(preorder, inorder, preStart + 1, inStart, inIndex - 1);
        root.right = buildTreeHelper(preorder, inorder, preStart + inIndex - inStart + 1, inIndex + 1, inEnd);
        
        return root;
    }
}
```
Time complexity: O(n), where n is the number of nodes in the tree.

Space complexity: O(n),due to the recursion stack, where n is the height of the binary tree.

---

**14. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum).**
```java
class Solution {
    int maxSum = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        calculateMaxSum(root);
        return maxSum;
    }
    
    private int calculateMaxSum(TreeNode node) {
        if (node == null) return 0;
        
        // Calculate maximum sum in left and right subtrees
        int leftSum = Math.max(0, calculateMaxSum(node.left));
        int rightSum = Math.max(0, calculateMaxSum(node.right));
        
        // Update maxSum by considering the current node's path
        maxSum = Math.max(maxSum, node.val + leftSum + rightSum);
        
        // Return the maximum sum of the path through the current node
        return node.val + Math.max(leftSum, rightSum);
    }
}
```
Time complexity: O(n), where n is the number of nodes in the tree.

Space complexity: O(h), where h is the height of the binary tree, due to the recursion stack.

---

**15. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree).**
```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("null").append(",");
            return;
        }
        sb.append(node.val).append(",");
        serializeHelper(node.left, sb);
        serializeHelper(node.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        Queue<String> queue = new LinkedList<>(Arrays.asList(nodes));
        return deserializeHelper(queue);
    }

    private TreeNode deserializeHelper(Queue<String> queue) {
        String val = queue.poll();
        if (val.equals("null")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(val));
        node.left = deserializeHelper(queue);
        node.right = deserializeHelper(queue);
        return node;
    }
}

```
Time complexity: We split the serialized string into an array of strings, which takes O(n) time. Then, we construct the binary tree recursively by visiting each node once. Therefore, the time complexity of deserialization is linear with respect to the number of nodes in the tree.

Space complexity: The space complexity is O(n) because we use a queue to store the nodes during deserialization, and the size of the queue can be at most O(n) when all nodes are stored in it. Additionally, the recursion stack space used during deserialization is proportional to the height of the tree, which can be at most O(n) for a skewed binary tree.

---


