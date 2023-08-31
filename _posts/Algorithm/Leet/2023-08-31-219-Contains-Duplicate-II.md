---
layout: post
title: '219. Contains Duplicate II'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-31T13:52:00+09:00'
---

## [🔗](https://leetcode.com/problems/contains-duplicate-ii/description/?envType=study-plan-v2&envId=top-interview-150) 문제

> **Easy**
>
> Given an integer array nums and an integer k, return true *if there are two* **_distinct indices_** i *and* j *in the array such that* nums[i] == nums[j] *and* abs(i - j) <= k.
>
> **Example 1:**
>
> ```
> Input: nums = [1,2,3,1], k = 3
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: nums = [1,0,1,1], k = 1
> Output: true
> ```
>
> **Example 3:**
>
> ```
> Input: nums = [1,2,3,1,2,3], k = 2
> Output: false
> ```

### 풀이

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {boolean}
 */
var containsNearbyDuplicate = function (nums, k) {
  // 풀이
  let hashTable = {};
  let j;

  for (let i = 0; i < nums.length; i++) {
    j = hashTable[nums[i]];

    if (j !== undefined && i - j <= k) {
      return true;
    }
    hashTable[nums[i]] = i;
  }
  return false;
};
```

```javascript
// 다른 풀이
var containsNearbyDuplicate = function (nums, k) {
  const hi = new Map();

  for (let i = 0; i < nums.length; i++) {
    if (hi.has(nums[i]) && i - hi.get(nums[i]) <= k) {
      return true;
    }
    hi.set(nums[i], i);
  }
  return false;
};
```

Hash Table을 사용해서 풀었고, 객체를 사용해 hash 값을 저장해 주었다. 반면 문제 풀이 후 찾은 다른 풀이는 Map을 사용해 문제를 푼것을 확인할 수 있었다. 로직 자체는 차이가 없지만 코드를 실행했을때 객체를 사용한 풀이와 Map을 사용한 풀이는 약 30ms의 Runtime 차이가 났다. 왜 Runtime 차이가 나는걸까에 대한 의문이 생겨 이에 대해 찾아보았다.

<img width="566" alt="스크린샷 2023-08-31 13 03 20" src="https://github.com/mackerel-10/mackerel-10.github.io/assets/67633810/4b5be5c8-ef69-459a-bb0b-0c92e3e05e84">

> [Map vs Object in JavaScript](https://stackoverflow.com/questions/18541940/map-vs-object-in-javascript#:~:text=An%20object%20behaves%20like%20a%20dictionary%20because%20JavaScript%20is%20dynamically%20typed%2C%20allowing%20you%20to%20add%20or%20remove%20properties%20at%20any%20time.)
> But Map() is much better because it:
>
> - Provides get, set, has, and delete methods.
> - **Accepts any type for the keys instead of just strings.**
> - **Provides an iterator for easy for-of usage and maintains the order of results.**
> - Doesn't have edge cases with prototypes and other properties showing up during iteration or copying.
> - Supports millions of items.
> - **Is very fast.**

레퍼런스 자료 [^1]를 읽었을때, 종합적으로 얘기하고 있는 것은 Map은 iterable하고, string뿐 아니라 다른 타입들을 key로 담을 수 있다는 장점이 있다고 설명하고 있다. 이 장점 때문에 딕셔너리를 사용해야 할 때, Object보다 Map 사용을 권장했다.

> However, if you're only using string-based keys and need maximum read performance, then objects might be a better choice. This is because ~[JavaScript engines compile objects down to C++ classes](https://mathiasbynens.be/notes/shapes-ics)~ in the background, and the access path for properties is much faster than a function call for Map().get().

하지만, 위 글과 같은 상황에서는 객체가 Map보다 더 나은 Performance를 낼 수 있다. 항상 Map이 답은 아니며, 사용해야 하는 상황에 따라 선택은 바뀔 수 있다.

---

### Reference

[^1]: [Map - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map#:~:text=the%20%3D%3D%3D%20operator.-,Objects%20vs.%20Maps,-Object%20is%20similar), [Map vs Object in JavaScript](https://stackoverflow.com/questions/18541940/map-vs-object-in-javascript)
