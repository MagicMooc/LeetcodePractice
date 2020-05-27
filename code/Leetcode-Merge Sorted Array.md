### [88\. Merge Sorted Array](https://leetcode-cn.com/problems/merge-sorted-array/)

Difficulty: **简单**


Given two sorted integer arrays _nums1_ and _nums2_, merge _nums2_ into _nums1_ as one sorted array.

**Note:**

*   The number of elements initialized in _nums1_ and _nums2_ are _m_ and _n_ respectively.
*   You may assume that _nums1_ has enough space (size that is greater or equal to _m_ + _n_) to hold additional elements from _nums2_.

**Example:**

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```


#### Solution

Language: **Java**

```java
/*1.合并后排序
-public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
-public static void sort(int[] a)
Sorts the specified array into ascending numerical order.
(Implementation note: The sorting algorithm is a Dual-Pivot Quicksort by Vladimir Yaroslavskiy, Jon Bentley, and Joshua Bloch.
This algorithm offers O(n log(n)) performance on many data sets that cause other quicksorts to degrade to quadratic performance,
 and is typically faster than traditional (one-pivot) Quicksort implementations.)
*/
class Solution {
	public void merge(int[] nums1, int m, int[] nums2, int n) {
		System.arraycopy(nums2,0,nums1,m,n);
		Arrays.sort(nums1);
	}
}
/*时间复杂度 : O((n+m)log(n+m))。
空间复杂度 : O(1)。
*/
//2.双指针/从前往后
//时间复杂度:O(m+n)
//空间复杂度:O(m)
class Solution {
	public void merge(int[] nums1, int m, int[] nums2, int n) {
		int[] nums1_copy = new int[m];
		System.arraycopy(nums1, 0, nums1_copy, 0, m);
		int p1 = 0;
		int p2 = 0;
		int p = 0;
		while (p1 < m && p2 < n) {
			nums1[p++] = (nums1_copy[p1] < nums2[p2]) ? nums1_copy[p1++] : nums2[p2++];
		}
		if (p1 < m) {
			System.arraycopy(nums1_copy, p1, nums1, p1 + p2, m + n - p1 - p2);
		}
		if (p2 < n) {
			System.arraycopy(nums2, p2, nums1, p1 + p2, m + n - p1 - p2);
		}
	}
}
/*
3.双指针/从后往前
时间复杂度：O(m+n)
空间复杂度：O(1)
*/
class Solution {
	public void merge(int[] nums1, int m, int[] nums2, int n) {
		int p1 = m - 1;
		int p2 = n - 1;
		int p = m + n - 1;
		while (p1 >= 0 && p2 >= 0) {
			nums1[p--] = (nums1[p1] > nums2[p2]) ? nums1[p1--] : nums2[p2--];
		}
		System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
	}
}
```