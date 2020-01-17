## leetcode_everyday
### 1.12-leecode54_螺旋矩阵

思路：定义矩阵上的四个方向点，left、right、top、bottom，按照题中的方式走一遍外圈（每圈都是向右->向下->向左->向上），每走一圈，每个方向点向内圈缩小一度，然后根据特点进行这四个方向点的增减，循环即可，将每圈的结果存入一个list中

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix==null || matrix.length ==0 ||matrix[0].length ==0)
        {
            return new LinkedList<>();
        }
        List<Integer> list = new LinkedList<>();
        int left=0;
        int right=matrix[0].length-1;
        int bottom=matrix.length-1;
        int top=0;
        while(left<=right && top<=bottom){
            for(int i=left;i<=right;i++){
                list.add(matrix[top][i]);
            }
            top++;
            for(int i=top;i<=bottom;i++){
                list.add(matrix[i][right]);
            }
            right--;
            for(int i=right;i>=left && top<=bottom;i--){
                list.add(matrix[bottom][i]);
            }
            bottom--;
            for(int i=bottom;i>=top && left<=right;i--){
                list.add(matrix[i][left]);
            }
            left++;
        }
        return list;
    }
}
```
### 1.14-leetcode55_跳跃游戏

思路：注意题中要求每个位置代表跳跃的最大长度，所以思路从前往后很难实现，故逆向思维，判断最后的每一步能否到达数组最后，从后往前进行跳跃，如果当前值+索引值大于数组的size的话（即到这一步就能跳到最后），让size等于当前的索引值，循环整个数组

循环结束后，判断size的大小，若size==0，即最后只剩下一个元素，则整个数组都可跳到最后，否则就必然跳不到

```java
class Solution {
    public boolean canJump(int[] nums) {
        int size=nums.length-1;
        if(nums.length==0){
            return false;
        }
        for(int i=nums.length-2;i>=0;i--){
            if(nums[i]+i>=size){
                size =i;
            }
            
        }
        if(size==0){
            return true;
        }
        else return false;
    }
}
```
### 1.14-leetcode55_最后一个单词的长度

思路：从右往左走，即可，注意先用trim函数删去字符串首尾的空格

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int count=0;
        s=s.trim();
        for(int i=s.length()-1;i>=0;i--){
            if(s.charAt(i) !=' '){
                count++;
            }
            else{
                break;
            }
        }
        return count;
    }
}
```
### 1-15-leetcode60_第K个排列

思路：因为按大小排列，可以根据n和k分别确定每一位的值，找出一个循环条件，即可

tips：学会利用StringBuilder来存储string

​			学会使用List中的ArrayList来存储和增删改查链表

​			(int)Math.ceil(double)的强制类型转化

```java
class Solution {
    public String getPermutation(int n, int k) {
        StringBuilder sb = new StringBuilder();
        List<Integer> list = new ArrayList<>();
        for(int i=1;i<=n;i++){
            list.add(i);
        }
        int fac=1,p=n;
        while(p!=0){
            fac *=p;
            p=p-1;
        }
        int i=0;
        int r=1;
        int m=fac/n;
        while((n-i)!=0 ){
            r=(int)Math.ceil((double)k/(double)m);
            int res =list.get(r-1);
            sb.append(res);
            list.remove(r-1);
            k=k-(r-1)*m;
            i++;
            m=(int)Math.ceil((double)m/(double)(n-i));
            
        }
        return sb.toString();

    }
}
```

### 1-15-leetcode61_旋转链表

