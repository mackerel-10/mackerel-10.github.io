---
layout: post
title: '80. Remove Duplicates from Sorted Array II'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-23T20:26:00+09:00'
---

## 문제

[🔗 링크](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

> **Medium**
>
> Given an integer array nums sorted in **non-decreasing order**, remove some duplicates **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.
> Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.
> Return k *after placing the final result in the first* k *slots of* nums.
> <mark>Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.</mark>

### 의사 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
  let 포인터1의 인덱스
  let 카운터

  for (포인터2의 인덱스 = 포인터1의 인덱스 + 1 && nums의 길이까지 반복) {
    if (카운터 < 2 && 포인터1이 가리키는 값 === 포인터2가 가리키는 값) {
	  포인터1 + 1의 위치에 포인터2 값 저장
	  카운터 증가
    } else if (포인터1이 가리키는 값 !== 포인터2가 가리키는 값) {
	  포인터1 + 1의 위치에 포인터2 값 저장
	  카운터 초기화
    }
  }

  return 수정된 nums의 길이;
};
```

### 내 코드

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function (nums) {
  let i = 0;
  let count = 1;

  for (let j = 1; j < nums.length; j++) {
    if (count < 2 && nums[i] === nums[j]) {
      nums[++i] = nums[j];
      count++;
    } else if (nums[i] !== nums[j]) {
      nums[++i] = nums[j];
      count = 1;
    }
  }

  return i + 1;
};
```

이전의 문제들과 다르게 이번 문제의 핵심은 _Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory._ '메모리를 사용해 다른 배열을 생성하지 말라'가 핵심이다. 이전에 Two Pointer 알고리즘을 이용해 푼 풀이들을 보면 새로운 배열을 생성하지 않고 기존에 `nums1`/`nums`에 할당된 메모리를 사용해 정답을 생성했다. 그래서 나도 Two Pointer 알고리즘을 활용해 문제를 풀어보았다.

배열은 중복이 허용이 되나 2개를 초과해서 중복되면 안되기에, `count`라는 변수를 생성해 같은 문자가 몇번 반복되는지 확인해주었다. `i = 0`, `j = 1`로 두개의 포인터를 생성해 값을 비교해나간다. `k`는 수정된 인덱스 값을 `i`가 갖고 있으므로, `i`에 1을 추가해 반환해주었다.

### 다른 해결법

```javascript
var removeDuplicates = function (nums) {
  // Special case...
  if (nums.length <= 2) {
    return nums.length;
  }
  // Initialize an integer k that updates the kth index of the array...
  // only when the current element does not match either of the two previous indexes...
  let k = 2;
  // Traverse elements through loop...
  for (let i = 2; i < nums.length; i++) {
    // If the index does not match the (k-1)th and (k-2)th elements, count that element...
    if (nums[i] != nums[k - 2] || nums[i] != nums[k - 1]) {
      nums[k] = nums[i];
      k++;
      // If the index matches the (k-1)th and (k-2)th elements, we skip it...
    }
  }
  return k; //Return k after placing the final result in the first k slots of nums...
};
```

2번의 중복까지는 허용되지만, 3번째부터의 중복은 허용되지 않는다. 이를 이용해 `k-1`이나 `k-2`번째의 값과 같지 않으면 이는 3번 중복되지 않았다는 것을 의미한다(주석으로도 설명이 돼있다.). 조건문을 2번 쓸 필요없이 한개의 조건문으로도 해결이 가능하다.

```javascript
const removeDuplicates = (nums) => {
  let j = 0;
  let i = 0;

  for (; i < nums.length; i += 1) {
    if (nums[i] !== nums[i + 2]) {
      nums[j] = nums[i];
      j += 1;
    }
  }

  return j;
};
```

여기서도 위 코드와 같이 비교범위를 2를 잡고 시작한다. `j`는 문제에서의 `k`를 의미하고 `i`는 비교할 인덱스를 의미한다. `i`를 0부터 시작하게 해 `i + 2` 인덱스의 값과 비교하며 다를 경우 `nums[j]`에 저장하게 된다. 위에 설명했듯이 이 두개의 값이 같으면 3번 중복된다는 의미기에(이 배열은 오름차순으로 정렬된 배열이다.) 조건문으로 아닌 경우만 저장한다.

두 풀이의 핵심은 비교범위를 **2**로 잡는다는 것이다. 다음에 비슷한 문제를 풀때 이 풀이를 기억해서 응용해봐야겠다.

---

### Reference

- [Pretty simple](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/solutions/599075/javascript-pretty-simple/?envType=study-plan-v2&envId=top-interview-150#:~:text=%5BJavaScript%5D%20Pretty%20simple)
- [Very Easy 100% (Fully Explained)(Java, C++, Python, JavaScript, C, Python3)](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/solutions/2415500/very-easy-100-fully-explained-java-c-python-javascript-c-python3/?envType=study-plan-v2&envId=top-interview-150#:~:text=JavaScript%20Solution%3A)
