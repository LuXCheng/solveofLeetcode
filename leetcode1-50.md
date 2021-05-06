## 2. 两数相加
### **题目描述**
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
请你将两个数相加，并以相同形式返回一个表示和的链表。
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例 1：  
输入：l1 = [2,4,3], l2 = [5,6,4]  
输出：[7,0,8]  
解释：342 + 465 = 807.  

示例 2：  
输入：l1 = [0], l2 = [0]  
输出：[0]  

示例 3：  
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]  
输出：[8,9,9,9,0,0,0,1]  

提示：
每个链表中的节点数在范围 [1, 100] 内
0 <= Node.val <= 9
题目数据保证列表表示的数字不含前导零
[Leetcode2.两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
### **解题思路**
模拟题  
使用l1保存结果，需要注意的是当`l1.next=null && l2.next!=null`时，将l2.next赋给l1.next，以及在最后链表遍历完时，需要判断是否需要进位；
### **解题代码**
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int remainder=0;
        ListNode node=l1;
        while(l1!=null && l2!=null) {
            int num=l1.val+l2.val+remainder;
            l1.val=num%10;
            remainder=num/10;
            if(l1.next==null && l2.next!=null) {
                l1.next=l2.next;
                l1=l1.next;
                break;
            }
            if(l1.next==null && remainder!=0) {
                l1.next=new ListNode(remainder);
                return node;
            }
            l1=l1.next;
            l2=l2.next;
        }
        while(l1!=null){
            int num=l1.val+remainder;
            l1.val=num%10;
            remainder=num/10;
            if(l1.next==null && remainder!=0) {
                l1.next=new ListNode(remainder);
                break;
            }
            l1=l1.next;
        }
        return node;
    }
}
```
