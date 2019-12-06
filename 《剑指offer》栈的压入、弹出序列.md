```java
import java.util.*;
//用for依次压入pushA，每个数都判断是否跟popA指向的当前数相同，是则pushA的这个数可以不入栈，且popA的指针右移一位
//pushA结束后，用while来判断栈是否为空，不为空则依次弹出，与popA的指针依次比较，如果相同，则继续，直到popA结束，返回true，否则false
public class Solution {
    
    private ArrayDeque<Integer> stack = new ArrayDeque<>();
    
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        int curPop = 0;        
        for(int i = 0;i < pushA.length;i++){
            if(pushA[i] == popA[curPop]){
                curPop++;
                if(curPop == popA.length){
                    return true;
                }
            }
            else{
                stack.addFirst(pushA[i]);    
            }
        }
        while(stack.size() != 0){
            if(popA[curPop] != stack.removeFirst()){
                return false;
            }
            curPop++;
        }
        return true;
    }
}
```
大佬的代码就是短：https://www.nowcoder.com/questionTerminal/d77d11405cc7470d82554cb392585106?answerType=1&f=discussion
```java
import java.util.Stack;
public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        int len = pushA.length;
        Stack<Integer> s = new Stack<Integer>();
 
        for(int i=0, j=0;  i < len; i++){
            s.push(pushA[i]);
            while(j < len && s.peek() == popA[j]){
                s.pop();
                j = j+1;
            }
        }
        return s.empty();
    }
}
```
