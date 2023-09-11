---
layout: post
title: '208. Implement Trie (Prefix Tree)'
category:
  - Algorithm
tags:
  - JavaScript
  - Leet Code
date: '2023-09-11T20:41:00+09:00'
---

## [🔗](https://leetcode.com/problems/implement-trie-prefix-tree/) 문제

> **Medium**
>
> A **[trie](https://en.wikipedia.org/wiki/Trie)** (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.
> Implement the Trie class:
>
> - Trie() Initializes the trie object.
> - void insert(String word) Inserts the string word into the trie.
> - boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
> - boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.
>
> **Example 1:**
>
> ```
> Input:
>   ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
>   [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
> Output:
>   [null, null, true, false, true, null, true
> Explanation:
>   Trie trie = new Trie();
>   trie.insert("apple");
>   trie.search("apple");   // return True
>   trie.search("app");     // return False
>   trie.startsWith("app"); // return True
>   trie.insert("app");
>   trie.search("app");     // return True
> ```

### 풀이

*Implement Trie \(Prefix Tree\)*라는 문제 제목을 보면 알 수 있듯이, 직접 Trie 자료구조를 구현해보는 문제이다. Trie의 각 노드는 하나의 Character를 소유하고 있고, 문자의 마지막 Character가 아닌 이상 다른 노드와 연결되어 있다.

![](image.png)
![Trie 자료구조](https://www.interviewcake.com/images/svgs/trie_with_four_strings.svg?bust=210)

처음에는 배열로 구현을 해보려고 했으나, `Search`까지는 해결할 수 있으나 `startsWith` 함수를 구현하기에 어려울거라는 생각이 들었다. 다른 구현 방법을 참고하기 위해 참고 코드를 찾아보게 되었고, Object를 사용해 Trie를 구현한 것을 확인할 수 있었다.

```javascript
class Trie {
  constructor() {
    this.root = {};
  }

  insert(word) {
    let node = this.root;
    for (let c of word) {
      if (node[c] == null) node[c] = {};
      node = node[c];
    }
    node.isWord = true;
  }

  traverse(word) {
    // Node 탐색
    let node = this.root;
    for (let c of word) {
      node = node[c];
      if (node == null) return null;
    }
    return node;
  }

  search(word) {
    const node = this.traverse(word);
    return node != null && node.isWord === true;
  }

  startsWith(prefix) {
    return this.traverse(prefix) != null;
  }
}
```

참고 코드에서는 Object를 사용해 Trie를 구현하고 있다. 간단하게 `c`를 key로 하는 `node`(Object)가 없으면 새로운 `node`(Object)를 생성해준다.

```javascript
var Trie = function () {
  this.root = {};
};

/**
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function (word) {
  let node = this.root;

  for (const c of word) {
    if (!node[c]) {
      node[c] = {};
    }
    node = node[c];
  }
  node.isEndOfWord = true;
};

/**
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function (word) {
  let node = this.root;

  for (const c of word) {
    node = node[c];
    if (!node) return false;
  }

  if (node.isEndOfWord) return true;
  else return false;
};

/**
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function (prefix) {
  let node = this.root;

  for (const c of prefix) {
    node = node[c];
    if (!node) return false;
  }

  return true;
};

/**
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```

위의 참고 코드를 기반으로 Object를 이용해 문제를 해결해 보았다. Hash Table을 사용할 수도 있으나, 이전에 Map과 Object의 차이점에 공부했을때 key로 String과 같은 타입을 사용할 때는 Object를 사용하는게 좋다고 했기에 Object를 선택했다. key는 `c`(Character), 그리고 value에는 서브 트리(하위 Chracter)들과 `isEndOfWord`와 같은 프로퍼티가 있다.

모든 단어의 끝에는 그 단어의 끝임을 알 수 있는 `isEndOfWord`라는 프로퍼티를 추가해 이를 기반으로 값을 `search`할 수 있게 해주었다. 이 프로퍼티가 추가된 이유는 `startsWith` 같은 경우는 `prefix`의 문자만 검색하면 되지만, `search`같은 경우는 매개변수로 들어오는 `word`와 동일한 문자가 있는지를 확인해줘야 하기 때문이다. `isEndOfWord`는 이를 확인하기 위한 플래그다.

---

### Reference

- [Clean JavaScript solution - Implement Trie \(Prefix Tree\) - LeetCode](https://leetcode.com/problems/implement-trie-prefix-tree/solutions/399178/clean-javascript-solution/?envType=study-plan-v2&envId=top-interview-150#:~:text=Clean%20JavaScript%20solution)
