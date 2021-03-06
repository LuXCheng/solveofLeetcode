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
链表模拟题  
使用l1保存结果，需要注意的是当`l1.next=null && l2.next!=null`时，将l2.next赋给l1.next，以及在最后链表遍历完时，需要判断是否需要进位；
### **解题代码**
```java
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
------
## 22. 括号生成
### **题目描述**
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。  

示例 1：  
输入：n = 3  
输出：["((()))","(()())","(())()","()(())","()()()"]  

示例 2：  
输入：n = 1  
输出：["()"]  
 
提示：
1 <= n <= 8
### **解题思路**
**回溯法**  
**深度优先遍历**  
使用DFS，判断字符串后是否添加'('或')';  
由括号匹配的性质可以知道，只有当目前字符串左括号的数量小于n时，可添加左括号；当右括号数量小于左括号时，可添加右括号，所以我们可以设置两个变量，分别表示左、右括号的数量；
### **解题代码**
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> list= new ArrayList<>();
        getStr(list,new StringBuilder(),0,0,n);
        return list;
    }

    public void getStr(List<String> list, StringBuilder str, int left, int right, int len)    {
        if(str.length() == len*2) {
            list.add(str.toString());
            return;
        }
        if(left<len) {
            str.append("(");
            getStr(list, str, left+1, right, len);
            str.deleteCharAt(str.length()-1);
        }
        if(right<left) {
            str.append(")");
            getStr(list, str, left, right+1, len);
            str.deleteCharAt(str.length()-1);
        }
    }
}
```
------
## 24. 两两交换链表中的节点

### **题目描述**
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1：  
输入：head = [1,2,3,4]  
输出：[2,1,4,3]  

示例 2：  
输入：head = []  
输出：[]  

示例 3：  
输入：head = [1]  
输出：[1]  
 
提示：  
链表中节点的数目在范围 [0, 100] 内
0 <= Node.val <= 100

进阶：你能在不修改链表节点值的情况下解决这个问题吗?（也就是说，仅修改节点本身。）  
### **解题思路**
链表  
本题需要注意两个点
> - 判断遍历的条件  
> - 如何实现两节点交换  

