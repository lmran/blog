# 实用小技巧

本文首发于掘金： 地址： [3分钟掌握JS开发中20个实用小技巧](https://juejin.im/post/5e9128fa518825738943b8c6)

### 前言

日常开发中，我们可能需要编写大量的js代码，  下面的这些开发小技巧可以帮助到你。话不多少， 直接上代码！

### 字符串技巧

> 格式化千分位

```js
'10000000.00'.replace(/(?!^)(?=(\d{3})+\.)/g, ',');  	// "10,000,000.00"
'10000000.00'.replace(/\B(?=(\d{3})+\.)/g, ',');		// "10,000,000.00"
'10000000.00'.replace(/\d{1,3}(?=(\d{3})+\.)/g, '$&,'); // "10,000,000.00"
```

> 日期比较,  时间为个位数字前面补0

```js
let date1 = '2020-01-01 08:34:59'
let date2 = '2020-04-01 18:34:59'
data2 > date1 // true
```

> 补零

```js
const addZero1 = (num, len = 2) => `0${num}`.slice(-len)
addZero1(3) // 03
const addZero2 = (num, len = 2) => `${num}`.padStart(len, 0)
addZero2(5) // 05
```

```!
这里需注意这两种用法的区别:  方法一会截取固定的长度， 方法二超出指定长度，直接返回原数值
```

### 数值技巧

> 时间戳

```js
const timestamp = +new Date('2020-02-02')  // timestamp => 1580601600000
```

> 精确小数位

```js
const roundNum = (num, decimal) => Math.round(num * 10 ** decimal) / 10 ** decimal
roundNum(1.768, 2)  // 1.77
```

> 判断奇偶

```js
const a = 5
!!(a & 1)  // true
!!(a % 2)  // true
```

>  取整：正数相当于`Math.floor()`，负数相当于`Math.ceil()` 

```js
const a1 = ~~1.333    // 1
Math.floor(1.333)     // 1

const a2 = ~~-1.333   // -1
Math.ceil(-1.333) 	  // -1	

const b1 = 1.333 | 0  // 1
const b2 = -1.333 | 0 // -1

const c1 = 1.333 >> 0 // 1
const c2 = -1.333 >> 0 // -1
```

> 进制转换   

```js
num.toString(n)    // 十进制 => n进制 
(13).toString(12)  // 11
parseInt(num, n)   // n进制  => 十进制
parseInt(12, 8)	   // 12
```

- 扩展实现整数不用加减乘除就能扩大n (2~36) 倍（这个似乎还是个面试题）

  ```js
  const convertN = (num, n) => parseInt(`${num.toString(n)}0`, n)
  convertN(12, 7) // 84
  ```

### 数组技巧

> 过滤数组所有的假值,  js中的假值有`false`、`null`、`undefined`、`空字符`、`0`和`NaN` 

```js
const filterArr = arr => arr.filter(Boolean)
filterArr([1, false, null, undefined, '', NaN])   // [1]
```

> 数组求和

```js
const arr = [1, 2, 3, 4];
arr.reduce((total, v) => total + v) // 10
```
> 数组最大最小值
```js
Math.min(...[1, 2, 3, 4]) // 1
Math.max(...[1, 2, 3, 4]) // 4
```

> 数据扁平化

```js
const flatten = (arr, n = 1) => 
	n > 0 
	? arr.reduce((a, v) => a.concat(Array.isArray(v) ? flatten(v, n - 1) : v), [] )
	: arr

flatten([1, 2, [1, [3]], [4]]) // [1, 2, 1, Array(1), 4]
flatten([1, 2, [1, [3]], [4]], 2) // [1, 2, 1, 3, 4]

```

>  交换赋值 

```js
let a = 1;
let b = 2;
[a, b] = [b, a]
// a => 2, b => 1
```

- 其它实现方法

  ```js
  1、var temp = a; b = a; a = temp;
  2、b = [a, a = b][0]
  3、a = a + b; b = a - b; a = a - b;
  4、a ^= b; b ^= a; a ^= b;
  ```

> 统计数组成员个数

```js
const arr = [0, 2, 3, 2, null, ''];
const obj = arr.reduce((t, v) => {
    t[v] = t[v] ? ++ t[v] : 1;
    return t;
}, {});
// obj => {0: 1, 2: 2, 3: 1, null: 1, "": 1}
```

>  创建指定长度数组 且每项都相同

```js
new Array(5).fill(true) // [true, true, true, true, true]
```

>  异步累计 

```js
async function fn(arr) {
    return arr.reduce(async(t, v) => {
        const dep = await t;
        const res = await fn2(v);
        obj[v] = res;
        return obj;
    }, Promise.resolve({}));
}

const result = await fn();
```

> 数组去重

```js
const arr = [...new Set([0, 1, 1, null, '', '',,])];  
// arr => [0, 1, null, "", undefined]
```

> URL参数拼接

```js
const obj = { a: 1, b: 2 }
Object.entries(obj).map(param => param.join('=')).join('&') // "a=1&b=2"
Object.keys(obj).map(key => `${key}=${obj[key]}`).join('&') // "a=1&b=2"
```



### 对象技巧

> 结构提取出需要的属性

```js
const { a, ...b } = { a: 1, b: 2, c: 3 }
// a => 1, b => { b: 2, c: 3 }
```

###  总结

 以上基本上是开发中常使用的小技巧，如有发现有遗漏我会及时更新上去的， 同时也希望大家在下方进行评论或补充喔，喜欢的可以**点个赞**或**收藏一下**，也许哪天就用得上了。 