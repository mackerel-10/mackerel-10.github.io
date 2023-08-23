---
layout: post
title: '88. Merge Sorted Array'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-23T15:50:00+09:00'
---

## 문제

[🔗 링크](https://leetcode.com/problems/merge-sorted-array/)

> **Easy**
>
> You are given two integer arrays nums1 and nums2, sorted in **non-decreasing order**, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.
> **Merge** nums1 and nums2 into a single array sorted in **non-decreasing order**.
> The final sorted array should not be returned by the function, but instead be *stored inside the array* nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

### 의사코드

```javascript
const nums1에 nums2값 추가하는 함수 = function {
  let 마지막 인덱스 = nums1의 마지막 인덱스;

  while (시작 인덱스 <= 마지막 인덱스) {
    let 중간 인덱스 = 시작 인덱스와 마지막 인덱스의 중간 값;

    if (nums1[중간인덱스] <= nums2값 <= nums1[중간 인덱스 + 1]) {
		nums1에 nums2값 삽입
		시작 인덱스 = 중간 인덱스 + 1
		while 반복문 중단
    }

    if (nums2값 < nums1[중간 인덱스]) {
		마지막 인덱스 = 중간 인덱스 - 1
    } else if (nums2값 > nums1[중간 인덱스]){
		시작 인덱스 = 중간 인덱스 + 1
    }
  }

  return {nums1, 시작 인덱스};
}

var merge = function(nums1, m, nums2, n) {
  m 길이만큼 nums1 자름

  if (n의 길이가 0) {
    return;
  }

  let 시작 인덱스 = 0;
  for (n만큼 반복) {
    if (num1의 길이가 0 || nums1의 마지막 인덱스 값 <= 비교하려는 nums2 값) {
		nums1 마지막에 nums2 값 추가
    } else if (nums1의 첫번째 값 >= nums2의 값) {
		nums1 처음에 nums2 값 추가
    } else {
		const 결과 = nums1에 nums2값 추가하는 함수
		nums1 = 결과.nums1
		시작 인덱스 = 결과.시작 인덱스
    }
  }
  return;
};
```

### 내 코드

```javascript
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */

const insertValueToNums1 = function (nums1, nums2, index, startIndex) {
  let endIndex = nums1.length - 1;

  while (startIndex <= endIndex) {
    let middleIndex = Math.floor((startIndex + endIndex) / 2);

    if (
      nums2[index] >= nums1[middleIndex - 1] &&
      nums2[index] <= nums1[middleIndex]
    ) {
      nums1.splice(middleIndex, 0, nums2[index]);
      startIndex = middleIndex + 1;
      break;
    }

    if (nums2[index] < nums1[middleIndex]) {
      endIndex = middleIndex - 1;
    } else if (nums2[index] > nums1[middleIndex]) {
      startIndex = middleIndex + 1;
    }
  }

  return { nums1, startIndex };
};

var merge = function (nums1, m, nums2, n) {
  // nums1 = nums1.slice(0, m);
  // nums2 = nums2.slice(0, n);
  nums1.splice(m, nums1.length - m);

  if (n === 0) {
    return;
  }

  let startIndex = 0;
  for (let index = 0; index < n; index++) {
    console.log(nums2[index]);
    if (nums1[nums1.length - 1] <= nums2[index] || nums1.length === 0) {
      // 제일 마지막이 nums2보다 크면 바로 push
      nums1.push(nums2[index]);
    } else if (nums1[0] >= nums2[index]) {
      // 제일 처음이 nums2보다 크면 바로 push
      nums1.splice(0, 0, nums2[index]);
    } else {
      const result = insertValueToNums1(nums1, nums2, index, startIndex);
      nums1 = result.nums1;
      startIndex = result.startIndex;
    }
  }

  console.log(nums1);
  return;
};
```

사실 간단하게 생각하면 되는 문제를 너무 깊고 어렵게 생각해서 풀이가 더욱 복잡해졌다. 내 방식은 `nums1`에 `nums2`의 값이 오름차순에 맞춰 들어갈 수 있게 하나씩 삽입하는 형식이다. 그래서 `splice`와 `push`를 사용해 값을 넣고 있는 것을 확인할 수 있다. `nums2`가 들어가야하는 위치를 빠르게 탐색하기 위해 이진 탐색을 사용했다.

가장 많이 헤맸던 곳은 이 문제는 정답값을 반환하는게 아니라 `nums1`에 담아서 반환하는 형식인데, `nums1`을 아무리 수정해도 수정된 값이 전달되지 않아서 고민을 많이 했다. 나중에서야 이유를 알게 됐는데, 문제가 되는 코드는 이 코드였다.

```javascript
// nums1 = nums1.slice(0, m);
// nums2 = nums2.slice(0, n);
nums1.splice(m, nums1.length - m);
```

`merge` 함수의 초반 부분에 내가 `slice`를 사용해서 배열을 잘라주는 부분이 있었는데, 매개변수로 전달된 `nums1` 배열의 참조값을 `nums1.slice(0, m)`을 통해 반환되는 배열의 참조값으로 수정해줘서 생긴 문제다. 문제를 해결하면서 참조의 의미가 헷갈려서 다시 공부를 했다. 객체의 이름은 객체에 대한 메모리 주소의 참조 값을 가지게 되는데, 배열은 사실상 JavaScript 내부에선 객체기 때문에 위의 `nums1` 또한 `nums1`이 저장된 메모리 주소의 참조 값을 가지고 있기에 값을 수정했을때 반환하지 않더라도 값이 수정됨을 알 수 있었다.

### 다른 해결법

```javascript
var merge = function (nums1, m, nums2, n) {
  for (let i = m, j = 0; j < n; i++, j++) {
    nums1[i] = nums2[j];
  }
  nums1.sort((a, b) => a - b);
};
```

위와 같이 간단하게 합친 다음에 sort하는 방식으로 푼 사람도 있었다. 정말 내가 복잡하게 생각했던 것...

```javascript
// Two pointer algorithm
var merge = function (nums1, m, nums2, n) {
  let i = m - 1; // nums1에 대한 index
  let j = n - 1; // nums2에 대한 index
  let k = m + n - 1; // nums1과 nums2의 병합시의 index

  while (j >= 0) {
    if (i >= 0 && nums1[i] > nums2[j]) {
      nums1[k--] = nums1[i--]; // nums1[k]에 값 추가 후 --
    } else {
      nums1[k--] = nums2[j--];
    }
  }
};
```

그리고, 새로운 알고리즘을 접했다. Two pointer algorithm에 대해 설명하자면,

- 리스트에 순차적으로 접근해야 할 때 **두 개의 점의 위치를 기록하면서 처리**하는 알고리즘

* 정렬되어 있는 두 리스트의 합집합에도 사용됨. 병합정렬(merge sort)의 counquer 영역의 기초

2번째 설명을 보면 알 수 있듯이 이 문제의 의의라고 생각된다. 이 문제 자체가 정렬되어 있는 두 리스트의 병합정렬이기 때문이다.

---

### Reference

- [\[LeetCode \| Javascript\] Merge Sorted Array](https://velog.io/@rkio/LeetCode-88.-Merge-Sorted-Array)
- [\[ JavaScript \] 자바스크립트 배열의 실체: 배열이 아닙니다.](https://undefined.tistory.com/5)
- [참조에 의한 객체 복사](https://ko.javascript.info/object-copy)
- [\[Algorithm\] 투포인터\(Two Pointer\) 알고리즘](https://butter-shower.tistory.com/226)
- [투 포인터\(Two Pointer\) 알고리즘](https://coding-food-court.tistory.com/121)
