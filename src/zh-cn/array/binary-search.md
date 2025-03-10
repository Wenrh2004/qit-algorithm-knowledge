# 二分查找

## 题目描述

[题目链接](https://leetcode.cn/problems/binary-search/description)

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例 1:

输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
示例 2:

输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1

提示：

你可以假设 nums 中的所有元素是不重复的。
n 将在 [1, 10000]之间。
nums 的每个元素都将在 [-9999, 9999]之间。

---

## 解题思路

### 前提

- 数组为**有序数组**
- 数组中**无重复元素**

### 难点

- 二分法的区间定义
  - 核心思想 **循环不变量**

> 循环不变量是在算法的循环过程中，保持不变的性质或条件。在二分查找中，循环不变量指的是：**在每次循环中，我们都能确保目标值要么不存在，要么一定在当前搜索区间内**。
> 
> 理解循环不变量有助于：
>
> 1. 正确定义搜索区间（左闭右闭或左闭右开）
> 2. 精确控制边界条件
> 3. 避免出现死循环或边界错误

### 步骤拆解

1. 确定数组的边界
2. 确定循环不变量
3. 确定循环条件
4. 确定中间位置

### 二分法区间定义

- 左闭右闭区间
  - 循环条件 `while (left <= right)`
  - 中间位置 `int mid = left + ((right - left) / 2);`
  - 右边界更新 `right = mid - 1;`
  - 左边界更新 `left = mid + 1;`

- 左闭右开区间
  - 循环条件 `while (left < right)`
  - 中间位置 `int mid = left + ((right - left) >> 1);`
  - 右边界更新 `right = mid;`
  - 左边界更新 `left = mid + 1;`

### 代码实现

#### Go

- 左闭右闭区间

```go
func search(nums []int, target int) int {
    left, right := 0, len(nums) - 1
    for (left <= right) {
        mid := left + (right - left) >> 1
        if (nums[mid] > target) {
            right = mid - 1
        } else if (nums[mid] < target) {
            left = mid + 1
        } else {
            return mid
        }
    }
    return -1;
}
```

- 左闭右开区间

```go
func search(nums []int, target int) int {
    left, right := 0, len(nums)

    for (left < right) {
        mid := left + (right - left) >> 1
        if (nums[mid] > target) {
            right = mid
        } else if (nums[mid] < target) {
            left = mid + 1
        } else {
            return mid
        }
    }
    return -1;
}
```
