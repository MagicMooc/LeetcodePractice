### [283\. Move Zeroes](https://leetcode-cn.com/problems/move-zeroes/)

Difficulty: **简单**


Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1.  You must do this **in-place** without making a copy of the array.
2.  Minimize the total number of operations.


#### Solution

Language: **Java**

```java
/*
1。边统计前面0的个数边将非零数字往前挪
*/
class Solution {
	public void moveZeroes(int[] nums) {
		int count = 0;
		for (int i = 0; i < nums.length; i++) {
			if (nums[i] == 0) {
				count++;
			} else {
				nums[i - count] = nums[i];
			}
		}
		for (int j = nums.length - count; j < nums.length; j++) {
			nums[j] = 0;
		}
	}
}

//2.引入插入指针，看到非零数字就进行插入
class Solution {
	public void moveZeroes(int[] nums) {
		int insertPos = 0;
		for (int num : nums) {
			if (num != 0) {
				nums[insertPos++] = num;
			}
		}
		while (insertPos < nums.length) {
			nums[insertPos++] = 0;
		}
	}
}


```