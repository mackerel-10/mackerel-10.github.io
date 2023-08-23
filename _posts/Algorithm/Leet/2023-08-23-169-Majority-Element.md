---
layout: post
title: '169. Majority Element'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-08-23T22:24:00+09:00'
---

## 문제

[🔗 링크](https://leetcode.com/problems/majority-element/description/?envType=study-plan-v2&envId=top-interview-150)

> **Easy**
>
> Given an array nums of size n, return *the majority element*.
> The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

### 의사 코드

```javascript
var majorityElement = function(nums) {
  nums 오름차순으로 정렬

  return nums[중간 인덱스];
};
```

### 내 코드

```javascript
var majorityElement = function (nums) {
  nums.sort((a, b) => a - b);

  return nums[Math.floor(nums.length / 2)];
};
```

굳이 숫자의 개수를 세지 않아도 간단하게 생각하면 해결되는 문제였다. 문제에서의 `majority element`란 배열내에서 `n / 2`보다 많은 갯수의 요소이다(`n`은 `nums`의 길이이다). `n / 2`개보다 많은 요소이면 배열의 중간 인덱스는 무조건 `majority element`가 차지하게 된다. 우선 배열을 오름차순으로 정렬 해주고, 중간 인덱스를 구한다음 그 인덱스에 해당하는 값을 반환시켜줬다.

### 다른 해결법

```java
public class Solution {
    public int majorityElement(int[] num) {
        int major = num[0], count = 1;

        for (int i = 1; i < num.length; i++){
            if (count == 0){
                count++;
                major = num[i]; // 비교대상 num[i] 변경
            } else if (major == num[i]){ // major가 num[i]이면 count 증가
                count++;
            } else count--; // major가 num[i]가 아니면 count 감소
        }
        return major;
    }
}
```

나처럼 정렬을 사용하지 않는 풀이법을 찾아보았다. 해당 풀이에선 `major`와 `count` 변수를 사용해 답을 찾아낸다. `major`에 처음엔 `num[0]`의 값을 넣고 시작을 한다. `major`의 값이 변동되려면 `count`가 `0`이 돼야 한다.`major`에 답인 `majority element`가 들어오면 해당 값의 개수가 `n / 2`개보다 많기에, `count`가 감소할 경우보다 증가할 경우가 많게 된다. 그러기에 `major`의 최종값은 `majority element`일 수 밖에 없다.

---

### Reference

- [O\(n\) time O\(1\) space fastest solution - Majority Element - LeetCode](https://leetcode.com/problems/majority-element/solutions/51613/o-n-time-o-1-space-fastest-solution/?envType=study-plan-v2&envId=top-interview-150#:~:text=O%28n%29%20time%20O%281%29%20space%20fastest%20solution)
