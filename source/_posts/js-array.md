---
title: js数组API
categories: javascript
tags: [web,js]
---
###### concat

concat() 方法用于连接两个或多个数组。

该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。

	arrayObject.concat(arrayX,arrayX,......,arrayX)
	
返回值

返回一个新的数组。该数组是通过把所有 arrayX 参数添加到 arrayObject 中生成的。如果要进行 concat() 操作的参数是数组，那么添加的是数组中的元素，而不是数组。

	// create two arrays
	var arr1 = ['a', 'b', 'c'];
	var arr2 = ['d', 'e', 'f'];

	/* call concat() on the first array passing
	   the second as an argument */
	var arr3 = arr1.concat(arr2);

	// log the result
	console.log(arr3);
	// expected output: a,b,c,d,e,f	

<!--more-->
	
###### copyWithin

数组实例的copyWithin方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组

Array.prototype.copyWithin(target, start = 0, end = this.length)  
target （必需）：从该位置开始替换数据。
start （可选）：从该位置开始读取数据，默认为 0 。如果为负值，表示倒数。
end （可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

返回值

修改后的数组

	[1, 2, 3, 4, 5].copyWithin(-2);
	// [1, 2, 3, 1, 2]

	[1, 2, 3, 4, 5].copyWithin(0, 3);
	// [4, 5, 3, 4, 5]

	[1, 2, 3, 4, 5].copyWithin(0, 3, 4);
	// [4, 2, 3, 4, 5]

	[1, 2, 3, 4, 5].copyWithin(-2, -3, -1);
	// [1, 2, 3, 3, 4]

###### entries

entries()方法返回一个新的数组迭代器对象，该对象包含数组中每个索引的键/值对。

返回值

一个新的数组迭代器对象。


	var a = ['a', 'b', 'c'];
	var iterator = a.entries();

	console.log(iterator.next().value); // [0, 'a']
	console.log(iterator.next().value); // [1, 'b']
	console.log(iterator.next().value); // [2, 'c']

	var a = ['a', 'b', 'c'];
	var iterator = a.entries();

	for (let e of iterator) {
	  console.log(e);
	}
	// [0, 'a']
	// [1, 'b']
	// [2, 'c']
	
###### from

from()方法返回一个新的数组迭代器对象，该对象包含数组中每个索引的键/值对。

Array.from(arrayLike[, mapFn[, thisArg]])
arrayLike（必需）：一个类似数组的或可迭代的对象，可以转换为数组。
mapFn （可选）：映射函数来调用数组的每个元素。
thisArg （可选）：在执行mapFn时使用的值。

返回值

一个新的数组实例。

	const bar = ["a", "b", "c"];
	Array.from(bar);
	// ["a", "b", "c"]

	Array.from('foo');
	// ["f", "o", "o"]
	Array.from('foo'); 
	// ["f", "o", "o"]

	var s = new Set(['foo', window]); 
	Array.from(s); 
	// ["foo", window]var m = new Map([[1, 2], [2, 4], [4, 8]]);
	Array.from(m); 
	// [[1, 2], [2, 4], [4, 8]]

	// arguments 参数专数组
	function f() {
	  return Array.from(arguments);
	}

	f(1, 2, 3);

	// [1, 2, 3]
	
###### every

entries()方法返回一个回调函数的条件bool值。

arr.every(callback[, thisArg])
callback（必需）：回调函数。
thisArg （可选）：当前值。

返回值

返回 回调函数每个数组元素的bool值

	function isBigEnough(element, index, array) { 
	  return element >= 10; 
	} 

	[12, 5, 8, 130, 44].every(isBigEnough);   // false 
	[12, 54, 18, 130, 44].every(isBigEnough); // true
	
###### fill

fill()方法将数组的所有元素从起始索引填充到结束索引。

arr.fill(value)
arr.fill(value, start)
arr.fill(value, start, end)
value：一个类似数组的或可迭代的对象，可以转换为数组。
start ：开始位置 默认为0
end ：结束位置 默认为当前length

返回值

