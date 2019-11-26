请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

- 解析

逐个字符判断即可
```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        StringBuilder res = new StringBuilder();
        for(int curIndex = 0;curIndex < str.length();curIndex++){
            char curChar = str.charAt(curIndex);
            if(curChar == ' '){
                res.append("%20");
            }else{
                res.append(curChar);
            }
        }
        return res.toString();
    }
}
```
