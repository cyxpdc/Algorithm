输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分

并保证奇数和奇数，偶数和偶数之间的相对位置不变。
```java
public class Solution {
    public void reOrderArray(int [] array) {
        if(array.length <= 1){
            return ;
        }
        int odd = 0;
        for(;odd < array.length;odd++){
            if(array[odd] % 2 == 0){
                int even = odd + 1;
                while(even < array.length && array[even] % 2 == 0){
                    if(even == array.length - 1){
                        return ;
                    }
                    even++;
                }
                int temp = array[odd];
                array[odd] = array[even];
                for(int i = 0;i < even - odd;i++){
                    int temp1 = array[odd+i+1];
                    array[odd+i+1] = temp;
                    temp = temp1;
                }
                
            }
        }
        return ;
    }
}
```