修改后的数组。

	[1, 2, 3].fill(4);               // [4, 4, 4]
	[1, 2, 3].fill(4, 1);            // [1, 4, 4]
	[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
	[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
	[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
	[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
	Array(3).fill(4);                // [4, 4, 4]
	[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

###### filter

filter()方法创建一个新的数组，其中包含符合回调条件的所有元素。

var newArray = arr.filter(callback[, thisArg])
callback：回调函数。
thisArg：当前元素

返回值

一个带有符合条件的元素的新数组。

	var words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];

	var longWords = words.filter(function(word){
	  return word.length > 6;
	});

	// Filtered array longWords is ["exuberant", "destruction", "present"]

	ES6

	var words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];

	var longWords = words.filter(word => word.length > 6);

	// Filtered array longWords is ["exuberant", "destruction", "present"]
	
###### find

find()方法返回满足回调条件的数组中第一个元素的值。否则将返回未定义。

arr.find(callback[, thisArg])
callback：回调函数。
thisArg：当前元素

返回值

如果元素符合条件  返回数组中的值;否则,未定义。

	function isBigEnough(element) {
	  return element >= 15;
	}

	[12, 5, 8, 130, 44].find(isBigEnough); // 130

###### findIndex

findIndex()方法返回满足回调条件的数组中第一个元素的索引。否则将返回-1。

arr.findIndex(callback[, thisArg])
callback：回调函数。
thisArg：当前元素

返回值

如果元素符合条件 返回数组中的值的索引;否则,-1。

	function isBigEnough(element) {
	  return element >= 15;
	}

	[12, 5, 8, 130, 44].findIndex(isBigEnough); 
	// index of 4th element in the Array is returned,
	// so this will result in '3'
	
###### forEach

findIndex()方法返回满足回调条件的数组中第一个元素的索引。否则将返回1。

	arr.forEach(function callback(currentValue, index, array) {
		//your iterator
	}[, thisArg]);
	
callback：回调函数。

currentValue：在数组中处理当前元素

index：在数组中处理当前元素的索引。
array：当前数组

	var a = ['a', 'b', 'c'];

	a.forEach(function(element) {
		console.log(element);
	});

	// a
	// b
	// c
	
###### includes

includes()方法确定一个数组是否包含某个元素，返回true或false。

arr.includes(searchElement)
arr.includes(searchElement, fromIndex)
serachElement：查询元素。

fromIndex：开始查询元素位置

返回值

boolean

	var a = [1, 2, 3];
	a.includes(2); // true 
	a.includes(4); // false

	[1, 2, 3].includes(2);     // true
	[1, 2, 3].includes(4);     // false
	[1, 2, 3].includes(3, 3);  // false
	[1, 2, 3].includes(3, -1); // true
	[1, 2, NaN].includes(NaN); // true
	
###### indexOf

indexOf()返回一个给定元素可以在数组中找到的第一个索引，如果不是，则返回- 1。

arr.indexOf(searchElement[, fromIndex])
serachElement：查询元素。

fromIndex：开始查询元素位置

返回值

返回一个给定元素可以在数组中找到的第一个索引，如果不是，则返回- 1。

	var a = [2, 9, 9]; 
	a.indexOf(2); // 0 
	a.indexOf(7); // -1

	if (a.indexOf(7) === -1) {
	  // element doesn't exist in array
	}
	
###### join

join()将数组中的所有元素(或类似数组的对象)连接到一个字符串中。

	arr.join()
	arr.join(separator)
	separator：分隔符

	var a = ['Wind', 'Rain', 'Fire'];
	a.join();    // 'Wind,Rain,Fire'
	a.join('-'); // 'Wind-Rain-Fire'

###### keys

keys()方法返回一个新的数组迭代器对象，该对象包含数组中每个索引的键。

	arr.keys()
	var arr = ['a', 'b', 'c'];
	var iterator = arr.keys();

	console.log(iterator.next()); // { value: 0, done: false }
	console.log(iterator.next()); // { value: 1, done: false }
	console.log(iterator.next()); // { value: 2, done: false }
	console.log(iterator.next()); // { value: undefined, done: true }

###### lastIndexOf

lastIndexOf()方法返回在数组中可以找到给定元素的最后一个索引，如果不存在，则返回- 1。该数组向后搜索，从fromIndex开始。

arr.lastIndexOf(searchElement)
arr.lastIndexOf(searchElement, fromIndex)
serachElement：查询元素。

fromIndex：开始查询元素位置

返回值

数组中元素的一个索引;如果没有找到 -1。

	var numbers = [2, 5, 9, 2];
	numbers.lastIndexOf(2);     // 3
	numbers.lastIndexOf(7);     // -1
	numbers.lastIndexOf(2, 3);  // 3
	numbers.lastIndexOf(2, 2);  // 0
	numbers.lastIndexOf(2, -2); // 0
	numbers.lastIndexOf(2, -1); // 3
	
###### map

map()方法创建一个新的数组，该数组的结果是调用调用数组中的每个元素的函数。

	var new_array = arr.map(function callback(currentValue, index, array) {
		// Return element for new_array
	}[, thisArg])

callback：回调函数。

currentValue：在数组中处理当前元素

index：在数组中处理当前元素的索引。
array：当前数组

返回值

每个元素的改变后的组成的新数组

	var numbers = [1, 5, 10, 15];
	var doubles = numbers.map(function(x) {
	   return x * 2;
	});
	// doubles is now [2, 10, 20, 30]
	// numbers is still [1, 5, 10, 15]

	var numbers = [1, 4, 9];
	var roots = numbers.map(Math.sqrt);
	// roots is now [1, 2, 3]
	// numbers is still [1, 4, 9]
	
###### reduce

reduce()方法对累加器和数组中的每个元素(从左到右)使用一个函数，以将其还原为一个值。

arr.reduce(callback[, initialValue])
callback：回调函数。

initiaValue：当前元素

返回值

数组的累加

	// create an array
	var numbers = [0, 1, 2, 3];

	/* call reduce() on the array, passing a callback
	that adds all the values together */
	var result = numbers.reduce(function(accumulator, currentValue) {
		return accumulator + currentValue;
	});

	// log the result
	console.log(result);
	// expected output: 6
	
###### some

some()方法检测数组中至少一个元素是否通过所提供回调函数的条件。

arr.some(callback[, thisArg])
callback：回调函数。

thisArg：当前元素

返回值

boolane

	function isBiggerThan10(element, index, array) {
	  return element > 10;
	}

	[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
	[12, 5, 8, 1, 4].some(isBiggerThan10); // true
	
###### splice

splice()方法通过删除现有元素和/或添加新元素来更改数组的内容。

array.splice(start)
array.splice(start, deleteCount)
array.splice(start, deleteCount, item1, item2, ...)
start：开始位置。

deleteCount：如果deleteCount为0，则没有删除元素。在这种情况下，您应该指定至少一个新元素。

item1， item2：添加到数组中的元素，从开始索引开始。如果没有指定任何元素，splice()将只删除数组中的元素。

返回值

包含已删除元素的数组。如果只删除了一个元素，则返回一个元素数组。如果没有删除元素，则返回空数组。

	var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];

	myFish.splice(2, 0, 'drum'); // insert 'drum' at 2-index position
	// myFish is ["angel", "clown", "drum", "mandarin", "sturgeon"]

	myFish.splice(2, 1); // remove 1 item at 2-index position (that is, "drum")
	// myFish is ["angel", "clown", "mandarin", "sturgeon"]
	
###### of

Array.of()方法创建一个新的数组实例，该数组实例的参数数目不定，不管参数的类型或类型。

Array.of(element0[, element1[, ...[, elementN]]])
elementN：创建数组的元素

返回值

返回新函数实例

	Array.of(7);       // [7] 
	Array.of(1, 2, 3); // [1, 2, 3]

	Array(7);          // [ , , , , , , ]
	Array(1, 2, 3);    // [1, 2, 3]

	Array.of(1);         // [1]
	Array.of(1, 2, 3);   // [1, 2, 3]
	Array.of(undefined); // [undefined]


	// 实现原理（兼容）
	if (!Array.of) {
	  Array.of = function() {
		return Array.prototype.slice.call(arguments);
	  };
	}	


	
转载：https://juejin.im/post/59c9f03df265da06602994f7?utm_source=gold_browser_extension