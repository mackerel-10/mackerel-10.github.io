---
layout: post
title: '167. Two Sum II - Input Array Is Sorted'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-28T16:01:00+09:00'
---

## [🔗](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) 문제

> **Medium**
>
> Given a **1-indexed** array of integers numbers that is already **_sorted in non-decreasing order_**, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 < numbers.length.
> Return *the indices of the two numbers,* index1 *and* index2*,* **_added by one_** *as an integer array* [index1, index2] *of length 2.*
> The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.
> Your solution must use only constant extra space.
>
> **Example 1:**
>
> - **Input:** numbers = [~2~,~7~,11,15], target = 9
> - **Output:** [1,2]
> - **Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
>
> **Example 2:**
>
> - **Input:** numbers = [~2~,3,~4~], target = 6
> - **Output:** [1,3]
> - **Explanation:** The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
>
> **Example 3:**
>
> - **Input:** numbers = [~-1~,~0~], target = -1
> - **Output:** [1,2]
> - **Explanation:** The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].

### 의사 코드

```javascript
var twoSum = function(numbers, target) {
  // numbers = alreday sorted in non-decreasing order
  // 1 <= index1 < index2 < numbers.length
  // return indices of the two numbers
  let 포인터1 = 0, 포인터2 = 1

  while (포인터1 < numbers 길이 - 1) {
    if (포인터1이 포인터2와 같고 더한 값이 target보다 작거나(조건1)
	  또는 포인터2가 numbers의 길이와 같다면(조건2)) {
	  포인터1 증가
	  포인터2 = 포인터1 + 1
	  continue
    }

    if (더한 값이 target이랑 같다면) {
	  // 문제에서 1 <= index1 < index2 < numbers.length라고 했기에 1을 더해줘야 함.
      return [포인터1 + 1, 포인터2 + 1];
    } else {
	  포인터2 증가
    }
  }
};
```

### 해결 방법

Two Pointer 알고리즘을 이용해 문제를 해결했다. 지난 세션에서 Big O에 대해 학습을 해, 시간복잡도와 공간복잡도를 고려해서 문제를 풀어보았다. Two Pointer를 사용함으로써 2개의 반복문(O(n^2))을 사용하지 않고 하나의 반복문(O(n))으로 문제를 해결했다.

```javascript
  while (i < numbers.length - 1) {
    if (j === numbers.length) {
      i++;
      j = i + 1;
      continue;
    }

    if (numbers[i] + numbers[j] !== target) {
      if (numbers[i] === numbers[i + 1]) { // 추가된 예외
        i++;
      } else {
        j++;
      }
    } else {
      return [i + 1, j + 1];
    }
  }
};
```

처음엔 중복된 값이 연속적으로 있는 경우를 처리해주지 않았으나, 풀이 제출 후 중복된 값이 연속적으로 있는 예시에서 시간 초과가 뜨면서 이를 고려하기 시작했다.

예외처리를 적용한 후 처음 제출했던 코드를 보면 중첩 조건문으로 예외 처리를 해주었다. 하지만, 중첩 조건문이 아니라 최소한의 조건문으로 이를 표현할 수 있을까에 대해 고민을 하기 시작했다. 예외 처리시 고려해야 할 점이 포인터1과 2가 가리키는 값이 같은 경우라고 생각해 첫번째 if문에 `numbers[i] === numbers[j]`조건을 추가해줬다. 또한, `numbers`는 정렬된 오름차순의 배열이기에 중복된 값만 제외하고 `target`값과 비교해주면 된다고 생각했다.

```javascript
  while (i < numbers.length - 1) {
    if (numbers[i] === numbers[j] || j === numbers.length) { // 이 부분
      i++;
      j = i + 1;
      continue;
    }

    if (numbers[i] + numbers[j] !== target) {
        j++;
    } else {
      return [i + 1, j + 1];
    }
  }
};

```

하지만, 이 예외처리에는 큰 문제점이 있었다. 같은 중복된 값을 더했을때 `target`일 경우도 생각해줘야 했는데, 새로 추가된 조건문은 이를 고려하지 않고 무조건 포인터1과 포인터2가 중복인 경우 포인터1과 2를(`i`와 `j`)를 증가시켜주게 된다. `target`이 될 수 있는 값이 있음에도 저 조건문 하나 때문에 `undefined`를 반환하게 된다.

```javascript
// 정답 코드
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (numbers, target) {
  let i = 0,
    j = 1;

  while (i < numbers.length - 1) {
    // 새로 추가된 조건
    if (
      (numbers[i] === numbers[j] && numbers[i] + numbers[j] < target) ||
      j === numbers.length
    ) {
      i++;
      j = i + 1;
      continue;
    }

    if (numbers[i] + numbers[j] === target) {
      return [i + 1, j + 1];
    } else {
      j++;
    }
  }
};
```

중복된 값이더라도 더했을때 `target`과 같은 경우는 찾아줘야 하기에, 기존의 조건에 추가적으로 조건을 작성해주었다. 포인터1과 2를 더했을때 `target`보다 작은 경우(`numbers[i] + numbers[j] < target`)가 새로 추가된 조건이다.

### 다른 해결법

```javascript
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */

var twoSum = function (numbers, target) {
  let p1 = 0;
  let p2 = numbers.length - 1;

  while (p1 < p2) {
    let sum = numbers[p1] + numbers[p2];
    if (sum === target) {
      return [p1 + 1, p2 + 1];
    } else if (sum > target) {
      p2--;
    } else {
      p1++;
    }
  }
};
```

풀이 방법은 비슷하나, 내 조건문은 조건이 너무 길어 조건을 좀 더 간결하게 작성하는 법이 궁금해 다른 풀이법을 찾아보게 됐다.

시작은 연속적인 인덱스에서 시작한 나와 다르게, 이 풀이의 포인터들은 제일 처음 인덱스와 제일 마지막 인덱스를 가리키고 있다. 핵심은 이 배열이 오름차순으로 정렬된 배열이며, 두 포인터의 값을 더했을때의 값과 타겟의 값과 비교를 통해 결과값을 산출해냄을 알 수가 있다. 조건문은 총 3개로 매우 간략한 조건을 사용하고 있다.

1. `sum === target` -> 결과값 반환
2. `sum > target` -> 포인터2(`p2`) 감소
3. else문 `sum < target` -> 포인터1(`p1`) 증가

길고 복잡한 조건이 아님에도 이 방식으로 문제를 해결하면 간략한 조건문으로 문제를 해결할 수 있음을 알았다.

---

### Reference

- [Deep And Easy - cheers!❤️](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/solutions/3417849/deep-and-easy-cheers/?envType=study-plan-v2&envId=top-interview-150#:~:text=Deep%20And%20Easy%20%2D%20cheers!%E2%9D%A4%EF%B8%8F)
