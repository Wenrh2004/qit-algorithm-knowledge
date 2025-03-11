# 树的遍历

## 思路

### 递归遍历

**递归三要素**

1. **确定递归函数的参数和返回值**： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

2. **确定终止条件**： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

3. **确定单层递归的逻辑**： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

### 迭代遍历

迭代遍历是通过显式使用栈或队列等数据结构来模拟递归过程，避免了系统栈的开销和递归深度限制。

## 代码实现

### 前序遍历（根-左-右）

#### 递归遍历

1. 访问根节点
2. 递归遍历左子树
3. 递归遍历右子树

```go
func preOrderTraversal(root *TreeNode) []int {
    result := []int{}
    
    // 定义递归函数
    var traversal func(node *TreeNode)
    traversal = func(node *TreeNode) {
        // 终止条件, 当节点为空时返回
        if node == nil {
            return
        }
        // 访问根节点
        result = append(result, node.Val)
        // 递归遍历左子树
        traversal(node.Left)
        // 递归遍历右子树
        traversal(node.Right)
    }

    // 调用递归函数
    traversal(root)
    return result
}
```

#### 迭代遍历

1. 初始化一个栈，将根节点入栈
2. 当栈不为空时，执行以下操作：
   - 弹出栈顶节点，并访问它
   - 将右子节点入栈（如果存在）
   - 将左子节点入栈（如果存在）
   - 注意：先右后左入栈，因为栈是后进先出的

```go
func preOrderTraversal(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result
    }
    
    stack := []*TreeNode{root}
    
    for len(stack) > 0 {
        // 弹出栈顶元素
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        
        // 访问节点
        result = append(result, node.Val)
        
        // 先右后左入栈（因为栈是后进先出）
        if node.Right != nil {
            stack = append(stack, node.Right)
        }
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
    }
    
    return result
}
```

### 中序遍历（左-根-右）

#### 递归遍历

1. 递归遍历左子树
2. 访问根节点
3. 递归遍历右子树

```go
func inOrderTraversal(root *TreeNode) []int {
    result := []int{}
    // 定义递归函数
    var traversal func(node *TreeNode)
    traversal = func(node *TreeNode) {
        // 终止条件, 当节点为空时返回
        if node == nil {
            return
        }
        // 递归遍历左子树
        traversal(node.Left)
        // 访问根节点
        result = append(result, node.Val)
        // 递归遍历右子树
        traversal(node.Right)
    }
    // 调用递归函数
    traversal(root)
    return result
}
```

#### 迭代遍历

1. 初始化一个栈和一个当前节点指针
2. 当栈不为空或者当前节点不为空时，执行以下操作：
   - 如果当前节点不为空，将其入栈，并将当前节点更新为其左子节点
   - 如果当前节点为空，弹出栈顶节点，并访问它，然后将当前节点更新为其右子节点

```go
func inOrderTraversal(root *TreeNode) []int {
    result := []int{}
    stack := []*TreeNode{}
    cur := root
    for cur != nil || len(stack) > 0 {
        // 如果当前节点不为空，将其入栈，并将当前节点更新为其左子节点
        if cur != nil {
            stack = append(stack, cur)
            cur = cur.Left
        } else {
            // 如果当前节点为空，弹出栈顶节点，并访问它，然后将当前节点更新为其右子节点
            cur = stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            result = append(result, cur.Val)
            cur = cur.Right
        }
    }
    return result
}
```

### 后序遍历 （左-右-根）

#### 递归遍历

1. 递归遍历左子树
2. 递归遍历右子树
3. 访问根节点

```go
func postOrderTraversal(root *TreeNode) []int {
    result := []int{}
    // 定义递归函数
    var traversal func(node *TreeNode)
    traversal = func(node *TreeNode) {
        // 终止条件, 当节点为空时返回
        if node == nil {
            return
        }
        // 递归遍历左子树
        traversal(node.Left)
        // 递归遍历右子树
        traversal(node.Right)
        // 访问根节点
        result = append(result, node.Val)
    }
    // 调用递归函数
    traversal(root)
    return result
}
```

#### 迭代遍历

1. 初始化一个栈和一个当前节点指针
2. 当栈不为空或者当前节点不为空时，执行以下操作：
   - 如果当前节点不为空，将其入栈，并将当前节点更新为其右子节点
   - 如果当前节点为空，弹出栈顶节点，并访问它，然后将当前节点更新为其左子节点

```go
func postOrderTraversal(root *TreeNode) []int {
    result := []int{}
    stack := []*TreeNode{}
    cur := root
    for cur!= nil || len(stack) > 0 {
        // 如果当前节点不为空，将其入栈，并将当前节点更新为其右子节点
        if cur!= nil {
            stack = append(stack, cur)
            cur = cur.Right
        } else {
            // 如果当前节点为空，弹出栈顶节点，并访问它，然后将当前节点更新为其左子节点
            cur = stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            result = append(result, cur.Val)
            cur = cur.Left
        }
    }
    return result
}
```

### 层序遍历

#### 递归遍历

```go
func levelOrder(root *TreeNode) [][]int {
    result := [][]int{}
    
    // 定义递归函数
    var traversal func(node *TreeNode, depth int)
    traversal = func(node *TreeNode, depth int) {
        // 终止条件, 当节点为空时返回
        if node == nil {
            return
        }
        
        // 如果结果列表的长度等于当前深度，则添加一个新的空列表
        if len(result) == depth {
            result = append(result, []int{})
        }
        
        // 将当前节点的值添加到对应深度的结果列表中
        result[depth] = append(result[depth], node.Val)
        
        // 递归遍历左子树和右子树，深度加1
        traversal(node.Left, depth+1)
        traversal(node.Right, depth+1)
    }
    
    // 从根节点开始，深度为0
    traversal(root, 0)
    return result
}
```

#### 队列

1. 初始化一个队列和一个结果列表
2. 将根节点入队
3. 当队列不为空时，执行以下操作：
   - 弹出队首节点，并将其值添加到结果列表中
   - 将节点的左子节点和右子节点入队（如果存在）

```go
func levelOrder(root *TreeNode) [][]int {
    result := [][]int{}
    if root == nil {
        return result
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        // 获取当前层的节点数
        levelSize := len(queue)
        // 初始化一个空列表，用于存储当前层的节点值
        level := []int{}
        for i := 0; i < levelSize; i++ {
            // 弹出队首节点
            node := queue[0]
            queue = queue[1:]
            // 将节点的值添加到当前层的列表中
            level = append(level, node.Val)
            // 将节点的左子节点和右子节点入队（如果存在）
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
        // 将当前层的节点值添加到结果列表中
        result = append(result, level)
    }
    return result
}
```
