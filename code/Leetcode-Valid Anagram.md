### [242\. Valid Anagram](https://leetcode-cn.com/problems/valid-anagram/description/)

Difficulty: **简单**


Given two strings _s_ and _t _, write a function to determine if _t_ is an anagram of _s_.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Note:**  
You may assume the string contains only lowercase alphabets.

**Follow up:**  
What if the inputs contain unicode characters? How would you adapt your solution to such case?


#### Solution

Language: **Java**

```java
//1.排序后比较
// O(NlogN)排序时间占主导
// O(1)空间取决于排序实现
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] sc = s.toCharArray();
        char[] tc = t.toCharArray();
        Arrays.sort(sc);
        Arrays.sort(tc);
        return Arrays.equals(sc,tc);
    }
}
/*
2.哈希映射
O(n) O(1)
进阶：
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

解答：
使用哈希表而不是固定大小的计数器。想象一下，分配一个大的数组来适应整个 Unicode 字符范围，这个范围可能超过 100万。哈希表是一种更通用的解决方案，可以适应任何字符范围。
* */
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for (int j = 0; j < 26; j++) {
            if (count[j] != 0) {
                return false;
            }
        }
        return true;
    }
}
```