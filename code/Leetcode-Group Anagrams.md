### [49\. Group Anagrams](https://leetcode-cn.com/problems/group-anagrams/)

Difficulty: **中等**


Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

*   All inputs will be in lowercase.
*   The order of your output does not matter.


#### Solution

Language: **Java**

```java
/*
1.排序数组分类
思路:
当且仅当它们的排序字符串相等时，两个字符串是字母异位词。
算法:
维护一个映射 ans : {String -> List}，其中每个键K 是一个排序字符串，每个值是初始输入的字符串列表，排序后等于K。
O(N*KlogK):N为strs长度，K为strs中字符串的最大长度，其中O(KlogK)为循环内部对每个字符串排序
O(N*K)
* */
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) return new ArrayList<>();
        Map<String,List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] c = s.toCharArray();
            Arrays.sort(c);
            String keyStr = String.valueOf(c);
            if (!map.containsKey(keyStr)) {
                map.put(keyStr,new ArrayList<>());
            }
            map.get(keyStr).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
/*
2.Instead of sorting, we can also build the key string in this way. 按单词中字母出现频率形成key
**/
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return new ArrayList<>();
        }
        Map<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] count = new char[26];
            for (char c : s.toCharArray()) {
                count[c - 'a']++;
            }
            String keyWord = String.valueOf(count);
            if (!map.containsKey(keyWord)) {
                map.put(keyWord,new ArrayList<>());
            }
            map.get(keyWord).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```