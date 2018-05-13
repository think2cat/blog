---
title: Javascript数组的every、some、map、filter、forEach
tags:
  - javascript
id: 1734
categories:
  - Web
abbrlink: 10afd6ef
date: 2017-03-10 16:02:32
---
*   **every**，判断是否所有元素都符合，需要每个都为true才返回true，否则返回false
*   **some**，判断是否有元素符合，一旦遇到true则停止
*   **map**，每个元素按function返回结果构成新数组
*   **filter**，返回所有符合条件的元素构成的新数组
*   **forEach**，单独执行每个元素，无返回

各函数执行时需要传入function作为参数，每次执行会传入value和index
<!-- more -->
看个例子就明白了
```js
var fun = function(v, i){
	console.log(v,i);
	return v %2 ==0;
}

var arr = [0,0,2,2,1,2,3,4,5,6,7,8];
arr.every(fun);
//-->0 0
//-->0 1
//-->2 2
//-->2 3
//-->1 4
//-->false
//every一旦遇到false则中止，并返回false

arr.some(fun)
//-->0 0
//-->true
//some一旦遇到true则中止，并返回true

arr.map(fun)
//-->0 0
//-->0 1
//-->2 2
//-->2 3
//-->1 4
//-->2 5
//-->3 6
//-->4 7
//-->5 8
//-->6 9
//-->7 10
//-->8 11
//-->[true, true, true, true, false, true, false, true, false, true, false, true]
//map会执行每个元素并把function返回的结果组成新数组，不改变原数组

arr.filter(fun)
//-->0 0
//-->0 1
//-->2 2
//-->2 3
//-->1 4
//-->2 5
//-->3 6
//-->4 7
//-->5 8
//-->6 9
//-->7 10
//-->8 11
//-->[0, 0, 2, 2, 2, 4, 6, 8]
//filter跟map不同的是新数组不是返回值，而是返回true的元素

arr.forEach(fun)
//-->0 0
//-->0 1
//-->2 2
//-->2 3
//-->1 4
//-->2 5
//-->3 6
//-->4 7
//-->5 8
//-->6 9
//-->7 10
//-->8 11
//-->undefined
//forEach不改变原数组，也无返回值，只是把数组依个执行了一下
```