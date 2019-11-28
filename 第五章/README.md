引用类型
----------------

### 数组
创建数组的方法就不赘述了，应该都知道了，下面记一下检测数组的方法：

1. instanceof
2. Array.isArray
3. Object.prototype.toString.call

```javascript
const arr = [];
console.log(arr instanceof Array);  // true
console.log(Array.isArray(arr));    // true
Object.prototype.toString.call(arr) // [object Array]
```
注意 typeof 运算符是不能实现上述操作的，他只会返回 object

**转换数组的方法**

1. toString()
2. valueOf()
3. join()

```javascript
const person = [ { name: 'Hans', age: 18 }, { name: 'Alsa', age: 22 } ];
const arr = [1, 2, 3];

console.log(person.toString()); // [object Object],[object Object]
console.log(arr.toString()); // 1,2,3

console.log(person.valueOf()); // 返回原数组 [ { name: 'Hans', age: 18 }, { name: 'Alsa', age: 22 } ]
console.log(arr.valueOf()); // 返回原数组

console.log(person.join("-")); // [object Object]-[object Object]
console.log(arr.join("-")); // 1-2-3
```
根据上面的例子来解释这几个方法，首先 `toString()` 方法，Array 上面的 `toString()` 方法在调用时，会返回一个字符串。字符串由数组中的每一项拼接而成，中间由逗号隔开，那么如果数组中的项有 object 对象的话，如果这个
对象中没有对 `toString()` 方法进行重写，那么就会返回 `[object Object]` ，所以在上例中调用 `person.toString()` 返回的是 `[object Object],[object Object]`，而调用 `arr.toString()` 的时候，返回的是
`1,2,3`，我自己测试了一下，发现数组里面不同的类型的项在调用 `toString()` 会有大概下面几种情况：
```javascript
console.log([null, null].toString()); // ","
console.log([undefined, undefined].toString()); // ","
console.log([function foo(){return 1}, function foo1(){return 2}].toString()); // function foo(){return 1},function foo1(){return 2}
```
总体来说，如果是对象，而且这个对象中没有覆盖原型上的 `toString()` 方法，会把这个对象转为 `[object Object]`, 如果是 `null` 或者 `undefined`，转为空，其余的就都是化成字符串，最后拼在一起。  

`valueOf()` 方法相对简单，直接返回原数组。

`join()` 方法会将数组的每一项按照传入的参数拼接起来，默认是按照逗号拼接起来，与 `toString()` 方法是一样的，但与 `toString()` 方法不一样的就在于它可以将拼接的字符自定义化。   

需要注意的是，上面是哪个方法都不会改变原本的数组，只是返回一个值，原数组不变

<hr />

**栈方法**

1. pop()
2. push()

```javascript
let stack = [];

stack.push("item");
stack.push("item1");
stack.push("item2");
console.log(stack);          // ["item", "item1", "item2"]
console.log(stack.pop());    // item2
console.log(stack);          // ["item", "item1"]
```
`push()` 方法向数组的末尾( 栈顶 )添加元素(入栈)，方法返回 push 后数组的长度，push 以后数组长度自动加上 push 进去的元素的个数。   
`pop()` 方法去掉数组末尾( 栈顶 )的一个元素(出栈)，方法返回 pop 掉的项(也就是返回最后一项)，同时数组长度减一。   

**注意：以上两个方法会修改原数组**

<hr />

**队列方法**

1. shift()
2. unshift()

`shift()` 方法可以移除数组的第一项并返回该项，同时数组长度自动减一，结合 `push()` 和 `shift()`，可以用数组实现队列(先进先出)的功能。   
`unshift()` 方法项数组的最前面添加任意个项并返回新的数组的长度，结合 `pop()` 可以实现反方向的队列。  

```javascript
let queue = ["red", "blue", "yellow"];

queue.push("black");
console.log(queue.shift()); // red

queue.unshift("green", "purple"); // 5
console.log(queue); // ["green", "purple", ""blue", "yellow", "black"]
```

**注意：以上两个方法会修改原数组**

<hr />

**重排序方法**

1. reverse()
2. sort()

`reverse()` 方法会翻转数组的顺序。   
`sort()` 方法相对比较复杂，首先，该方法会调用数组每一项的 `toString()` 方法，也就是说，它比较的一定是字符串，在不传入任何函数的情况下，默认按照“升序”排列数组项，但因为是比较的字符串，所以比较的是字符的 ASCII 码：
```javascript
var values = [0, 1, 5, 10, 15]; 
values.sort(); 
alert(values); //0,1,10,15,5 
```
注意上面不是按照数字大小来排列的，而是按照字符串的 ASCII 码的值的大小来比较的。   
除此之外，对 `sort` 更好的应用是传入一个比较函数，通过这个函数的返回值来对数组进行排序，这个函数称为**比较函数**，它接收两个参数，如果第一个参数小于第二个之前则返回一个负数，如果两个参数相等
则返回 0，如果第一个参数大于第二个之后则返回一个正数：
```javascript
function compare(value1, value2) { 
    if (value1 < value2) { 
        return -1; 
    } else if (value1 > value2) { 
        return 1; 
    } else { 
        return 0; 
    } 
}

const values = [0, 1, 5, 10, 15]; 
values.sort(compare);
alert(values); //0,1,5,10,15
```
这样排序出来是升序，如果想降序，只需要在第一个参数小于第二个参数时候返回一个正数，而第一个参数大于第二个参数的时候返回一个负数即可。   
对于数值类型或者者其 valueOf()方法会返回数值类型的对象类型，可以将比较函数简写为：
```javascript
function compare(value1, value2){ 
 return value2 - value1; 
}
```

**同样，两个方法都会修改原数组**   

<hr />

**操作数组的方法**

1. concat()
2. splice()
3. indexOf()
4. lastIndexOf()
5. every()
6. filter()
7. forEach()
8. map()
9. some()
10. reduce()
11. reduceRight()

**1. concat()**

作用于数组当中，`concat()` 方法用于连接数组，或者说拼接数组，它可以接收任意类型的参数，然后它会把接收来的每一个参数都添加到对应数组的尾部，如果传入的是数组，那么就把数组的每一项按个放到对应数组的尾部，该方法会返回创建的新数组，同时不会修改原数组：
```javascript
let arr6 = [1,2];
let arr7 = arr6.concat(3, [4, 5], "15", { num: 1 }, undefined, [{name: 1}, {age:2}])
console.log(arr7); //  [1, 2, 3, 4, 5, "15", {…}, undefined, {…}, {…}]
```

**2. slice()**
