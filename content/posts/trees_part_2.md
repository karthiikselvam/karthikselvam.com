---
title: "Trees"
description: "2024"
date: "2024-10-08"
tags:
- datastructures
---

**1. Binary Tree Inorder Traversal.**
```java
public class BinaryTreeInorderTraversal {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }

    private void inorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        // Traverse the left subtree
        inorderHelper(node.left, result);
        // Visit the root node
        result.add(node.val);
        // Traverse the right subtree
        inorderHelper(node.right, result);
    }
}

/*Iterative Approach using Stack */
public class BinaryTreeInorderTraversalIterative {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;

        while (current != null || !stack.isEmpty()) {
            // Reach the leftmost node of the current node
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            // Current must be null at this point
            current = stack.pop();
            result.add(current.val); // Add the node to the result
            // Visit the right subtree
            current = current.right;
        }

        return result;
    }
}

```

**2. Preorder Traversal.**
```java
public class BinaryTreePreorderTraversal {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderHelper(root, result);
        return result;
    }

    private void preorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        // Visit the root node
        result.add(node.val);
        // Traverse the left subtree
        preorderHelper(node.left, result);
        // Traverse the right subtree
        preorderHelper(node.right, result);
    }
}

/*Iterative Approach using Stack */
public class BinaryTreePreorderTraversalIterative {

    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            result.add(node.val);

            // Push right child first so that left child is processed first
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        return result;
    }
}

```

**3. Postorder Traversal.**
```java
public class BinaryTreePostorderTraversal {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorderHelper(root, result);
        return result;
    }

    private void postorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        // Traverse the left subtree
        postorderHelper(node.left, result);
        // Traverse the right subtree
        postorderHelper(node.right, result);
        // Visit the root node
        result.add(node.val);
    }
}

/*Iterative Approach using Stack */
public class BinaryTreePostorderTraversalIterative {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);

        while (!stack1.isEmpty()) {
            TreeNode node = stack1.pop();
            stack2.push(node);

            if (node.left != null) {
                stack1.push(node.left);
            }
            if (node.right != null) {
                stack1.push(node.right);
            }
        }

        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val);
        }

        return result;
    }
}

```

**3. Level Order Traversal.**
```java
public class BinaryTreeLevelOrderTraversal {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                currentLevel.add(currentNode.val);

                if (currentNode.left != null) {
                    queue.add(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right);
                }
            }
            result.add(currentLevel);
        }

        return result;
    }
}
```

**4. Find the maximum depth of a binary tree.**
```java
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
    }
```

**5. Check if a binary tree is height-balanced.**
```java
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    private int checkHeight(TreeNode node) {
        if (node == null) {
            return 0; // Base case: the height of an empty tree is 0
        }

        int leftHeight = checkHeight(node.left);
        int rightHeight = checkHeight(node.right);

        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1; // Not balanced
        }

        return Math.max(leftHeight, rightHeight) + 1; // Height of the current node
    }
```

**6. Validate Binary Search Tree.**
```java
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean isValidBST(TreeNode node, long min, long max) {
        if (node == null) {
            return true; // An empty node is always a valid BST
        }
        
        if (node.val <= min || node.val >= max) {
            return false; // Node's value is out of the allowed range
        }
        
        // Recursively validate the left subtree and right subtree
        return isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max);
    }
```

**7. Find the lowest common ancestor of two nodes in a BST.**

In a BST: If both nodes (p and q) are less than the root node, then the LCA must be in the left subtree. If both nodes (p and q) are greater than the root node, then the LCA must be in the right subtree. If one node is on the left and the other node is on the right of the root, then the root is the LCA.
```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        }
        
        // If both p and q are less than root, LCA is in the left subtree
        if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        
        // If both p and q are greater than root, LCA is in the right subtree
        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        
        // If one node is on the left and the other is on the right, root is the LCA
        return root;
    }
```

**8. Insert a node into a BST.**

Start at the Root: Begin at the root node. 
Traverse the Tree: Depending on the value of the node to be inserted: If the value is less than the current node's value, move to the left child. If the value is greater than or equal to the current node's value, move to the right child. 
Insert the Node: When you find an appropriate null position (left or right child), insert the new node there.

