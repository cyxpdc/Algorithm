报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

1.     1
2.     11
3.     21
4.     1211
5.     111221
1 被读作  "one 1"  ("一个一") , 即 11。
11 被读作 "two 1s" ("两个一"）, 即 21。
21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。

给定一个正整数 n（1 ≤ n ≤ 30），输出报数序列的第 n 项。

注意：整数顺序将表示为一个字符串。

 

示例 1:

输入: 1

输出: "1"


示例 2:

输入: 4

输出: "1211"




>思路：cur遇到连续的数num，次数记录在count里，遇到不连续的后，res += count个num后，count置为0，继续cur

```java
class Solution {
    public String countAndSay(int n) {
        if(n == 1){
            return "1";
        }
        String str = countAndSay(n-1);//黑盒
        char[] arr = str.toCharArray();//转换为字符数组
        String res = "";
        int count = 0;
        int cur = 0;//当前位置
        int pre = 0;//第一个为某个数的位置
        while(cur < arr.length){
            if(arr[cur] == arr[pre]){
                count++;//次数加1
                cur++;//右移
            }
            else{
                res += String.valueOf(count) +   
                    String.valueOf(Integer.parseInt(String.valueOf(arr[pre])));
                count = 0;
                pre = cur;
            }
        }
        //最后cur位于最后一个位置，跳出循环，所以需要计算这个位置及其之前相同的数
        res += String.valueOf(count) +   
                    String.valueOf(Integer.parseInt(String.valueOf(arr[pre])));
        return res;//解黑盒
    }
}
```
Discuss：

>得到第一个，去求第二个，再根据第二个去求第三个，依次类推，跟我的做法思路一样，但是改为了迭代形式，速度快了很多
```java
public class Solution {
    public String countAndSay(int n) {
        String s = "1";
        for(int i = 1; i < n; i++){
            s = countIdx(s);
        }
        return s;
    }
    
    public String countIdx(String s){
        StringBuilder sb = new StringBuilder();
        char c = s.charAt(0);
        int count = 1;
        for(int i = 1; i < s.length(); i++){
            if(s.charAt(i) == c){
                count++;
            }
            else
            {
                sb.append(count);
                sb.append(c);
                c = s.charAt(i);
                count = 1;
            }
        }
        sb.append(count);
        sb.append(c);
        return sb.toString();
    }
}
```
