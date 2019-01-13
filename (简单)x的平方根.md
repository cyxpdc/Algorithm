实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

>示例 1:

输入: 4

输出: 2

>示例 2:

输入: 8

输出: 2

说明: 8 的平方根是 2.82842..., 

     由于返回类型是整数，小数部分将被舍去。



>res * res会越界，ac失败
```java
class Solution {
    public int mySqrt(int x) {
        int res = 0;
        while(res * res < x){
            res++;
        }
        if(res * res == x){
            return res;
        }
        else{
            return res - 1;
        }
    }
}
```
>修改一下，ac成功，不过速度很慢
```java
class Solution {
    public int mySqrt(int x) {
        int res = 1;
        while(x / res >= res){
            res++;
        }
        if(res * res == x){
            return res;
        }
        else{   
            return res - 1;
        }
    }
}
```
>Discuss：二分查找
```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0) return 0;
	    int start = 1, end = x;
	    while (start < end) { 
		   int mid = start + (end - start) / 2;
		   if (mid <= x / mid && (mid + 1) > x / (mid + 1))// Found the result
		    return mid; 
		   else if (mid > x / mid)// Keep checking the left part
			   end = mid;
		   else
			   start = mid + 1;// Keep checking the right part
	    }
	    return start;
    }
}
```