```java
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val); // Create a new node if the tree is empty
        }
        
        if (val < root.val) {
            root.left = insertIntoBST(root.left, val); // Insert in the left subtree
        } else {
            root.right = insertIntoBST(root.right, val); // Insert in the right subtree
        }
        
        return root; // Return the unchanged root
    }
```

**9. Delete Node in a BST.**
To delete a node in a Binary Search Tree (BST), you need to consider three main cases based on the number of children the node to be deleted has: Node with No Children (Leaf Node): Simply remove the node. 
Node with One Child: Remove the node and replace it with its single child. 
Node with Two Children: Find the node's in-order predecessor (maximum value in the left subtree) or in-order successor (minimum value in the right subtree), swap values with the node to be deleted, and then delete the predecessor or successor.
```java
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        
        if (key < root.val) {
            root.left = deleteNode(root.left, key);
        } else if (key > root.val) {
            root.right = deleteNode(root.right, key);
        } else {
            // Node to be deleted found
            if (root.left == null) {
                return root.right; // Node with only right child or no child
            }
            if (root.right == null) {
                return root.left; // Node with only left child
            }
            
            // Node with two children: Get the inorder predecessor (largest in the left subtree)
            TreeNode minNode = getMin(root.right);
            root.val = minNode.val; // Replace value with inorder successor's value
            root.right = deleteNode(root.right, minNode.val); // Delete the inorder successor
        }
        
        return root;
    }
    
    private TreeNode getMin(TreeNode node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }
```

**9. Convert Sorted Array to Binary Search Tree.**
Find the Middle Element: Choose the middle element of the array to be the root. This ensures that the tree will be balanced. 
Recursively Build Left and Right Subtrees: Apply the same process to the left and right subarrays to build the left and right subtrees.
```java
     public TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBSTHelper(nums, 0, nums.length - 1);
    }

    private TreeNode sortedArrayToBSTHelper(int[] nums, int left, int right) {
        if (left > right) {
            return null; // Base case: No elements to form a subtree
        }

        int mid = left + (right - left) / 2; // Find the middle index
        TreeNode root = new TreeNode(nums[mid]); // Create the root node

        root.left = sortedArrayToBSTHelper(nums, left, mid - 1); // Recursively build the left subtree
        root.right = sortedArrayToBSTHelper(nums, mid + 1, right); // Recursively build the right subtree

        return root;
    }
```

**9. Serialize and Deserialize Binary Tree.**

Serialization: Traverse the tree and record the value of each node. Use a delimiter to mark the end of children or the absence of children (e.g., null). 
Deserialization: Reconstruct the tree using the serialized data. Use the recorded values to build the tree structure, handling null values to manage missing nodes.
```java
    public String serialize(TreeNode root) {
        if (root == null) {
            return "null,";
        }
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append("null,");
            return;
        }
        sb.append(root.val).append(",");
        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);
    }

    // Deserialization
    public TreeNode deserialize(String data) {
        String[] values = data.split(",");
        Queue<String> queue = new LinkedList<>();
        for (String value : values) {
            queue.add(value);
        }
        return deserializeHelper(queue);
    }

    private TreeNode deserializeHelper(Queue<String> queue) {
        String value = queue.poll();
        if (value.equals("null")) {
            return null;
        }
        TreeNode node = new TreeNode(Integer.parseInt(value));
        node.left = deserializeHelper(queue);
        node.right = deserializeHelper(queue);
        return node;
    }
```


**10. Binary Tree Zigzag Level Order Traversal.**

Use a Queue: Use a queue to perform a level order traversal (BFS). 
Alternate Directions: Keep track of the current level's direction. Use a flag to alternate between appending nodes from left to right and from right to left. 
Add Nodes to the Current Level: Depending on the direction, either add the nodes at the current level to the end of the current level's list or to the front.
```java
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean leftToRight = true;

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            LinkedList<Integer> currentLevel = new LinkedList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode currentNode = queue.poll();
                
                if (leftToRight) {
                    currentLevel.add(currentNode.val);
                } else {
                    currentLevel.addFirst(currentNode.val);
                }
                
                if (currentNode.left != null) {
                    queue.offer(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.offer(currentNode.right);
                }
            }

            result.add(currentLevel);
            leftToRight = !leftToRight; // Toggle the direction
        }

        return result;
    }

```

