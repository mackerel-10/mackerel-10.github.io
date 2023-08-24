---
layout: post
title: '26. Remove Duplicates from Sorted Array'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-23T17:34:00+09:00'
---

## 문제

[🔗 링크](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

> **Easy**
>
> Given an integer array nums sorted in **non-decreasing order**, remove the duplicates **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return *the number of unique elements in* nums.
> Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:
>
> - Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
> - Return k.

### 의사 코드

```javascript
var removeDuplicates = function(nums) {
  const 중복된 값이 제거된 배열 = 중복된 값 제거
  const k = 중복된 값이 제거된 배열의 길이
  nums에 중복된 값이 제거된 배열 삽입

  return k;
};
```

### 내 코드

```javascript
var removeDuplicates = function (nums) {
  const setNums = [...new Set(nums)];
  const k = setNums.length;
  nums.splice(0, 0, ...setNums);

  return k;
};
```

`Set` class를 이용해 중복된 값을 제거해주고 `setNums`란 배열을 생성했다. 이 배열의 길이를 `k`에 저장해주고, `splice`를 이용해 `nums` 배열을 수정해줬다.

### 다른 해결법

```javascript
var removeDuplicates = function (nums) {
  let i = 0;
  for (let j = 1; j < nums.length; j++) {
    // j는 1씩 증가
    if (nums[i] !== nums[j]) {
      // 값이 다른 경우 i 인덱스는 증가한다
      i++;
      nums[i] = nums[j];
    }
  }
  return i + 1; // i는 0부터 시작하기에 1을 더해주었다
};
```

Two Pointer Algorithm을 이용해 풀이하였다. 분명 앞에서 배웠는데, 적용할 생각을 못했다. 위 코드는 `i`와 `j`라는 변수를 생성해 `i = 0`, `j = 1`로 두개의 포인트를 생성해 값을 비교하며 0번째 인덱스부터 중복되지 않는 값을 넣어간다.

```javascript
var removeDuplicates = function(nums) {
  // 1st solution *
  //TIME-COMPLEXITY: O(n);
  //MEMORY: O(n);
  var deduplicatedSet = new Set(nums);
  var deduplicatedArray = Array.from(deduplicatedSet);
  for (var i = 0; i < deduplicatedArray.length; i++) {
    nums[i] = deduplicatedArray[i];
  }
  nums.length = deduplicatedArray.length;
  return nums.length;
```

나랑 비슷하지만 다른 방법으로 풀었다. 나와 똑같이 `Set`를 통해 중복된 값을 제거해주고, `Array.from`을 통해 얕은 복사를 해 `deduplicatedArray`를 생성했다. `deduplicatedArray`는 내가 만든 `setNums`와 같고, 여기서는 `nums.length`를 반환해줬는데 난 `k` 변수를 따로 생성해서 `k`에 배열의 길이를 담아 반환해주었다. 그리고 난 `splice`를 이용해 복사를 해주었다.

---

### Reference

- [C++/Java/Python/JavaScript \| 🚀 ✅ Fully Explained \| 🚀 ✅ Two Pointer](https://leetcode.com/problems/remove-duplicates-from-sorted-array/solutions/3416595/c-java-python-javascript-fully-explained-two-pointer/?envType=study-plan-v2&envId=top-interview-150#:~:text=%7D%0A%7D%3B-,JavaScript,-/**%0A%20*%20%40param)
- [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/solutions/3337993/javascript-o-1-extra-memory-99-beats/?envType=study-plan-v2&envId=top-interview-150#:~:text=JAVASCRIPT%20%7C%7C%20O%281%29%20extra%20memory%20%7C%7C%2099%25%20beats)
