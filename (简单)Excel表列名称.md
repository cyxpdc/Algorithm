给定一个正整数，返回它在 Excel 表中相对应的列名称。

例如，

	1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    29 -> AC
    30 -> AD
    ...
    52 -> AZ
    53 -> BA
    54 -> BB
    55 -> BC
    ...
    78 -> BZ
    79 -> CA
    80 -> CB
    81 -> CC
    ...
    701 -> ZY
    702 -> ZZ
    703 -> AAA
    ...
    
示例 1:

输入: 1

输出: "A"

示例 2:

输入: 28

输出: "AB"

示例 3:

输入: 701

输出: "ZY"
>26进制转换，记得减1
```java
class Solution {
    public String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();
        for(;n > 0;n /= 26){
            n--;
            sb.insert(0, (char) (n % 26 + 'A'));
        }
        return sb.toString();
    }
}
```
