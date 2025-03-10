# 前缀和

## 题目描述

[题目链接](https://kamacoder.com/problempage.php?pid=1070)

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

输入描述
第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间下标：a，b （b > = a），直至文件结束。

输出描述
输出每个指定区间内元素的总和。

输入示例
> 5
> 1
> 2
> 3
> 4
> 5
> 0 1
> 1 3

输出示例
> 3
> 9

数据范围：
> 0 < n <= 100000

## 解题思路

对于区间和问题，最优的解题思路是使用**前缀和**算法。下面是详细的解题思路：

## 前缀和算法

前缀和是一种预处理技术，它可以将多次区间查询的时间复杂度从 O(n) 降低到 O(1)。

### 基本原理

1. **预处理阶段**：
   - 创建一个前缀和数组 `prefixSum`，其中 `prefixSum[i]` 表示原数组前 i 个元素的和
   - 对于原数组 `arr`，有 `prefixSum[i] = prefixSum[i-1] + arr[i-1]`（假设数组下标从0开始）
   - 通常我们会设 `prefixSum[0] = 0`，表示前0个元素的和为0

2. **查询阶段**：
   - 要计算区间 `[a, b]` 的和，只需计算 `prefixSum[b+1] - prefixSum[a]`
   - 这是因为 `prefixSum[b+1]` 包含了前 b+1 个元素的和，而 `prefixSum[a]` 包含了前 a 个元素的和

### 代码实现

#### Go

```go
package main

import (
 "bufio"
 "fmt"
 "os"
 "strconv"
)

func main() {
 scanner := bufio.NewScanner(os.Stdin)
 scanner.Split(bufio.ScanLines)

 scanner.Scan()
 n, _ := strconv.Atoi(scanner.Text())

 arr := make([]int, n)
 for i := 0; i < n; i++ {
  scanner.Scan()
  arr[i], _ = strconv.Atoi(scanner.Text())
 }

 prefixSum := getPrefixSum(arr)

 for scanner.Scan() {
  a, _ := strconv.Atoi(scanner.Text())
  scanner.Scan()
  b, _ := strconv.Atoi(scanner.Text())
  sum := prefixSum[b+1] - prefixSum[a]
  fmt.Println(sum)
 }
}

func getPrefixSum(input []int) []int {
 prefixSum := make([]int, len(input))
 prefixSum[0] = 0
 for i, v := range input {
  prefixSum[i+1] = prefixSum[i] + v
 }
 return prefixSum
}
```

#### Java

```java
public class IntervalSum {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 读取数组长度
        int n = scanner.nextInt();
        
        // 创建原始数组
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
        }
        
        // 计算前缀和数组
        int[] prefixSum = new int[n + 1];
        prefixSum[0] = 0;
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + arr[i];
        }
        
        // 处理区间查询
        while (scanner.hasNext()) {
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            
            // 计算区间和
            int sum = prefixSum[b + 1] - prefixSum[a];
            System.out.println(sum);
        }
        
        scanner.close();
    }
}
```

### 时间复杂度分析

- 预处理阶段：O(n)，其中 n 是数组长度
- 每次查询：O(1)
- 总体时间复杂度：O(n + q)，其中 q 是查询次数

### 空间复杂度分析

- 需要额外的 O(n) 空间来存储前缀和数组

## 适用场景

前缀和算法特别适合处理以下场景：

1. 需要频繁查询数组区间和
2. 数组元素不会被修改（静态数组）

如果需要处理动态修改的情况，可以考虑使用树状数组（Binary Indexed Tree）或线段树（Segment Tree）等更高级的数据结构。
