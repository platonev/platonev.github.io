---
layout: post
title: "二叉树的遍历与重建"
date: 2017-07-25 11:57:00 +0800
categories: data-structure tree
---
## 遍历
1. 前序遍历：先处理**当前节点**，再处理两颗子树（标记节点的深度）

2. 中序遍历：首先处理左子树，然后是**当前节点**，最后处理右子树
```java
private void printTree(BinaryNode<AnyType> t) {
    if(t != null) {
        printTree(t.left);
        System.out.println(t.element);
        printTree(t.right);
    }
}
```
3. 后序遍历：先处理两颗子树，再处理**当前节点**（计算节点的高度）
```java
private int height(BinaryNode<AnyType> t) {
    if(t == null)
        return -1;
    else
        return 1 + Math.max(height(t.left), height(t.right));
}
```

> 注： 从上面的描述可以看出，所谓的前序、中序和后序是按照**处理当前节点的位置**来划分的

**例：**
```
                   a
                 /   \
                b     c
               / \   / \
              d   e f   g
             /
            h
```
前序遍历结果：a b d h e c f g

中序遍历结果：h d b e a f c g

后序遍历结果：h d e b f g c a

# 重建：

**已知一棵二叉树的前序遍历序列和中序遍历序列，构造该二叉树的过程如下：**

1. 根据前序遍历序列的第一个元素建立根结点；

2. 在中序遍历序列中找到该元素，确定根结点的左右子树的中序遍历序列；

3. 在前序遍历序列中确定左右子树的前序遍历序列；

4. 由左子树的前序遍历序列和中序遍历序列建立左子树；

5. 由右子树的前序遍历序列和中序遍历序列建立右子树。

**例：** 根据前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}构造二叉树；

1. 由前序序列知树的根节点为1，左子树的前序遍历序列为{2,4,7}，中序遍历序列为{4,7,2}；右子树的前序遍历序列和中序遍历序列分别为{3,5,6,8}、{5,3,8,6}:
```
                     1
                   /   \
                 /       \
              2 4 7    3 5 6 8        前序遍历序列
              4 7 2    5 3 8 6        中序遍历序列
```
2. 对于左子树，从前序遍历序列{2,4,7}知，根节点为2，从中序遍历序列知其左子树的前序遍历序列和中序遍历中序遍历序列分别为{4,7}、{};相应的右子树的根节点为3，其左子树的前序遍历序列和中序遍历中序遍历序列分别为{5}、{8,6}:
```
                     1
                   /   \
                 2       3
               /   \   /   \
             4 7      5    6 8        前序遍历序列
             4 7      5    8 6        中序遍历序列
```
3. ...
```
                     1
                   /   \
                 2       3
                / \    /   \
               4      5     6
              / \    / \   / \
                 7        8           前序遍历序列
                 7        8           中序遍历序列
```
4. ...
```
                     1
                   /   \
                 2       3
                /      /   \
               4      5     6
                \          /
                 7        8
```

Java代码实现：
```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

TreeNode reConstructBinaryTree(int[] pre,int[] in) {

    if(pre.length == 0) {
        return null;
    }

    TreeNode treeNode = new TreeNode(pre[0]);

    int m = find(in, pre[0]);

    treeNode.left = reConstructBinaryTree(subArray(pre, 1,  m + 1), subArray(in, 0, m));

    treeNode.right = reConstructBinaryTree(subArray(pre, m + 1, pre.length), subArray(in, m + 1, in.length));

    return treeNode;
}

private int find(int[] array, int t) {
    int i = 0;
    for(;i < array.length; i++) {
        if(array[i] == t)
            break;
    }
    return i;
}

private int[] subArray(int[] array, int start, int end) {
    int[] a = new int[end - start];

    for(int i = start; i < end; i++) {
        a[i-start] = array[i];
    }

    return a;
}
```


**已知一棵二叉树的后序遍历序列和中序遍历序列，构造该二叉树的过程如下：**

1. 根据后序遍历序列的最后一个元素建立根结点；

2. 在中序遍历序列中找到该元素，确定根结点的左右子树的中序遍历序列；

3. 在后序遍历序列中确定左右子树的后序遍历序列；

4. 由左子树的后序遍历序列和中序遍历序列建立左子树；

5. 由右子树的后序遍历序列和中序遍历序列建立右子树。
