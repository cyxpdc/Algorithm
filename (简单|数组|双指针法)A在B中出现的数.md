两个数组，判断数组A的数出现在B中有几个



示例:

A:{1,3,4,5,6}

B:{0,3,3,4,6,,6,7}

输出:5



思路:

双指针法:

先对A、B排序，再用两个指针，分别为A和B的，依次比较

如果B小于A，B右移

如果B等于A，B右移且计数器加1

如果B大于A，A右移

```java
public int nums(int[] A,int[] B){
    int num = 0;
    int a = 0,b = 0;
    while(a < A.length || b < B.length){
        if(B[b] < A[a]){
            b++;
        }else if(B[b] == A[a]){
            num++;
            b++;
        }else{
            a++;
        }
    }
    return num;
}
```



