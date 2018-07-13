### ES6 展开运算符 Spread operator

展开运算符（spread）是三个点（…）,可以将数组转为用逗号分隔的参数序列。如同函数不定参数的逆运算。

```
console.log(...[1, 2, 3])
// 1 2 3
console.log(1, ...[2, 3, 4], 5);
// 1 2 3 4 5
```

> 注意：任何实现了Iterator接口的对象，都可以用展开运算符转为真正的数组。

* 应用场景：

1) 函数调用

```
function func(x, y, z, a, b) {
    console.log(x, y, z, a, b);
}

var args = [1, 2];
func(0, ...args, 3, ...[4]);
// 0 1 2 3 4
```

2) 替换apply方法

```
function func(x, y, z) { }
var args = [0, 1, 2];

// ES5写法
func.apply(null, args);

// ES6写法
func(...args);
```

3) 数组字面量操作

```
var parts = ['shoulders', 'knees'];
var lyrics = ['head', ...parts, 'and', 'toes'];
// ["head", "shoulders", "knees", "and", "toes"]
```

4) 合并数组

```
var arr1 = [0, 1, 2], arr2 = [3, 4, 5];
// ES5写法
Array.prototype.push.apply(arr1, arr2);
// ES6写法
arr1.push(...arr2);
arr1 // [0, 1, 2, 3, 4, 5]

arr1.unshift(...arr2) // 将arr2 追加到数组的开头
```

5) 拷贝数组

```
var arr = [1,2,3];
var arr2 = [...arr]; // 和arr.slice()差不多
```

6) ES7对象解构、合并

```
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }

 let a={x:1,y:2};
 let b={z:3};
 let ab={...a,...b};
 console.log(ab); //{x:1,y:2,z:3}
```