**11. Diameter of Binary Tree.**

Height of Subtrees: The diameter can be computed by using the height of the left and right subtrees for each node. 
Recursive Depth-First Search (DFS): Traverse the tree using DFS and calculate the height of each subtree. During this traversal, compute the diameter as the sum of the heights of the left and right subtrees. Update Maximum Diameter: Keep track of the maximum diameter encountered during the DFS traversal.
```java
public class DiameterOfBinaryTree {
    private int diameter = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        calculateHeight(root);
        return diameter;
    }
    
    private int calculateHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        int leftHeight = calculateHeight(node.left);
        int rightHeight = calculateHeight(node.right);
        
        // Update the diameter if the path through this node is longer
        diameter = Math.max(diameter, leftHeight + rightHeight);
        
        // Return the height of the subtree rooted at this node
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

**12. Find the kth smallest element in a BST..**

In-Order Traversal: Perform an in-order traversal of the BST. This traversal visits nodes in ascending order because, in a BST, the left subtree contains smaller elements, the root is the next smallest, and the right subtree contains larger elements.
Tracking the k-th Element: During the in-order traversal, keep a count of the nodes visited. When the count equals k, the current node is the k-th smallest element.
```java
public class KthSmallestElementInBST {
    private int count = 0;
    private int result = Integer.MIN_VALUE;

    public int kthSmallest(TreeNode root, int k) {
        inOrderTraversal(root, k);
        return result;
    }

    private void inOrderTraversal(TreeNode node, int k) {
        if (node == null) {
            return;
        }

        // Traverse the left subtree
        inOrderTraversal(node.left, k);

        // Process the current node
        count++;
        if (count == k) {
            result = node.val;
            return;
        }

        // Traverse the right subtree
        inOrderTraversal(node.right, k);
    }
}
```

**13. Find the maximum path sum in a binary tree (path can start and end at any node).**

For each node, calculate the maximum sum of the path that passes through the node and update the global maximum if this value is higher.

```java
public class MaxPathSum {
    private int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        calculateMaxSum(root);
        return maxSum;
    }

    private int calculateMaxSum(TreeNode node) {
        if (node == null) {
            return 0;
        }

        // Calculate the max path sum from the left and right subtrees
        int leftMax = Math.max(0, calculateMaxSum(node.left));  // if the value is negative, ignore it
        int rightMax = Math.max(0, calculateMaxSum(node.right));

        // Calculate the path sum that passes through the current node
        int currentPathSum = node.val + leftMax + rightMax;

        // Update the global max sum if the current path sum is greater
        maxSum = Math.max(maxSum, currentPathSum);

        // Return the maximum path sum including the current node and one of its subtrees
        return node.val + Math.max(leftMax, rightMax);
    }
}
```

**14. Construct Binary Tree from Preorder and Inorder Traversal.**

Start with the first element in the preorder array as the root. Find the index of this root element in the inorder array. Elements to the left of this index in the inorder array form the left subtree, and elements to the right form the right subtree. Recursively construct the left and right subtrees using the corresponding segments of the preorder and inorder arrays.

```java
public class ConstructBinaryTree {
    private int preorderIndex = 0;
    private Map<Integer, Integer> inorderIndexMap = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // Build a map to store value -> index relations from inorder traversal
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        return constructTree(preorder, 0, inorder.length - 1);
    }

    private TreeNode constructTree(int[] preorder, int inorderStart, int inorderEnd) {
        if (inorderStart > inorderEnd) {
            return null;
        }

        // Get the current root from preorder traversal
        int rootVal = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootVal);

        // Find the index of the root in inorder traversal
        int inorderIndex = inorderIndexMap.get(rootVal);

        // Recursively construct the left and right subtrees
        root.left = constructTree(preorder, inorderStart, inorderIndex - 1);
        root.right = constructTree(preorder, inorderIndex + 1, inorderEnd);

        return root;
    }
}
    int[] preorder = {3, 9, 20, 15, 7};
    int[] inorder = {9, 3, 15, 20, 7};
```






