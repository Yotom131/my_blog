+++

title = '82. 删除排序链表中的重复元素 II'
date = 2024-04-14T21:37:10+08:00
author = "Yotom"
description = "力扣hot100的一道题目"
tags = [
    "力扣",
    "hot100",
    "链表",
    "中等",

]
categories = [
    "学习记录",
    "算法",
    "力扣hot100",
]

+++

# 删除排序链表中的重复元素 II

---

## 题目链接

[82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

---

## 解题代码

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) {
            return head;
        }
        
        ListNode* dummy = new ListNode(0, head);

        ListNode* cur = dummy;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int x = cur->next->val;
                while (cur->next && cur->next->val == x) {
                    cur->next = cur->next->next;
                }
            }
            else {
                cur = cur->next;
            }
        }

        return dummy->next;
    }
};

```

---

## 思路

这是官方解，通过设置一个哑结点，然后用一个指针依次遍历，比较下一个和下辖一个是否相同，如果相同则跳过下一个，直到跳过所有相同的。

---