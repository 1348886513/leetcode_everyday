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




