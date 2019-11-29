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

1. `concat()`
2. `splice()`
3. `splice()`
4. `indexOf()`
5. `lastIndexOf()`
6. `every()`
7. `filter()`
8. `forEach()`
9. `map()`
10. `some()`
11. `reduce()`
12. `reduceRight()`

**1. concat()**

作用于数组当中，`concat()` 方法用于连接数组，或者说拼接数组，它可以接收任意类型的参数，然后它会把接收来的每一个参数都添加到对应数组的尾部，如果传入的是数组，那么就把数组的每一项按个放到对应数组的尾部，该方法会返回创建的新数组，同时不会修改原数组：
```javascript
let arr6 = [1,2];
let arr7 = arr6.concat(3, [4, 5], "15", { num: 1 }, undefined, [{name: 1}, {age:2}])
console.log(arr7); //  [1, 2, 3, 4, 5, "15", {…}, undefined, {…}, {…}]
```

<hr />

**2. slice()**

用法：截取数组，该方法会返回一个新的数组，不会影响原数组。   
首先，在什么都不传的情况下，返回的新数组与原数组一样，`slice()` 函数最多可以接受两个参数，分别表示截取的开始位置和结束位置(传入参数对应数组下标)，但是截取出来的数组不会包括结束位置的项，`slice()` 会截取开始位置到结束位置减一的所有项放到系数组中，
如果传入的参数有负数，就把该负数加上数组的长度再来确定位置，如果结束位置小于等于开始位置，返回空数组：
```javascript
const arr3 = [1,2,3,4,5,6,7,8,9,10];
console.log(arr3.slice(2));         // [3, 4, 5, 6, 7, 8, 9, 10]
console.log(arr3.slice(2, 10));     // [3, 4, 5, 6, 7, 8, 9, 10]
console.log(arr3.slice(-4, -1));    // [7, 8, 9] 等价于 arr3.slice(6, 9)
console.log(arr3.slice(4, 2));      // []
```

<hr />

**3.splice()**

`splice()` 可以对数组进行删除，插入和替换的操作，同时会修改原数组。
**删除：** 可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数，会以数组的形式返回删除的项。   
**插入：** 可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。    
**替换：** 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。     
```javascript
let colors = ["red", "green", "blue", "white", "black", "pink", "brown"];

// 删除
console.log(colors.splice(0, 2)); // ["red", "green"]
console.log(colors); // ["blue", "white", "black", "pink", "brown"]

// 插入
colors.splice(0, 0, "red", "green"); // 从第 0 个位置开始插入两项  返回一个空数组，因为没有被删除的
console.log(colors); // ["red", "green", "blue", "white", "black", "pink", "brown"]

// 替换
colors.splice(2, 1, "white"); // 将下标为 2 的元素替换为 white
console.log(color); // ["red", "green", "white", "white", "black", "pink", "brown"]`
```

<hr />

**4. indexOf() 和 5. lastIndexOf()**

作用：用于查找某一个值是否在数组中存在，`indexOf()` 从前向后查找，`lastIndexOf()` 从后向前查找，如果找到就返回对应项的下标，没有找到就返回 -1。   
**注意：两个方法都是只找到第一个存在的值以后就停止，不在向前（或向后）查找。**   
```javascript
const numbers = [1,2,3,4,5,4,3,2,1];

console.log(numbers.indexOf(4)); // 3 没有找第二个 4 ，找到第一个就停止
console.log(numbers.lastIndexOf(4)); // 5
``` 
上面只是列举了基本类型作为距离，但是当数组中包括引用类型的时候，结果可能会出人意料：
```javascript
const users = [
  { id: 0, age:21, name: "Hans" },
  { id: 1, age:26, name: "Anna" },
  { id: 2, age:32, name: "Baggins" }
]

const user = { id: 0, age:21, name: "Hans" };

console.log(users.indexOf(user));  // -1
```
这里为什么是 -1 呢，其实在前面的章节里已经有说过了，那么因为 `indexOf()` 和 `lastIndexOf()` 使用的是全等( === )，然而引用类型实际上比较的是存储在栈内存中的地址，虽然说两个对象的内容一样，但是这两个对象在堆内存中的地址不一样，
所以存储在栈内存中的值也就是二者的地址是不一样的，所以不会找到对应的项，再多说一句，虽然我们看起来他们的内容一样，但是在解释器眼里是看不到他们一样的。   

<hr />

**迭代方法**

JS 中数组的迭代方法有：`every()`, `filter()`, `some()`, `map()`, `forEach()`，这些方法都接收两个参数，第一个是运行在每一项上的一个函数，第二个(可选)是该函数运行时的 this 的指向，
传入的函数可接收三个参数：当前项，当前项的下标和数组本身：

- every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。 
- filter()：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
- forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。
- map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
- some()：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。

<hr />

**归并方法 reduce() 和 reduceRight()**

`reduce()` 和 `reduceRight()` 很相似，只不过一个从左开始遍历数组，一个从右遍历数组，下面都以 `reduce()` 来举例。    
方法接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。    
传给 `reduce()` 的函数接收 4 个参数：前一个值、当前值、项的下标和数组本身。这个传入函数的返回值会作为第一个参数自动传给下一项：
```javascript
// 利用 reduce 来实现数组求和
const values = [1, 2, 3, 4, 5];
const sum = values.reduce((prev, cur, index, self) => {
  return prev + cur;
}, 0)
console.log(sum); // 15
```
后面传入的那个 0(初始值) 是作为第一次调用传入函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。   


 

