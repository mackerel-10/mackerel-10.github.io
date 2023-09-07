---
layout: post
title: '530. Minimum Absolute Difference in BST'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-09-07T15:23:00+09:00'
---

## [🔗](https://leetcode.com/problems/minimum-absolute-difference-in-bst/) 문제

> **Easy**
>
> Given the root of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.
>
> **Example 1:**
>
> ![bst1](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)
>
> ```
> Input: root = [4,2,6,1,3]
> Output: 1
> ```
>
> **Example 2:**
>
> ![bst2](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)
>
> ```
> Input: root = [1,0,48,null,null,12,49]
> Output: 1
> ```

### 풀이

제목을 확인하면 알 수 있듯이, BST(Binary Search Tree)를 활용한 문제이다. 문제를 보면 _the minimum absolute difference between the values of any two different nodes in the tree_.라는 부분이 있고, 이를 직역하면 *트리 안의 두 개의 다른 노드 사이의 차이값 중 최소값*이다.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

var getMinimumDifference = function (root) {
  let currentNode = root;
  let min = Number.MAX_SAFE_INTEGER;
  let left, right;

  while (currentNode.left || currentNode.right) {
    if (currentNode) {
      if (currentNode.left && currentNode.right) {
        left = Math.abs(currentNode.val - currentNode.left.val);
        right = Math.abs(currentNode.val - currentNode.right.val);
        if (left < right) {
          if (left < min) {
            min = left;
          }
          currentNode = currentNode.left;
        } else if (right < left) {
          if (right < min) {
            min = right;
          }
          currentNode = currentNode.right;
        } else {
          // = equal
          if (right < min) {
            min = right;
          }
          if (currentNode.left.left) {
            currentNode = currentNode.left;
          } else if (currentNode.right.left) {
            currentNode = currentNode.right;
          } else {
            currentNode = currentNode.left;
          }
        }
      } else if (!currentNode.left) {
        if (Math.abs(currentNode.val - currentNode.right.val) < min) {
          min = Math.abs(currentNode.val - currentNode.right.val);
        }
        currentNode = currentNode.right;
      } else if (!currentNode.right) {
        if (Math.abs(currentNode.val - currentNode.left.val) < min) {
          min = Math.abs(currentNode.val - currentNode.left.val);
        }
        currentNode = currentNode.left;
      }
    }
    console.log(currentNode.val, min);
  }

  return min;
};
```

최적화와 정리 없이 구현만을 목적으로 두고 작성을 했기에 매우 많은 조건문으로 이루어져 있다. 내 풀이는 인접한 노드끼리의 차를 구한다. 문제대로라면 전체적인 탐색을 하면서 최소값인 차이를 찾아내야 하나, 전체적인 탐색도 없이 노드와 left, right 사이의 차를 구한 후 둘 중 최소값인 노드를 선택해 탐색해 나가는 형식이다.

이 방식으로 문제를 해결하다가 하나의 예제를 겪으면서 내 풀이가 잘못됨을 알았다.

```
[236,104,701,null,227,null,911]
```

![bst3](https://github.com/mackerel-10/mackerel-10.github.io/assets/67633810/1bc5ea8d-e912-4c48-b51d-b2606b12bb3a)

내 풀이대로라면 701과 911은 탐색에서 제외되고, 236과 227 사이의 연산 또한 일어나지 않는다. 이 예제를 통해 풀이가 잘못됨을 알았고, 잘못된 부분을 수정하기로 했다.

### 풀이 참고

잘못된 부분을 수정하기 위해 부분 탐색이 아니라 완전 탐색으로 수정해야 했고, 'BST를 완전 탐색하려면 어떻게 해야할까'에 대한 고민이 들어 풀이를 찾아보게 됐다.

```javascript
// 참고 코드
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var getMinimumDifference = function (root) {
  let arr = [];
  function inOrder(node) {
    if (node) {
      inOrder(node.left);
      arr.push(node.val);
      inOrder(node.right);
    }
  }
  // inOrder will increasing order in BST
  inOrder(root);

  // because of increasing order we find minimum difference by travelling linearly once
  let minDiff = arr[1] - arr[0];
  for (let i = 2; i < arr.length; i++) {
    minDiff = Math.min(minDiff, arr[i] - arr[i - 1]);
  }
  return minDiff;
};
```

이 풀이는 `inOrder` 함수를 사용해 재귀적으로 탐색을 진행하며 `arr`에 값을 넣는다. `arr`의 값들은 오름차순으로 정렬돼어 있어, `for`문에서 선형적으로 차이값을 계산할 수 있다.

`inOrder` 함수를 사용해 오름차순으로 값이 정렬되는 과정이 궁금해 직접 탐색과정을 정리해보았다.

![bst3](https://github.com/mackerel-10/mackerel-10.github.io/assets/67633810/1bc5ea8d-e912-4c48-b51d-b2606b12bb3a)

```
236(root) ➡️ 104(push) ➡️ 227(push) ➡️ 236(root, push) ➡️ 701(push) ➡️ 911(push)
```

1. **236** ➡️ 104

   `inOrder(node.left)`로 우선 왼쪽 서브트리를 탐색한다.

2. 104(`push`) ➡️ 227

   104의 `node.left`는 `null`이므로 `arr`에 `push`가 된다. `push`가 끝나면 `inOrder(node.right)`가 호출되기에 227로 이동한다.

3. 227(`push`) ➡️ **236(push)**

   227은 left, right가 모두 없기에 바로 `push`된다. 루트 노드인 236에서 왼쪽 서브트리 탐색이 끝났기에, 236 또한 `push`되며 오른쪽 서브트리 탐색을 시작한다.

### 풀이 개선

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

var getMinimumDifference = function (root) {
  let valueList = [];
  let min = Number.MAX_SAFE_INTEGER;

  function bst(node) {
    if (node) {
      bst(node.left);
      valueList.push(node.val);
      bst(node.right);
    }
  }

  bst(root);

  let sub;
  for (let i = 0; i < valueList.length - 1; i++) {
    sub = valueList[i + 1] - valueList[i];
    if (sub < min) {
      min = sub;
    }
  }

  return min;
};
```

참고 코드를 응용하여 개선해보았다. 전에 다른 알고리즘 문제를 풀며 `min`값을 첫번째 계산값을 넣는게 아니라`Number.MAX_SAFE_INTEGER`를 사용하는 것을 보고, 이를 활용해 보았다. 처음 `sub`값은 `MAX_SAFE_INTEGER`보다 작은 값일 수 밖에 없기에 `min`값은 `for`문의 첫 반복시 바로 값이 바뀌게 된다.

---

### Reference

- [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/solutions/3202655/getminimumdifference-inorder-o-n-javascript/?envType=study-plan-v2&envId=top-interview-150#:~:text=getMinimumDifference%20%2D%20Inorder%2D%20O%28n%29%20%2D%20Javascript)
