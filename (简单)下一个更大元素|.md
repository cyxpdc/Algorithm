给定两个没有重复元素的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。

nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出-1。

>示例 1:

输入: nums1 = [4,1,2], nums2 = [1,3,4,2].

输出: [-1,3,-1]

解释:

    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
    
>示例 2:

输入: nums1 = [2,4], nums2 = [1,2,3,4].

输出: [3,-1]

解释:

    对于num1中的数字2，第二个数组中的下一个较大数字是3。
    
    对于num1中的数字4，第二个数组中没有下一个更大的数字，因此输出 -1。
    
注意:

nums1和nums2中所有元素是唯一的。

nums1和nums2 的数组大小都不超过1000。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        for(int i = 0;i < nums1.length;i++){
            boolean flag = false;
            for(int j = 0;j < nums2.length;j++){
                if(nums2[j] == nums1[i]){
                    for(int k = j + 1;k < nums2.length;k++){
                        if(nums2[k] > nums1[i]){//如果找到了，赋值即可
                            nums1[i] = nums2[k];
                            flag = true;
                            break;
                        }
                    }
                    if(flag == true){//如果为true，说明找到了第一个大数，不用查找nums2了
                        break;
                    }
                }
            }
            if(flag == false){//如果为false，说明没有找到可以赋值的数，即没有大数，所以为-1
                nums1[i] = -1;
            }
        }
        return nums1;
    }
}
```
>也可以用hashmap

>https://leetcode.com/problems/next-greater-element-i/discuss/269384/Use-2-HashMaps-Easy-Solution
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        if(nums1==null || nums2==null) return new int[0];
        Map<Integer,Integer> numbers=new HashMap<>();
        Map<Integer,Integer> indexes=new HashMap<>();
        List<Integer> res=new ArrayList<>();
        for(int j=0;j<nums2.length;j++)
        {
            numbers.put(nums2[j],j);
            indexes.put(j,nums2[j]);
        }
        
        for(int i=0;i<nums1.length;i++)
        {
            int currnum=nums1[i];
            if(!numbers.containsKey(currnum))
            {
                res.add(-1);
                continue;
            }
            int currindex=numbers.get(currnum);
            while(currindex < nums2.length-1)
            {
                currindex=currindex+1;
                if(indexes.get(currindex)>currnum)
                {
                    res.add(indexes.get(currindex));
                    break;
                }
                
            }
            if(res.size()<=i) res.add(-1);
        }
        int[] result=new int[res.size()];
        int k=0;
        for(int num:res)
        {
            result[k++]=num;
        }
        return result;
        
    }
}
```
>https://leetcode.com/problems/next-greater-element-i/discuss/265362/Java-solution-1-ms-faster-than-100-solutions-using-arrays
>简化的hashmap做法，快的一批
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] next_greatest = new int[10000];
        int[] final_array = new int[nums1.length];
        Arrays.fill(next_greatest, -1);
        //找到nums2中的下一个更大元素
        for (int i = 0;i < nums2.length;i++){
            for (int k = i+1;k < nums2.length;k++){
                if (nums2[i] < nums2[k]){
                    next_greatest[nums2[i]] = nums2[k];//[当前元素]=下一个更大元素
                    break;
                }
            }
        } 
        for (int j = 0;j < nums1.length;j++){
             final_array[j] = next_greatest[nums1[j]];
        }
        return final_array;
    }
} 
```
