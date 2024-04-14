+++
title = '23.合并K个升序链表'
date = 2024-04-14T18:36:10+08:00
author = "Yotom"
description = "力扣hot100的一道题目"
tags = [
    "力扣",
    "hot100",
    "链表",
    "困难",

]
categories = [
    "学习记录",
    "算法",
    "力扣hot100",
]

+++

# 合并K个升序链表

合并2个升序链表的进阶版，其实更像是中等题。

---

## 题目链接

[23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

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
    ListNode* mergeTwoLists(ListNode* a, ListNode* b){
        if(!a && !b) return nullptr;
        ListNode head;
        ListNode *p = &head;
        while(a && b){
            if(a->val < b->val){
                p->next = a;
                a = a->next;
            }
            else{
                p->next = b;
                b = b->next;
            }
            p = p->next;
        }
        if(a) p->next = a;
        else p->next = b;

        return head.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.empty()) return nullptr;
        for(int i=1;i<=lists.size()-1;i++){
        lists[i]=mergeTwoLists(lists[i],lists[i-1]);
}
        return lists[lists.size()-1];
    }
```

---

## 思路

根据两个链表的合并依次遍历链表并合并每次遍历的链表和已合并的链表。

---

## 优化

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
    ListNode* mergeTwoLists(ListNode* a, ListNode* b){
        if((!a)||(!b)) return a ? a : b;
        ListNode head;
        ListNode *p = &head;
        while(a && b){
            if(a->val < b->val){
                p->next = a;
                a = a->next;
            }
            else{
                p->next = b;
                b = b->next;
            }
            p = p->next;
        }
        if(a) p->next = a;
        else p->next = b;

        return head.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.empty()) return nullptr;
        return merge(lists, 0, lists.size()-1);
    }
    
    ListNode* merge(vector<ListNode*>& lists, int l, int r){
        if(l>r) return nullptr;
        if(l==r) return lists[l];
        int mid = (l+r)/2;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid+1, r));
    }
```

思路与之前一致，但是通过归并的思路进行合并每个升序链表。

---