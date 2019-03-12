写一个程序，输出从 1 到 n 数字的字符串表示。

1. 如果 n 是3的倍数，输出“Fizz”；

2. 如果 n 是5的倍数，输出“Buzz”；

3.如果 n 同时是3和5的倍数，输出 “FizzBuzz”。

示例：

n = 15,

返回:

[

    "1",
    
    "2",
    
    "Fizz",
    
    "4",
    
    "Buzz",
    
    "Fizz",
    
    "7",
    
    "8",
    
    "Fizz",
    
    "Buzz",
    
    "11",
    
    "Fizz",
    
    "13",
    
    "14",
    
    "FizzBuzz"
    
]
>简单遍历判断即可，注意分支顺序
```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> resList = new ArrayList<>();
        for(int i = 1;i <= n;i++){
            if(i % 3 == 0 && i % 5 == 0){
                resList.add("FizzBuzz");
            }else if(i % 3 == 0){
                resList.add("Fizz");
            }else if( i % 5 == 0){
                resList.add("Buzz");
            }else{
                resList.add(Integer.toString(i));
            }
        }
        return list;
    }
}
```
