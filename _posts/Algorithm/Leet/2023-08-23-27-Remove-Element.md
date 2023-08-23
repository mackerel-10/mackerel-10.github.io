---
layout: post
title: '27. Remove Element'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-23T16:56:00+09:00'
---

## 문제

[🔗 링크](https://leetcode.com/problems/remove-element/?envType=study-plan-v2&envId=top-interview-150)

> **Easy**
>
> Given an integer array nums and an integer val, remove all occurrences of val in nums **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)**. The order of the elements may be changed. Then return *the number of elements in* nums *which are not equal to* val.
> Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:
>
> - Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
> - Return k.

### 의사 코드

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
  const 필터링된 값 = val가 아닌 값만 필터링;
  const k = 필터링된 값의 길이;
  nums = 필터링된 값으로 수정

  return k;
};
```

### 내 코드

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function (nums, val) {
  const newNums = nums.filter((number) => number !== val);
  const k = newNums.length;
  newNums.forEach((num, index) => (nums[index] = num));
  // nums.splice(0, k, ...newNums);

  return k;
};
```

이전 문제처럼 복잡하지 않게 간단하게 생각했다. 처음에는 배열을 문자열로 바꾼후 `replace`를 이용해 `val`에 해당되는 값을 제거하려고 했는데, `filter` 함수가 존재하는 것을 기억했다.

`filter` 함수를 사용해 `val`가 아닌 값들을 필터링 해줬고, 생성된 새로운 배열의 길이를 `k`에 담고 `nums`를 새로운 배열로 수정해줬다. 뒤에 쓰레기값들은 신경쓰지 않아도 된다고 문제에 적혀있어, 넣어야할 값들만 0번 인덱스에 추가해주었다.

### 다른 해결법

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function (nums, val) {
  for (let i = 0; i < nums.length; i++) {
    if (val == nums[i]) {
      nums.splice(i, 1);
      i = -1;
    }
  }
  return nums.length;
};
```

`splice`를 통해 간단하게 제거하는 법이 있었다.

```javascript
function removeElement(nums, val) {
  // Counter for keeping track of elements other than val
  let count = 0;
  // Loop through all the elements of the array
  for (let i = 0; i < nums.length; i++) {
    // If the element is not val
    if (nums[i] !== val) {
      nums[count++] = nums[i];
    }
  }
  return count;
}
```

count라는 인덱스를 생성해 nums의 0번째 인덱스부터 val에 해당되지 않는 값을 넣는 방식이다. 내 코드는 `filter`와 `forEach`를 사용하기에 반복문이 2번 돌게 되는데 이 코드를 활용하면 한번의 반복문으로 해결할 수 있어서 더 좋은 풀이법인것 같다.

---

### Reference

- [LeetCode 27. Remove Element](https://sub2n.github.io/2019/04/16/LeetCode-27-Remove-Element/)
- [C++/JAVA/Python/JS \| 🚀 ✅ Fully Explained \| 🚀 ✅ Easy to Understand - Remove Element - LeetCode](https://leetcode.com/problems/remove-element/solutions/3416598/c-java-python-js-fully-explained-easy-to-understand/?envType=study-plan-v2&envId=top-interview-150#:~:text=%7D%0A%7D%3B-,JAVASCRIPT,-function%20removeElement%28)