1.只有当某节点的next和next.next存在时，才能够交换；  
2.两节点交换时最重要的是注意交换的第二节点后的节点需要连接的位置；
### **解题代码**
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode ansNode=new ListNode(0);
        ansNode.next=head;
        ListNode node=ansNode;
        while(node.next!=null && node.next.next!=null) {
            ListNode node1=node.next;
            ListNode node2=node.next.next;
            node1.next=node2.next;
            node.next=node2;
            node2.next=node1;
            node=node1;
        }
        return ansNode.next;
    }
}
```
------
## 31. 下一个排列
### **题目描述**
实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。  
如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。  
必须 原地 修改，只允许使用额外常数空间。  

示例 1：  
输入：nums = [1,2,3]  
输出：[1,3,2]  

示例 2：  
输入：nums = [3,2,1]  
输出：[1,2,3]  

示例 3：  
输入：nums = [1,1,5]  
输出：[1,5,1]  

示例 4：  
输入：nums = [1]  
输出：[1]  
 
提示：  
1 <= nums.length <= 100  
0 <= nums[i] <= 100  
### **解题思路**
把序列转换成数字来看，例如：  
1 2 3  
1 3 2  
2 1 3  
2 3 1  
3 1 2  
3 2 1  
即123,132,213,231,312,321，可以看到下一个“数”比前一个“数”大，则我们可以知道就是找下一个较大的“数”；  
可以从数位的降序、升序来看，且从最低位开始，当某一段降序时，就不能再这里面选择数使得“数”变大，只有选择前面没有在降序序列的第一个数，以及选择降序序列中第一个大于该数的数，交换位置，后续序列转换为升序；
例如找出`9 4 7 6 5 2 1`的下一个排列
交换4和5的位置，并将7~1按升序排列，结果为9 5 1 2 4 6 7；

### **解题代码**
```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i=nums.length-2;
        while(i>=0 && nums[i]>=nums[i+1]) {
            i--;
        }
        if(i>=0) {
            int j=nums.length-1;
            while(j>=0 && nums[i]>=nums[j]){
                j--;
            }
            int temp=nums[i];
            nums[i]=nums[j];
            nums[j]=temp;
        }
        int left=i+1;
        int right=nums.length-1;
        while(left<right) {
            int temp=nums[left];
            nums[left]=nums[right];
            nums[right]=temp;
            left++;
            right--;
        }
    }
}
```
------
## 39. 组合总和
### **题目描述**
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。  
candidates 中的数字可以无限制重复被选取。  

说明：  
所有数字（包括 target）都是正整数。  
解集不能包含重复的组合。   

示例 1：  
输入：candidates = [2,3,6,7], target = 7,  
所求解集为：[[7],[2,2,3]]  

示例 2：  
输入：candidates = [2,3,5], target = 8,  
所求解集为：[[2,2,2,2],[2,3,3],[3,5]]
 
提示：  
1 <= candidates.length <= 30  
1 <= candidates[i] <= 200  
candidate 中的每个元素都是独一无二的。  
1 <= target <= 500  
### **解题思路**
**深度优先搜索+剪枝**  
按照candidates从下标0开始向后搜索，如果当前candidates的数满足进入List<Integer>中，因为题目中写到每个数可以无限次使用，所以从当前下标开始继续向前搜索，而如果不从选择当前数，则选择下一个数，当idx>candidates.length时，回溯；
例如：  
```java
search(int idx){
    search(idx+1);
    if(target-candidates[idx]>=0){
        search(idx);
    }
}
```

### **解题代码**
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans=new ArrayList<List<Integer>>();
        List<Integer> sub=new ArrayList<>();
        //Arrays.sort(candidates);
        dfsCombinationSum(ans,sub,candidates,target,0);
        return ans;
    }

    public void dfsCombinationSum(List<List<Integer>> ans,List<Integer> sub,int[] candidates,int target,int idx) {
        if(idx==candidates.length) {
            return;
        }
        if(target==0) {
            ans.add(new ArrayList(sub));
            return;
        }

        dfsCombinationSum(ans,sub,candidates,target,idx+1);

        if(target-candidates[idx]>=0) {
            sub.add(candidates[idx]);
            dfsCombinationSum(ans, sub, candidates, target-candidates[idx], idx);
            sub.remove(sub.size()-1);
        }
    }
}
```
### **复杂度分析**

- 时间复杂度 O(l) l为结果的长度大小
- 空间复杂度 O(target) 主要为递归的深度

------
## 40. 组合总和 II

### **题目描述**
给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。  
candidates 中的每个数字在每个组合中只能使用一次。  

说明：  
所有数字（包括目标数）都是正整数。  
解集不能包含重复的组合。   

示例 1:  
输入: candidates = [10,1,2,7,6,1,5], target = 8,  
所求解集为:[[1, 7],[1, 2, 5],[2, 6],[1, 1, 6]]  

示例 2:  
输入: candidates = [2,5,2,1,2], target = 5,  
所求解集为:[[1,2,2],[5]]  
### **解题思路**
`39.组合总和I`的扩展；  
本题主要的是每个数只能使用一次，与39题的无限使用完全不同，能够最简单的想到的是先将candidates排序，才能避免数的重复使用；  
避免数的重复使用即可以想到假如当前idx下标数组的数满足条件，那么之后的相等的数就不能再使用，因为这一个数就已经把该位置的数的情况搜索完了；  
而从之前三数之和和四数之和经验，去除重复数使用while循环即可；  
### **解题代码**
```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> sub=new ArrayList<>();
        Arrays.sort(candidates);
        dfsCombinationSum2(ans,sub,candidates,target,0);
        return ans;
    }

    void dfsCombinationSum2(List<List<Integer>> ans,List<Integer> sub,int[] candidates,int target,int idx){
        if(target==0) {
            ans.add(new ArrayList<>(sub));
            return;
        }
        for(int i=idx;i<candidates.length;i++) {
            if(i>idx&&candidates[i]==candidates[i-1]) {
                continue;
            }
            if(target-candidates[i]>=0) {
                sub.add(candidates[i]);
                dfsCombinationSum2(ans,sub,candidates,target-candidates[i],i+1);
                sub.remove(sub.size()-1);
            }
        }
    }
}
```
