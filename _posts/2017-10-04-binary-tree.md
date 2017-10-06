---
layout: post
title:  "JavaScript和树"
categories: Javascript
date:   2017-10-04 20:48:05
author: Logan
tags:  binary
---

* content
{:toc}

# 二叉树的概念

二叉树（Binary Tree）是n（n>=0）个结点的有限集合，该集合或者为空集（空二叉树），或者由一个根结点和两棵互不相交的、分别称为根结点的左子树和右子树的二叉树组成。

![二叉树的概念](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary1.jpg "二叉树的概念")

# 二叉树的特点

每个结点最多有两棵子树，所以二叉树中不存在度大于2的结点。二叉树中每一个节点都是一个对象，每一个数据节点都有三个指针，分别是指向父母、左孩子和右孩子的指针。每一个节点都是通过指针相互连接的。相连指针的关系都是父子关系。

# 二叉树的五种基本形态

- 空二叉树
- 只有一个根结点
- 根结点只有左子树
- 根结点只有右子树
- 根结点既有左子树又有右子树





![二叉树的五种基本形态](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary2.jpg "二叉树的五种基本形态")

# 特殊二叉树

## 斜树

在一棵二叉树中，如果所有分支结点只存在**左子树**或者**右子树**

## 满二叉树

在一棵二叉树中，如果所有分支结点都存在左子树和右子树，并且所有叶子都在同一层上，这样的二叉树称为满二叉树。如下图所示：

![满二叉树](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary3.png "满二叉树")

## 完全二叉树

完全二叉树是指最后一层左边是满的，右边可能满也可能不满，然后其余层都是满的

完全二叉树的**特点**：

>叶子结点只能出现在最下两层。<br>
最下层的叶子一定集中在左部连续位置。<br>
倒数第二层，若有叶子结点，一定都在右部连续位置。<br>
如果结点度为1，则该结点只有左孩子。<br>
同样结点树的二叉树，完全二叉树的深度最小。

![完全二叉树](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary4.png "完全二叉树")

# 二叉树的性质

- 二叉树的性质一：在二叉树的第i层上至多有2^(i-1)个结点(i>=1)
- 二叉树的性质二：深度为k的二叉树至多有2^k-1个结点(k>=1)

# 二叉树的顺序存储结构

二叉树的顺序存储结构就是用一维数组存储二叉树中的各个结点，并且结点的存储位置能体现结点之间的逻辑关系。

![二叉树的顺序存储结构](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary5.jpg "二叉树的顺序存储结构")

# 二叉链表

二叉树的存储按照国际惯例来说一般也是采用链式存储结构的。

二叉树每个结点最多有两个孩子，所以为它设计一个数据域和两个指针域是比较自然的想法，我们称这样的链表叫做二叉链表。

![二叉链表](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary6.jpg "二叉链表")

# 二叉树的遍历

二叉树的遍历(traversing binary tree)是指从根结点出发，按照某种次序依次访问二叉树中所有结点，使得每个结点被访问一次且仅被访问一次。

二叉树的遍历有**三种方式**，如下：

- 前序遍历（DLR），首先访问根结点，然后遍历左子树，最后遍历右子树。简记根-左-右。
- 中序遍历（LDR），首先遍历左子树，然后访问根结点，最后遍历右子树。简记左-根-右。
- 后序遍历（LRD），首先遍历左子树，然后遍历右子树，最后访问根结点。简记左-右-根。

## 前序遍历

若二叉树为空，则空操作返回，否则先访问根结点，然后前序遍历左子树，再前序遍历右子树。

![前序遍历](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary7.jpg "前序遍历")

遍历的顺序为：`A B D H I E J C F K G`

```js
//前序遍历
function preOrder(node){
    if(!node == null){
        putstr(node.show()+ " ");
        preOrder(node.left);
        preOrder(node.right);
    }
}
```

## 中序遍历

若树为空，则空操作返回，否则从根结点开始（注意并不是先访问根结点），中序遍历根结点的左子树，然后是访问根结点，最后中序遍历右子树。

![中序遍历](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary8.jpg "中序遍历")

遍历的顺序为：`H D I B E J A F K C G`

```js
//使用递归方式实现中序遍历
function inOrder(node){
    if(!(node == null)){
        inOrder(node.left);//先访问左子树
        putstr(node.show()+ " ");//再访问根节点
        inOrder(node.right);//最后访问右子树
    }
}
```

## 后序遍历

若树为空，则空操作返回，否则从左到右先叶子后结点的方式遍历访问左右子树，最后访问根结点。

![后序遍历](https://raw.githubusercontent.com/logan70/logan70.github.io/master/images/2017-10-06/binary9.jpg "后序遍历")

遍历的顺序为：`H I D J E B K F G C A`

```js
//后序遍历
function postOrder(node){
    if(!node == null){
        postOrder(node.left);
        postOrder(node.right);
        putStr(node.show()+ " ");
    }
}
```

# 实现二叉查找树

二叉查找树（BST）由节点组成，所以我们定义一个Node节点对象如下：

```js
function Node(data,left,right){
    this.data = data;
    this.left = left;//保存left节点链接
    this.right = right;
    this.show = show;
}


function show(){
    return this.data;//显示保存在节点中的数据
}
```

# 查找最大和最小值

查找BST上的最小值和最大值非常简单，因为较小的值总是在左子节点上，在BST上查找最小值，只需遍历左子树，直到找到最后一个节点

## 查找最小值

```js
function getMin(){
    var current = this.root;
    while(!(current.left == null)){
        current = current.left;
    }
    return current.data;
}
```

该方法沿着BST的左子树挨个遍历，直到遍历到BST最左的节点，该节点被定义为：

```js
current.left = null;
```

这时，当前节点上保存的值就是最小值

## 查找最大值

在BST上查找最大值只需要遍历右子树，直到找到最后一个节点，该节点上保存的值就是最大值。

```js
function getMax(){
    var current = this.root;
    while(!(current.right == null)){
        current = current.right;
    }
    return current.data;
}
```