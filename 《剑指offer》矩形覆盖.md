我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
```java
public class Solution {
    public int RectCover(int target) {
        //每一个矩形都有如下情况：
        //1 如果竖着放，则与 target为 n-1 时相同；
        //2 如果横着放，则与 target 为 n-2 时相同。
        if(target <= 2){
            return target;
        }
        return RectCover(target-1) + RectCover(target-2);
    }
}
```
https://www.nowcoder.com/questionTerminal/72a5a919508a4251859fb2cfb987a0e6?answerType=1&f=discussion
```java
public class Solution {
    public int RectCover(int target) {
        if (target <= 2){
            return target;
        }
        int pre1 = 2; //初始化的时候，这是n=2块，随着i更新
        int pre2 = 1; //初始化的时候，这是n=1块，随着pre1更新
        for (int i = 3; i <= target; i++){
            int temp = pre1 + pre2;
            pre2 = pre1;
            pre1 = temp;
        }
        return pre1; //相对于 n+1 块来说，第 n 种的方法
    }
}
```
