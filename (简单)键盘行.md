给定一个单词列表，只返回可以使用在键盘同一行的字母打印出来的单词。键盘如下图所示。

 

American keyboard

 

示例：

输入: ["Hello", "Alaska", "Dad", "Peace"]

输出: ["Alaska", "Dad"]
>https://leetcode.com/problems/keyboard-row/discuss/269355/0ms-100-speed-90-memory-Java-solution
```java
class Solution {
    public String[] findWords(String[] words) {
        //26个字母分别对应的行位置
        int[] rows = new int[]{2,3,3,2,1,2,2,2,1,2,2,2,3,3,1,1,1,1,2,1,1,3,1,3,1,3};
        List<String> result = new ArrayList<>();
        
        for(String word : words){
            String lowWord = word.toLowerCase();
            int row = rows[lowWord.charAt(0) - 'a'];
            boolean flag = true;
            for(int i = 1; i < word.length() ; i++){
                //若不同行，则结束此单词的判断
                if(rows[lowWord.charAt(i) - 'a'] != row){
                    flag = false;
                    break;
                }
            }
             if(flag){
                 result.add(word);
             }
            
        }
        return result.toArray(new String[result.size()]);
    }
}
```
