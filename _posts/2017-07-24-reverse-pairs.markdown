---
layout: post
title: "Reverse Pairs"
date: 2017-07-24 10:25:03 +0800
categories: interview
---
题目地址：[https://leetcode.com/problems/reverse-pairs/](https://leetcode.com/problems/reverse-pairs/)

**Description**

Given an array ```nums```, we call ```(i, j)``` an <i>**important reverse pair**</i> if ```i < j``` and ```nums[i] > 2*nums[j]```.

You need to return the number of important reverse pairs in the given array.

Example1:
```
Input: [1,3,2,3,1]
Output: 2
```
Example2:
```
Input: [2,4,3,5,1]
Output: 3
```

**Note:**

- The length of the given array will not exceed 50,000.
- All the numbers in the input array are in the range of 32-bit integer.

**Solution**

Brute force (Time Limit Exceeded)

```c++
int reversePairs(vector<int>& nums) {

  int count = 0;
  int n = nums.size();

  for(int j = 1; j < n; j++) {
    for(int i = 0; i < j; i++){
      if(nums[i] > 2LL * nums[j]) // 2LL
      count++;
    }
  }

  return count;
}
```

[BIT](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-indexed-trees/) (Accepted)

```c++
void update(vector<int>& BIT, int index) {

  while(index > 0) {
    BIT[index] += 1;
    index -= index&(-index);
  }
}

int query(vector<int>& BIT, int index) {

  int sums = 0;
  while(index < BIT.size()) {
    sums += BIT[index];
    index += index&(-index);
  }
  return sums;
}

/**
 * @param A an array
 * @return total of reverse pairs
 */
int reversePairs(vector<int>& nums) {

  int count = 0;
  long n = nums.size();
  vector<int> nums_copy(nums);
  vector<int> BIT(n + 1, 0);

  sort(nums_copy.begin(), nums_copy.end());

  for(int i = 0; i < n; i++) {
    int index = lower_bound(nums_copy.begin(), nums_copy.end(), nums[i] + 1) - nums_copy.begin() + 1;
    count += query(BIT, index);
    index = lower_bound(nums_copy.begin(), nums_copy.end(), nums[i]) - nums_copy.begin() + 1;
    update(BIT, index);
  }

  return count;
}
```
