# 有效字母异位

## 例题

[例题链接](https://leetcode.cn/problems/valid-anagram/description/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1: 输入: s = "anagram", t = "nagaram" 输出: true

示例 2: 输入: s = "rat", t = "car" 输出: false

**说明:** 你可以假设字符串只包含小写字母。

## 思路

**数组其实就是一个简单哈希表**，而且这道题目中字符串只有小写字符，那么就可以定义一个数组，来记录字符串s里字符出现的次数。

**注意**只有在**限制题目数值大小时**才能使用数组哈希，而且如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。

1. 定一个数组叫做record，大小为26 就可以了，初始化为0，因为字符a到字符z的ASCII也是26个连续的数值。
2. 为了方便举例，判断一下字符串s= "aee", t = "eae"。


1. 因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。
2. 再遍历 字符串s的时候，只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
3. 同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。
4. record数组如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。最后如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。

```
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            record[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (record[i] != 0) {
                // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        // record数组所有元素都为零0，说明字符串s和t是字母异位词
        return true;
    }
};
```