思路：根据题中特点，统计一遍链表中节点的个数，然后把单链表转化为循环链表(即tail.next=head)，然后数到倒数第k个节点，然后分别设置头和尾即可

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null || head.next==null || k==0) return head;
        ListNode p=head;
        int count=1;
        while(p.next!=null){
            count++;
            p=p.next;
        }
        k %=count;
        if(k==0) return head;
        p.next=head;
        for(int i=0;i<count-k;i++){
            p=p.next;
        }
        ListNode newHead=p.next;
        p.next=null;
        return newHead;
    }
}
```
### 1-15-leetcode62_不同路径

思路：基本动态规划问题，找出动态dp方程，求出每个点的最大路径数

```java
class Solution {
    public int uniquePaths(int m, int n) {
        /**
        * 基本动态规划问题，将复杂问题化简成一个个简单的子问题
        * dp[i][j]表示到i，j的路数
        * 上和左边界均为1，其他等于左一个和上一个之和
        */
        int [][] dp = new int[m][n];
        for(int i=0;i<m;i++){
            dp[i][0]=1;
        }
        for(int j=0;j<n;j++){
            dp[0][j]=1;
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];

    }
}
```
### 1-16-leetcode67_二进制求和

思路：二进制相加问题为：每个位数对应相加：0+0=0，1+0=1，1+1=0但进位1

根据以上规则，将输入的两个字符串先把长度定为一致，短的那个在前面补0即可，然后对应相加（字符串转为数字即s.charAt(i)-'0'），用count代表进位，最后将结果反转后转化为字符串即可

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder sb = new StringBuilder();
        if(a.length()<b.length()){
            String tmp=a;
            a=b;
            b=tmp;
        }
        int l = a.length()-b.length();
        for(int i=0;i<l;i++){
            b = '0'+b;
        }
        int count=0;
        for(int i=a.length()-1;i>=0;i--){
           int sum=count;
           sum +=(a.charAt(i)-'0');
           sum +=(b.charAt(i)-'0');
           if(sum==0){
            sb.append(0);
            count=0;
           } 
           else if(sum==1){
             sb.append(1);
             count=0;
           } 
           else if(sum==2){
               sb.append(0);
                count=1;
           }
           else{
               sb.append(1);
               count=1;
           }
        }
        if(count!=0){
            sb.append(1);
        }
        return sb.reverse().toString();
    }
}
```
### 1-16-leetcode-71_简化路径

思路：用栈，先将String按“/"分割，然后遇到".."出栈，遇到路径的话进栈

tips：

- ” “表示字符串，’ ‘表示char，两者不能比较的
- 将字符串按照”/“ 分割成字符串数组 String[] a =path.split("/")
- 字符串数组的比较中==和equals是完全不同的
  - ==指地址相同，equals指值相同

故此题要用equals

```java
class Solution {
    public String simplifyPath(String path) {
        String[] a = path.split("/");
        StringBuilder sb = new StringBuilder();
        Stack<String> stack = new Stack<>();
        for(int i=0;i<a.length;i++){
            if(!a[i].equals("..") && !a[i].equals(".") && !a[i].equals("")){
                stack.push(a[i]);
            }
            else if(!stack.isEmpty() && a[i].equals("..")){
                stack.pop();
            }
        }
        if(stack.isEmpty()){
            return "/";
        }
        else{
             for(int i=0;i<stack.size();i++){
            sb.append("/"+stack.get(i));
        }
        return sb.toString();
        }
       
    }
}
```
### 1-17-leetcode72_编辑距离

思路：动态规划问题，看代码注释

tips: Math.min()只能比较两个值的大小，若多个需要挑最小，则需要再加一个Min

- 找边界
- 找dp条件(根据前一个)
- 求出所有的dp[i][j]

```java
class Solution {
    public int minDistance(String word1, String word2) {
        /** 
        动态规划，dp[i][j]：表示word1和word2的前i和前j个的操作次数
        将结果转化为求dp[i-1][j]、dp[i][j-1]、dp[i-1][j-1]的操作次数
        dp[i][j]=dp[i-1][j]+1
        dp[i][j]=dp[i][j-1]+1
        dp[i][j]=dp[i-1][j-1](当第i位和第j位不等时，需要+1)
        */
        int m=word1.length();
        int n=word2.length();
        int[][] dp=new int[m+1][n+1];
        //边界初始化
        for(int i=0;i<m+1;i++){
            dp[i][0]=i;
        }
        for(int j=0;j<n+1;j++){
            dp[0][j]=j;
        }
        int prior_ij=0;
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                int prior_i = dp[i-1][j]+1;
                int prior_j = dp[i][j-1]+1;
                if(word1.charAt(i-1)!=word2.charAt(j-1)){
                     prior_ij = dp[i-1][j-1]+1;
                }
                else  prior_ij=dp[i-1][j-1];
                dp[i][j]=Math.min(prior_i,Math.min(prior_j,prior_ij));
            }
        }
        return dp[m][n];

    }
}
```











