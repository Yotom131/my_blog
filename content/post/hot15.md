+++

title = '15.三数之和'
date = 2024-04-05T14:56:10+08:00
author = "Yotom"
description = "力扣hot100的一道题目"
tags = [
    "力扣",
    "hot100",
    "双指针",
    "中等",

]
categories = [
    "学习记录",
    "算法",
    "力扣hot100",
]

+++

# 三数之和

这道题我最开始的解法超时了，太暴力了，思绪有点没转过来。

---

## 题目链接

[15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/?envType=study-plan-v2&envId=top-100-liked)

---

## 解题代码

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        set<multiset<int>> res;
        for(int i = 0; i < nums.size()-2; i++)
        {
            for(int j = i+1; j < nums.size()-1; j++){
                for(int k = j+1; k < nums.size(); k++){
                    if(nums[i] + nums[j] + nums[k] == 0){
                        multiset<int> tmp = {nums[i], nums[j], nums[k]};
                        res.insert(tmp);
                    }

                }
            }
        }
        vector<vector<int>> ans;
        for(auto i: res){
            vector<int> t(i.begin(), i.end());
            ans.push_back(t);
        }
        return ans;
    }
};
```

---

## 思路

没有想太多，看见组不能重复就想到了set和multiset（可以重复的set）。

multiset可以实现排序，然后再利用set去掉重复的，就可以输出答案。

找到目标值的思路也很暴力，三层循环找，然后条件判断存入即可。

最后来一层转换，实现`vector<vector>`形式的输出。

但这个代码只能通过部分测试样例，时间复杂度还有待优化。

---

## 优化

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size(); i++){
            if(i > 0 && nums[i]==nums[i-1]) continue;
            int tar = -nums[i];
            int l = i+1, r = nums.size()-1;
            while(l<r){
                if(nums[l]+nums[r] == tar) {
                    res.push_back({nums[i], nums[l], nums[r]});
                    while(l<r && nums[l] == nums[l+1]) l++;
                    while(l<r && nums[r] == nums[r-1]) r--;
                    l++;
                    r--;
                }else if(nums[l]+nums[r] > tar) r--;
                else l++;
            }
        }
        return res;
    }
};
```

这个思路是经典双指针算法。

优化主要有三个重点

1. 把数列排序，这样子可以帮助我们方便操纵指针指向的数应该变大还是变小。
2. 使用双指针。
3. 去重处理，i、r、l各个位置的值与上一次相同的时候应该跳过。

经过以上处理，AC拿下。

---