实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

示例 1:

输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:

输入: haystack = "aaaaa", needle = "bba"
输出: -1
说明:

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。


KMP：
```java
//KMP
class Solution {
    public int strStr(String s, String m) {
        if(m.length() <= 0){
            return 0;
        }
        if(s.length() <= 0){
            return -1;
        }
		char[] str1 = s.toCharArray();
		char[] str2 = m.toCharArray();
		int[] next = getNextArray(str2);
		int i1 = 0,i2 = 0;//两个指针
		while(i1 < str1.length && i2 < str2.length){
			if(str1[i1] == str2[i2]){
				i1++;
				i2++;
			}
			//两个else都是不等的情况
			else if(next[i2] == -1){//为-1，说明i2是第一个位置,即i1到下一位后，跟str2的第一个位置比较
				i1++;
			}
			else{
				i2 = next[i2];
			}
		}
		return i2 == str2.length ? i1 - i2 : -1;
    }
    
    public static int[] getNextArray(char[] str2) {
		if(str2.length == 1){
			return new int[]{-1};
		}
		int[] next = new int[str2.length];
		next[0] = -1;
		next[1] = 0;
		int i = 2;
		int cn = 0;//跳转的位置
		while(i < next.length){
			if(str2[i-1] == str2[cn]){
				next[i++] = ++cn;
			}
			//可以就往前跳，不行就等于0
			else if(cn > 0){
				cn = next[cn];
			}
			else{//cn<=0
				next[i++] = 0;
			}
		}
		return next;
	}
}
```

讨论区:

haystack中每needle个字符进行比较
```java
class Solution {
    public int strStr(String haystack, String needle) {
        for(int i=0;i<= haystack.length()-needle.length();i++){
            if(haystack.substring(i, i + needle.length() ).equals(needle)){
                return i;
            }
        }
        return -1;
    }
}
```
