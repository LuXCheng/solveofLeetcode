## 169. 多数元素
### **题目描述**
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1：  
输入：[3,2,3]  
输出：3  

示例 2：  
输入：[2,2,1,1,1,2,2]  
输出：2  

进阶：  
尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。
### **解题思路**
- 方法一
排序法  
将数组排序，则众数始终会在数组nums/2位置处；
- 方法二
原理不知......  
代码意思是设置一个value保存当前值，当count=0时，更新value；
遍历数组，当value=nums[i]时，count+1，value!=nums[i]时，count-1；
### **解题代码**
```java
class Solution {
    public int majorityElement(int[] nums) {
        int value = 0;
        int count = 0;
        for(int num:nums) {
            if(count==0) {
                value=num;
            }
            count += (value==num)?1:-1;
        }
        return value;
    }
}
```
------
## 171. Excel表列序号

### **题目描述**
给定一个Excel表格中的列名称，返回其相应的列序号。
例如，

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
示例 1:
输入: "A"
输出: 1

示例 2:
输入: "AB"
输出: 28

示例 3:
输入: "ZY"
输出: 701
### **解题思路**
不一样进制转换，26个字母相当于26进制，即基数为26；
先把最高位的位权
`pow(26,String.length()-1)`
计算出来，再遍历字符串，位权也随着除以26；
### **解题代码**
```java
class Solution {
    public int titleToNumber(String columnTitle) {
        int ans=0;
        int len=columnTitle.length();
        double pow=Math.pow(26,len-1);
        pow=(int) pow;
        for (int i = 0; i < len; i++) {
            int num=columnTitle.charAt(i)-'A'+1;
            ans+=(pow*num);
            pow/=26;
        }
        return ans;
    }
}
```
------
## 198. 打家劫舍
### **题目描述**
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你不触动警报装置的情况下，一夜之内能够偷窃到的最高金额。

示例 1：  
输入：[1,2,3,1]  
输出：4  
解释：偷窃 1 号房屋 (金额 = 1)，然后偷窃 3 号房屋(金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。

示例 2：  
输入：[2,7,9,3,1]  
输出：12  
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋(金额 = 9)，接着偷窃 5 号房屋(金额=1)。偷窃到的最高金额 = 2 + 9 + 1 = 12 。
 
提示：
1 <= nums.length <= 100
0 <= nums[i] <= 400

### **解题思路**
动态规划
第i号房间共有两种选择
>当偷窃i号房间时，只能选择i-2号房间之前的房间偷窃；
>当不偷窃i号房间时，可选择i-1之前房间偷窃；

由于只记录最大偷窃金额，即使用动态规划有`dp[i]=max(dp[i-2]+nums[i],dp[i-1])`dp[i]表示从0到i号房间盗窃的最大金额；

同时，当i=0时，dp[0]=nums[0],i=1时，dp[1]=max(nums[0],nums[1])，且后续遍历时只用到了dp[i-2]和dp[i-1]两个值，则只需设置两个变量用来保存即可，减少内存的使用；

### **解题代码**
```java
class Solution {
    public int rob(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        if(nums.length==1) return nums[0];

        int first=nums[0],second=Math.max(nums[0],nums[1]);

        int l= nums.length;
        for(int i = 2; i< l; i++) {
            int temp = Math.max(first+nums[i],second);
            first=second;
            second=temp;
        }
        return second;   
    }
}
```
