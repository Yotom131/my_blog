+++

title = 'hot100 11.盛最多水的容器'
date = 2024-04-04T22:35:10+08:00
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
    "力扣hot100"
]

+++

# 11.盛最多水的容器

这是一道简单的双指针算法题，力扣上标注了中等难度，我是抱着每日一题的心态来刷的，总体而言比较简单。

## 题目链接

[11. 盛最多水的容器 - 力扣（LeetCode）](https://leetcode.cn/problems/container-with-most-water/description/?envType=study-plan-v2&envId=top-100-liked)

---

## 解题代码

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ma = 0, d, l, t, s;
        for(int i = 0; i < height.size()-1; i++){
            if(i>0) if(height[i] <= l) continue;
            for(int j = height.size()-1; j >= i+1; j--){
                if(height[j] <= l) continue;
                d = j-i;
                l = height[i] > height[j] ? height[j] : height[i];
                s = d*l;
                ma = s > ma ? s : ma;
            }
        }
        
        return ma;
    }
};
```

---

## 思路

看了下大概的题意，只需要找到两根柱子短的那一根与两根柱子之间的长度组成的矩形最大值就OK了。

所以得到公式：`s = d*l`，`s`是面积，`d`是两者间距离，`l`是二柱更短的那根。

于是设立了用来储存最大`s`的变量`ma`，只需要指针`i`、`j`两头每次移动的时候算出`s`用来和`ma`来比较，最后更新`ma`即可。

需要注意的问题是，这道题如果直接暴力完成会超时，但我不想在算法层面上优化，于是进行了剪枝，增加了

```c++
if(i>0) if(height[i] <= l) continue;
```

和

```c++
if(height[j] <= l) continue;
```

两个判断的剪枝操作。

这个剪枝的思路来源于，随着二根柱子的推进，无论谁比之前的那根短柱短，那都不可能面积更大。因为`d`也在减小，所以只用计算新柱子大于`l`的情况。

---

## 优化

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int i = 0, j = height.size() - 1, res = 0;
        while(i < j) {
            res = height[i] < height[j] ? 
                max(res, (j - i) * height[i++]): 
                max(res, (j - i) * height[j--]); 
        }
        return res;
    }
};
```

作者：Krahets

链接：[点此跳转](https://leetcode.cn/problems/container-with-most-water/solutions/11491/container-with-most-water-shuang-zhi-zhen-fa-yi-do/)

这个优化思路来源于力扣上其他佬的题解，是纯正的双指针，比我写的简洁很多，并且少了层循环，是一种非常高效的思路。但需要求证：只移动短柱，面积可能增大；移动长柱，面积一定减小这一结论。

---

