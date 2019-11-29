一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
```java
public class Solution {
    public int JumpFloor(int target) {
        if(target == 0 || target == 1 || target == 2){
            return target;
        }
        return JumpFloor(target - 1) + JumpFloor(target - 2);
    }
}
```
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
```java
public class Solution {
    public int JumpFloorII(int target) {
        if(target == 0){
            return 1;
        }
        if(target == 1 || target == 2){
            return target;
        }
        int res = 0;
        for(int i = 1;i <= target;i++){
            res += JumpFloorII(target - i);
        }
        return res;
    }
}
```
