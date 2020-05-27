### [1\. Two Sum](https://leetcode-cn.com/problems/two-sum/)

Difficulty: **简单**


Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **_exactly_** one solution, and you may not use the _same_ element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```


#### Solution

Language: **Java**

```java
/*该题要求只需返回一组解
*/
//1.Brute Force
//时间复杂度O(n^2)
class Solution {
	public int[] twoSum(int[] nums, int target) {
		for (int i = 0; i < nums.length - 1; i++) {
			for (int j = i+1; j < nums.length; j++) {
				if (nums[i] + nums[j] == target) {
					return new int[]{i, j};
				}
			}
		}
		throw new IllegalArgumentException("No two sum solution");
	}
}

/*
* 2.两遍哈希表
* 在第一次迭代中，我们将每个元素的值和它的索引添加到表中。
* 然后，在第二次迭代中，我们将检查每个元素所对应的目标元素（target−nums[i]）是否存在于表中。
* 注意，该目标元素不能是nums[i] 本身！
* O(n):第二次循环内部哈希查找为O(1)
* O(n)
。*/
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            int another = target - nums[i];
            if (map.containsKey(another) && map.get(another) != i) {
                return new int[]{i,map.get(another)};
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}

/*3。一遍哈希
 * 事实证明，我们可以一次完成。
 * 在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。
 * 如果它存在，那我们已经找到了对应解，并立即将其返回。
 * O(n)
 * O(n)
 */
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int another = target - nums[i];
            if(map.containsKey(another)){
                return new int[]{map.get(another),i};
            }
            map.put(nums[i],i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```