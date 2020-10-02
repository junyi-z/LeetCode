## LCP 19. 秋叶收藏集

1. 动态规划，`dp[i][j]`二维数组中，第一列的状态表示字符串leaves中对于第0片到第i片叶子进行调整操作，使得第i片叶子处于状态j的最小操作次数，用0，1，2表示红黄红三种状态，

2. 当j是0的时候，需要把叶子转为第一种红，那么他之前的所有叶子也必须是处于j=0，即第一种红的状态，它只与之前的处于`j = 0`的叶子有关系，不需要考虑处于1或者2的叶子，此时的状态转移方程是：`dp[i][0] = dp[i-1][0] + isYellow(i)`，影响此时的操作数的值的因素只有当前第i片叶子是否是黄色；

3. 当j是1的时候，需要把叶子转为黄色，那么之前的叶子可以是第一种红，也可以是黄色，只需要取最小的数字即可，状态转移方程：`dp[i][1] = min(dp[i-1][0], dp[i-1][1]) + isRed(i)`

4. 当j是2的时候，需要把当前叶子转为红色，那么之前的叶子可以是黄色，可以是第二种红色，但是绝对不能是第一种红色，因为最终的叶子是必须是红黄红三块内容，所以状态转移方程是：`dp[i][2] = min(dp[i-1][1], dp[i-1][2]) + isYellow(i)`，需要注意的是，此时叶子数量必须是大于等于3的，因为每一种叶子必须至少有一个；

5. `isYellow(i), isRed(i)` 是用来表示当前的叶子是什么颜色，对于状态0，2来说，如果是黄色，说明需要操作一次，即操作数+1，对于状态1来说，如果是红色，也需要操作一次；

6. 另外注意，第一次`dp[0][0]`的值由leaves字符串的第一位字符确定，

7. 时间复杂度是`O(n)`，字符串的长度，空间复杂度是`0(n)`，二维数组的大小，

8. ```java
   class Solution {
       public int minimumOperations(String leaves) {
           int len = leaves.length();
           int[][] dp = new int[len][3];
           dp[0][0] = leaves.charAt(0) == 'y' ? 1 : 0;
           dp[0][1] = dp[0][2] = dp[1][2] = Integer.MAX_VALUE;
           for(int i = 1; i < len; i++){
               int isYellow = leaves.charAt(i) == 'y'? 1 : 0;
               int isRed = leaves.charAt(i) == 'r'? 1 : 0;
               dp[i][0] = dp[i - 1][0] + isYellow;
               dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][1]) + isRed;
               if(i > 1){
                   dp[i][2] = Math.min(dp[i - 1][1], dp[i - 1][2]) + isYellow;
               }
           }
           return dp[len - 1][2];
       }
   }
   ```

   