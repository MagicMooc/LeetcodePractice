### [189\. Rotate Array](https://leetcode-cn.com/problems/rotate-array/)

Difficulty: **简单**


Given an array, rotate the array to the right by _k_ steps, where _k_ is non-negative.

**Follow up:**

*   Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
*   Could you do it in-place with O(1) extra space?

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Example 2:**

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

**Constraints:**

*   `1 <= nums.length <= 2 * 10^4`
*   It's guaranteed that `nums[i]` fits in a 32 bit-signed integer.
*   `k >= 0`


#### Solution

Language: **Java**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int temp, prevoius;
        for (int i = 0; i < k; i++) {
            prevoius = nums[nums.length - 1];
            for (int j = 0; j < nums.length; j++) {
                temp = nums[j];
                nums[j] = prevoius;
                prevoius = temp;
            }
        }
    }
}
//Approach 2: Using Extra Array
class Solution {
    public void rotate(int[] nums, int k) {
        int[] a = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            a[(i + k) % nums.length] = nums[i];
        }
        for (int j = 0; j < nums.length; j++) {
            nums[j] = a[j];
        }
    }
}

//Approach 3: Using Cyclic Replacements
/*环状替代和我的思路一致，不过我觉得这样解释可能更容易理解。
把元素看做同学，把下标看做座位，大家换座位。
第一个同学离开座位去第k+1个座位，第k+1个座位的同学被挤出去了，他就去坐他后k个座位，如此反复。
但是会出现一种情况，就是其中一个同学被挤开之后，坐到了第一个同学的位置（空位置，没人被挤出来），
但是此时还有人没有调换位置，这样就顺着让第二个同学换位置。
那么什么时候就可以保证每个同学都换完了呢？n个同学，换n次，所以用一个count来计数即可。
 */
class Solution {
	public void rotate(int[] nums, int k) {
		k = k % nums.length;
		int count = 0;
		for (int start = 0; count < nums.length; start++) {
			int current = start;
			int prev = nums[start];
			do {
				int next = (current + k) % nums.length;//找到下一个座位位置
				int temp = nums[next];//找角落
				nums[next] = prev;//坐下
				current = next;//角落的人为下一次找座位做准备
				prev = temp;
				count++;//计数
			} while (start != current);
		}
	}
}

//Approach 4: Using Reverse
class Solution {
	public void rotate(int[] nums, int k) {
		k %= nums.length;
		reverse(nums, 0, nums.length - 1);
		reverse(nums, 0, k - 1);
		reverse(nums, k, nums.length - 1);
	}

	public void reverse(int[] nums, int start, int end) {
		while (start < end) {
			int temp = nums[start];
			nums[start] = nums[end];
			nums[end] = temp;
			start++;
			end--;
		}
	}
}
```