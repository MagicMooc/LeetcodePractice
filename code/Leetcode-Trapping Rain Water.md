### [42\. Trapping Rain Water](https://leetcode-cn.com/problems/trapping-rain-water/)

Difficulty: **困难**


Given _n_ non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)  
<small style="display: inline;">The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. **Thanks Marcos** for contributing this image!</small>

**Example:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```


#### Solution

Language: **Java**

```java
/*
1.按列求
求每一列的水，我们只需要关注当前列，以及左边最高的墙，右边最高的墙就够了。
装水的多少，当然根据木桶效应，我们只需要看左边最高的墙和右边最高的墙中较矮的一个就够了。
所以，根据较矮的那个墙和当前列的墙的高度可以分为三种情况。
-较矮的墙的高度大于当前列的墙的高度：较矮的一边，也就是左边的墙的高度，减去当前列的高度就可以了
-较矮的墙的高度小于当前列的墙的高度：0
-较矮的墙的高度等于当前列的墙的高度：0
遍历每一列，然后分别求出这一列两边最高的墙。找出较矮的一端，和当前列的高度比较，结果就是上边的三种情况
O(n^2) O(1)
* */
class Solution {
	public int trap(int[] height) {
		int sum = 0;
		for (int i = 1; i < height.length-1; i++) {
			int max_left = 0;
			for (int j = i - 1; j >= 0; j--) {
				if (max_left < height[j]) {
					max_left = height[j];
				}
			}
			int max_right = 0;
			for (int k = i + 1; k < height.length; k++) {
				if (max_right < height[k]) {
					max_right = height[k];
				}
			}
			int min = Math.min(max_left, max_right);
			if (min > height[i]) {
				sum += min - height[i];
			}
		}
		return sum;
	}
}

/*
2.动态规划
对于每一列，我们求它左边最高的墙和右边最高的墙，都是重新遍历一遍所有高度，这里我们可以优化一下。
首先用两个数组，max_left [i] 代表第 i 列左边最高的墙的高度，max_right[i] 代表第 i 列右边最高的墙的高度。（一定要注意下，第 i 列左（右）边最高的墙，是不包括自身的，和 leetcode 上边的讲的有些不同）
对于 max_left我们其实可以这样求。
max_left [i] = Max(max_left [i-1],height[i-1])。它前边的墙的左边的最高高度和它前边的墙的高度选一个较大的，就是当前列左边最高的墙了。
对于 max_right我们可以这样求。
max_right[i] = Max(max_right[i+1],height[i+1]) 。它后边的墙的右边的最高高度和它后边的墙的高度选一个较大的，就是当前列右边最高的墙了。
这样，我们再利用解法二的算法，就不用在 for 循环里每次重新遍历一次求 max_left 和 max_right 了。
O(n) O(n)
* */
class Solution {
	public int trap(int[] height) {
		int sum = 0;
		int max_left[] = new int[height.length];
		int max_right[] = new int[height.length];
		for (int i = 1; i <= height.length - 2; i++) {
			max_left[i] = Math.max(max_left[i - 1], height[i - 1]);
		}
		for (int i = height.length - 2; i >= 1; i--) {
			max_right[i] = Math.max(max_right[i + 1], height[i + 1]);
		}
		for (int i = 0; i < height.length; i++) {
			int min = Math.min(max_left[i], max_right[i]);
			if (min > height[i]) {
				sum+=min-height[i];
			}
		}
		return sum;
	}
}

/*
3.双指针法
我们先明确几个变量的意思：

left_max：左边的最大值，它是从左往右遍历找到的
right_max：右边的最大值，它是从右往左遍历找到的
left：从左往右处理的当前下标
right：从右往左处理的当前下标
定理一：在某个位置i处，它能存的水，取决于它左右两边的最大值中较小的一个。
定理二：当我们从左往右处理到left下标时，左边的最大值left_max对它而言是可信的，但right_max对它而言是不可信的。
（见下图，由于中间状况未知，对于left下标而言，right_max未必就是它右边最大的值）
定理三：当我们从右往左处理到right下标时，右边的最大值right_max对它而言是可信的，但left_max对它而言是不可信的。

								   right_max
 left_max                             __
   __                                |  |
  |  |__   __??????????????????????  |  |
__|     |__|                       __|  |__
		left                      right
对于位置left而言，它左边最大值一定是left_max，右边最大值“大于等于”right_max，这时候，如果left_max<right_max成立，
那么它就知道自己能存多少水了。无论右边将来会不会出现更大的right_max，都不影响这个结果。
所以当left_max<right_max时，我们就希望去处理left下标，反之，我们希望去处理right下标。
O(n) O(1)

* */
class Solution {
	public int trap(int[] height) {
		int sum = 0;
		int left = 1;
		int right = height.length - 2;
		int max_left = 0;
		int max_right = 0;
		while (left <= right) {
			max_left = Math.max(max_left, height[left - 1]);
			max_right = Math.max(max_right, height[right + 1]);
			if (max_left < max_right) {
				if (max_left > height[left]) {
					sum += max_left - height[left];
				}
				left++;
			} else {
				if (max_right > height[right]) {
					sum += max_right - height[right];
				}
				right--;
			}
		}
		return sum;
	}
}

```