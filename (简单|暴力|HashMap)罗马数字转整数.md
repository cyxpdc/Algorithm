罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

    字符          数值
    I             1
    V             5
    X             10
    L             50
    C             100
    D             500
    M             1000

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

- I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
- X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
- C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

示例 1:

    输入: "III"
    输出: 3

示例 2:

    输入: "IV"
    输出: 4

示例 3:

    输入: "IX"
    输出: 9

示例 4:

    输入: "LVIII"
    输出: 58
    解释: L = 50, V= 5, III = 3.

示例 5:

    输入: "MCMXCIV"
    输出: 1994
    解释: M = 1000, CM = 900, XC = 90, IV = 4.


思路：暴力解法，不断地做判断即可(代码难看，下面有比较好的)
```java
class Solution {
    public int romanToInt(String s) {
        int num = 0;
        char[] arr = s.toCharArray();
        for(int i = 0;i < s.length();i++){
            //六种特殊情况
            if(arr[i] == 'I'){
                if(i == arr.length - 1){//如果到了最后一个，直接加，不用判断
                    num += 1;
                }
                else if(arr[i+1] == 'V'){
                    num += 4;
                    i++;//其下一个i也不用循环了
                }
                else if(arr[i+1] == 'X'){
                    num += 9;
                    i++;
                }
                else{//正常加
                    num += 1;
                }
            }
            else if(arr[i] == 'X'){
                if(i == arr.length - 1){
                    num += 10;
                }
                else if(arr[i+1] == 'L'){
                    num += 40;
                    i++;
                }
                else if(arr[i+1] == 'C'){
                    num += 90;
                    i++;
                }
                else{
                    num += 10;   
                }
            }
            else if(arr[i] == 'C'){
                if(i == arr.length - 1){
                    num += 100;
                }
                else if(arr[i+1] == 'D'){
                    num += 400;
                    i++;
                }
                else if(arr[i+1] == 'M'){
                    num += 900;
                    i++;
                }
                else{
                    num += 100;    
                }
            }
            //常规情况
            else if(arr[i] == 'V'){
                num += 5;
            }
            else if(arr[i] == 'L'){
                num += 50;
            }
            else if(arr[i] == 'D'){
                num += 500;
            }
            else if(arr[i] == 'M'){
                num += 1000;
            }
        }
        return num;
    }
}
```

或者利用题目的思路，使用map:参考(https://blog.csdn.net/hit1110310422/article/details/80865772)
```java
//1、相同的数字连写，所表示的数等于这些数字相加得到的数，如：Ⅲ = 3； 
//2、小的数字在大的数字的右边，所表示的数等于这些数字相加得到的数， 如：Ⅷ = 8；Ⅻ = 12； 
//3、小的数字在大的数字的左边，所表示的数等于大数减小数得到的数，如：Ⅳ= 4；Ⅸ= 9；
//原版:很慢
class Solution {
    public int romanToInt(String s) {
        int num = 0;
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        for (int i = 0; i < s.length(); ++i) {
            int val = map.get(s.charAt(i));
            //如果是最后一个，或者小的数字在大的数字的右边，与当前数相加
            if (i == s.length() - 1 || map.get(s.charAt(i + 1)) <= map.get(s.charAt(i))) {
                num += val;
            } 
            //小的数字在大的数字的左边，直接减掉当前数，效果等于大数减小数
            else {
                num -= val;
            }
        }
        return num;
    }
}
//稍微修改：挺快的
//相比在循环中s.charAt(i),不如直接在外面转换成char[] arr = s.toCharArray()
class Solution {
    public int romanToInt(String s) {
        char arr[] = s.toCharArray();
        int num = 0;
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        for (int i = 0; i < s.length(); ++i) {
            int val = map.get(arr[i]);
            if (i == s.length() - 1 || map.get(arr[i+1]) <= map.get(arr[i])) {
                num += val;
            } 
            else {
                num -= val;
            }
        }
        return num;
    }
}
```
