---
title: 'es6: Map & Set'
date: 2017-09-23 11:22:37
tags: Map Set
categories: es6
---

### Set 数据结构

####  基本属性

Set 数据结构类似于数组， 不同于数组的是， Set 数据结构中不允许存在重复的值。

```
let arr = [1,2,2,3,4];
let set = new Set(arr);
console.log(arr) // {1,2,3,4} 去除重复数字
```

在上面的程序中， 使用 new Set构造函数生成的 `set` 的类型是一个对象，并且通过使用 `set` 结构进行处理之后的数据中没有重复数据。

使用 set 进行数组去重的方法

```
arr = [...new Set([arr])]//  [1,2,3,4]
//如上所示，完成了数组的去重操作
```

#### Set 实例的属性和方法, 

 如下图所示

![](es6-Map-and-Set/Set.png)

`let set = new Set([1,2,3,4])`

`set.prototype.constructor`  : 默认 `set` 函数

`set.size` 表示 `set` 数据结构中成员的个数

```
set.size // 4
```

对于 `set` 结构的方法，分为操作方法和遍历方法

操作方法:

`add()` 用于向 `set` 数据机构中添加成员

`delete()` 用于删除数据中的某个值，返回布尔值， 表示是否删除成功

`has()` 表示判断数据结构中是否含有某个值，返回的是布尔值，表示是否含有

`clear()` 表示清除所有的成员，没有返回值

```
set// {1,2,3,4}
set.add(5)// {1,2,3,4,5}
set.has(5) // true
set.delete(5) // true 删除成功
set.has(5) // false
set.clear()
set // {}
```

一个数组去重操作:

```
function removeDeu (Array) {
  return [...new Set(Array)] // 换成 Array.from(new Set(Array)) 也是可以的
}
使用 Array.from 也可以将Set 结构转换为数组结构
```

#### Set 实例的遍历方法

`keys()`  返回键名的数组

`values()` 遍历键值

`entires()` 遍历键值对儿

因为 `Set()` 结构只有键值，没有键名，因此使用 `values()` 和 `keys()` 的遍历结果是一样的

```
let arr = [1,2,3,4];
let set = new Set(arr);
for (let key of set.keys()) {
  console.log(key)
}
// 1,2,3,4,5
```

对于set 结构可以转换为 数组结构,可以应用数组的 `map` 等方法

```
let arr = [...set];
```

### Map 数据结构

#### 基本属性

对于对象而言，对象是有键值对的数据结构组成的，但是对于对象的键，他的格式会被转换为字符串， 即便我们传入了一个数组或者对象

```
let obj = {};
let o = {a: 1};
obj[o] = 'content';
for (let key in obj) {
	console.log(typeOf key)
}
// string
```

使用 `map` 结构可以实现对象的键不再必须是字符串的形式

创建 `map` 结构:

```
let map = new Map();
```

#### `map ` 属性的方法

通过 console　控制台打印数据如下

![](es6-Map-and-Set/map.png)

类似于　`Set` 结构, 存在 `set` `get` `delete` `has` `clear` 等几种操作方法

![](es6-Map-and-Set/map%20操作数据.png)

`set(键， 值)` 写入数据

`get(键)` 从数据结构中获得对应键的值

>虽然 `typeof map` // Object 以及  `map instanceof Object === true` 但是， Map 是一种不同于对象的数据结构，在对象上使用 [ ] 访问属性的方法在 map 数据结构中是不适用的，要获得 `Map` 数据结构中的值， 我们是通过使用 `get` 方法，后面我们会说如何将 map 数据结构转换为对象

`delete(键)` 删除对应键的值

`clear()` 将 `map` 数据中的所有数据清除

`has()` 返回布尔值, 表示是否含有某条数据

使用 `map` 的数据结构不同于对象的是，通过使用 `map` 添加的键名可以是变量

```
let map = new Map();
let obj = {a: 1};
map.set(obj, 'good');
map.get(obj) // 'good'
for (let key of map) {
	console.log(typeof key) // object
}
```

可以是数组:

```
let arr = [1];
map.set(arr, 'good');
map.get(arr)// 'good'
```

>需要注意的一点，通过使用 `get()` 获取键值的时候，最终是查找到了键值的内存地址

代码如下:

```
map.set({a:1}, 'good');
map.get({a:1})// undefined
```

在上面的代码中，虽然查找的键值是一样的，但是两个 `{a:1}` 实际上的内存地址是不一样的。使用下面的代码可以找到对应的键值，因为变量 obj 指向了同一个对象的内存地址。

```
let obj = {a: 1};
map.set(obj, 'good');
map.get(obj) // 'good'
```

***

关于对象及内存地址:

每创建一个对象，总会创建一个内存地址，对于下面对象:

`let person = { name: '张宁宁' }` 

我们使用 `person.name` 来访问到值的，但是，对象的值并没有存放在`person` 容器内，`person` 中存放了对象的属性，

这个属性相当于指针，指向存放 `张宁宁` 的内存地址

例如:

`{} === {}` // false

因为这两个空对象的内存地址是不一样的，所以是不相等的

***

使用 `map` 结构可以传入数组，数组中的成员是存在两个元素的数组结构。其中第一个元素会被当作键名，第二个元素会被当作键值

```
let map = new Map([['name', '张宁宁']]);
map.get('name') // '张宁宁'
```

#### map 对象的遍历方法

对于 `map` 结构的遍历，存在下面几种方法

`keys()` 返回键名的遍历器

`values()` 返回键值的遍历器

`entires()` 返回所有成员的遍历器

`forEach()` 类似于数组中的 `forEach()` 方法， 实现对于 `map` 数据结构的遍历

使用 `keys()` `values()` 以及 `entires()` 返回的是类数组对象

```
let map = new Map();
map.set('h', 'hello');
map.set('g', 'good');
map.keys() // {'h', 'g'};
map.values() // {'hello', 'good'};
map.entires(); // {'h': 'hello', 'g': 'good'};
```

使用扩展运算符可以将类数组对象转化为数组对象，从而对于类数组对象上应用各种方法。

```
[...map.keys()] // ['h', 'g']
```

#### `Map` 与各种数据结构的相互转化

1. 对象转为 `map`

   ```
   function toMap (obj) {
     let map = new Map();
     for (let key in obj) {
       map.set(key, obj[key]);
     }
     return map;
   }
   ```

2. `map` 转化为对象

   ```
   function toObj (map) {
     let obj = Object.create(null);
     for (let [k,v] of map) {
       obj[k] = v;
     }
     return obj;
   }
   ```

   ​