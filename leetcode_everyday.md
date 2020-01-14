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





