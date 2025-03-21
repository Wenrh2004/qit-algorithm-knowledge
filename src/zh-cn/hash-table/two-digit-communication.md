# 两个数组的交集

## 例题

[例题链接](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

题意：给定两个数组，编写一个函数来计算它们的交集。

**说明：** 输出结果中的每个元素一定是唯一的。 我们可以不考虑输出结果的顺序。

## 思路

学会使用：unordered_set

输出结果中的每个元素一定是唯一的，也就是说输出的结果的去重的， 同时可以不考虑输出结果的顺序

注意只有在限制题目数值大小时才能使用数组哈希，而且如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。

std::set和std::multiset底层实现都是红黑树，std::unordered_set的底层实现是哈希表， 使用unordered_set 读写效率是最高的，并不需要对数据进行排序，而且还不要让数据重复，所以选择unordered_set。

```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果，之所以用set是为了给结果集去重
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里又出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```