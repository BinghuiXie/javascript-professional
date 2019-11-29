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

### RegExp 类型

**创建一个正则表达式**

 一种方式是字面量模式创建正则表达式：
```javascript
const reg = / pattern / flags ;
```
pattern 部分可以使任何正则表达式，flags 包含以下三个标志：
- g： 全局搜索
- i：不区分大小写
- m：多行匹配

```javascript
const pattern1 = /[bc]at/i
```

另一种创建正则表达式的方法是通过 `RegExp` 构造函数，它接收两个参数：一个是要匹配的字符串模式，另一个是可选的标志字符串(i,g,m):
```javascript
const pattern2 = new RegExp("[bc]at", "i");
```
需要注意的是，`RegExp`构造函数内传入的两个参数都是字符串，所以对于一些元字符需要进行双重转义，例如，字符\在字符串中通常被转义为\\\\，而在正则表达式字符串中就会变成\\\\\\\\。   

### Function 类型

JS中常用创建函数有两种形式：函数声明，函数表达式。   

**函数声明**

```javascript
function sum(num1, num2) {
  return num1 + num2;
}
```

**函数表达式**

```javascript
const sum = function(num1, num2) {
  return num1 + num2;
};
```

上面两种形式中，因为函数是对象，所以其函数名 sum 其实保存的是一个指针，或者是一个引用，其值是这个函数对象在堆内存中的地址，这就像之前写到的变量保存 object 只是保存了其在堆内存中的地址而已，保存的不是实际的对象，所以，这就允许我们进行如下操作：
```javascript
let sum = function(num1, num2) {
  return num1 + num2;
};
console.log(sum(10, 20)); // 30

const anotherSum = sum;
console.log(anotherSum(10, 20)); // 30

sum = null;
console.log(anotherSum(10, 20)); // 30
```
**有一点需要注意一下，直接使用不带圆括号的函数名只是访问函数指针，而不是调用函数**   
JS 中的函数式没有重载的，同名函数只会是后面定义的覆盖前面的，而且因为 JS 中函数的参数本来也不是必须的，就算没有形式参数，调用函数时传入的实参也会在 arguments 对象里面找到，所以说不存在重载这一概念：
```javascript
function reload() {
  const argus = [...arguments];
  console.log(argus);
}
// 这样也是可以传入参数的，只不过在函数内部职能通过 arguments 对象访问到传入参数而已
console.log(reload(1,2,3,4,5)); // [1, 2, 3, 4, 5]
```

**函数声明与函数表达式的区别**

函数声明与函数表达式只有一个很简单的区别：函数声明会存在函数声明提升而函数表达式不存在，看一个例子：
```javascript
// 函数声明
console.log(sum1(10, 20));  // 30
function sum1(num1, num2) {
  console.log(num1 + num2);
}

// 函数表达式
console.log(sum2(10, 20));   // Uncaught ReferenceError: Cannot access 'sum2' before initialization
const sum2 = function(num1, num2) {
  console.log(num1 + num2);
}
```
根据上面的例子，函数声明在代码执行前就可以访问了，然而函数表达式在其执行前访问会报错，这是因为对于函数声明，在代码开始执行之前，解析器就已经通过一个名为**函数声明提升**的过程，
读取并将函数声明添加到执行环境中。对代码求值时，JavaScript 引擎在第一遍会声明函数并将它们放到源代码树的顶部。所以，即使声明函数的代码在调用它的代码后面，JavaScript 引擎也能把函数声明提升到顶部。
而对于函数表达式则不存在函数提升，所以在代码执行之前访问对应函数会报错。

<hr />

**函数内部的属性**

1. `arguments.callee`
2. `this`

arguments 对象是函数内部一个接受传入参数的类数组对象，其含有一个 `callee` 属性，这个属性包含当前正在执行的函数，它可以在函数内部被用来访问当前正在执行的函数，通常来说，当函数是匿名函数的时候，`arguments.callee` 会
很有用，尤其是在匿名函数完成的是一个阶乘的计算的时候：
```javascript
// 对于非匿名函数，可以这么写
function factorial (n) {
    return !(n > 1) ? 1 : factorial(n - 1) * n;
}

[1, 2, 3, 4, 5].map(factorial);

// 对于匿名函数，可以这么写
// 此时 arguments.callee 指向的就是当前传入 map 的这个函数
[1, 2, 3, 4, 4].map(function() {
  return !(n > 1) ? 1 : arguments.callee(n - 1) * n;
})
```

this 对象相对来说比较复杂，可以参考这一篇文章：
[ECMA-262-3 in detail. Chapter 3. This.](http://dmitrysoshnikov.com/ecmascript/chapter-3-this/)      

<hr />

**函数的属性和方法**

**常用函数的属性**

1. length
2. prototype

length 属性返回函数形参的个数：
```javascript
function foo(a,b,c,d){
  console.log(arguments.callee.length) 
}
foo();              // 4
foo(1,2,3,4,5,6);   // 4
```
可以看出，函数的 length 属性与实际传入参数的个数无关，其长度是形式参数的个数。

prototype 属性是每一个函数都有的属性，当函数作为构造函数时，通过这个函数创建的实例会将这个构造函数原型 (也就是 prototype ) 上的属性和方法都继承下来：
```javascript
function Person(age) {
  this.age = age;
}
Person.prototype.getAge = function() {
  return this.age;
}

const personA = new Person(12);
console.log(personA.getAge());  // 12
console.log(personA.age);       // 12
const personB = new Person(26);
console.log(personB.getAge());  // 26
console.log(personB.age);       // 26
```
关于更详细的原型，以及原型链的东西，可参考：    
[JavaScript学习心得](http://binghuixie.cn/2019/02/21/JavaScript%E5%AD%A6%E4%B9%A0%E5%BF%83%E5%BE%97/)      

**改变 this 指向的方法： call(), apply() bind()**    

三者都是挂载在 Function 原型上的( 例如：Function.prototype.call() )，当然也可以通过 Function 的实例访问到这三个函数。   
三个函数的作用都是改变函数内部 this 的指向，也就是说在特定的作用域中调用函数，三者只有很小的区别：
- `call()`: 第一个参数为需要绑定的 this 的值(函数运行的作用域)，剩余的参数个数任意，是需要传给所调用函数的参数
- `bind()`: 第一个参数为需要绑定的 this 的值(函数运行的作用域)，第二个参数为一个数组，可以是 Array 的实例，也可以是 arguments 对象
- `apply()`: 第一个参数为需要绑定的 this 的值(函数运行的作用域)，剩余的参数与 call() 一样，但不同的是它会返回一个改变了 this 绑定以后的函数

先来看一个最基础的使用的例子：
```javascript
var num1 = 15, num2 = 25;
function sum() {
  console.log(this.num1 + this.num2);
}
const numObj = {
  num1: 50,
  num2: 60
}

sum();                  // 40
sum.call(numObj);       // 110
```
注意到这里面我用了 var 定义变量而不是 let 或者 const，这是因为 const 和 let 定义的变量不会作为 window 对象的属性，而 var 定义的变量会作为 window 对象的属性，也就是说可以通过 `window.varname` 访问到，因为在全局调用
`sum()` 的时候 this 就是 window， 所以能返回 40，如果使用 let 或者 const 定义，那么就会返回 NaN。     
传参的例子：
```javascript
function sum(num1, num2){ 
 return num1 + num2; 
} 
function callSum1(num1, num2){ 
 return sum.call(this, num1, num2);
} 
function callSum2(num1, num2){ 
 return sum.apply(this, [num1, num2]);
} 
alert(callSum1(10,10)); //20 
alert(callSum2(10,10)); //20
```
bind 方法就不举例了，除了返回一个新的函数，这个函数的 this 指向的是传入 bind 的那个 this 的指向，其他的都和 call 一样。React 里面 bind 用的比较多。   
三个方法最常用的就是扩展函数的运行作用域：
```javascript
window.color = "red"; 
const o = { color: "blue" }; 
function sayColor(){ 
 alert(this.color); 
} 
sayColor();             //red   全局作用域调用
sayColor.call(this);    //red   全局作用域调用 
sayColor.call(window);  //red   全局作用域调用
sayColor.call(o);       //blue   o 对象作用域调用
```

<hr />

**基本包装类型**

基本包装类型的作用在于为基本类型包装对应的对象，这样就可以在这些基础类型上调用一些方法了：
```javascript
const str = "some text";
const str2 = str.toUpperCase();
```
上例中，如果 str 只是一个单纯的字符串的话，是没有可能通过这个字符串调用方法的，实际上，在代码执行到第二行的时候，后台会自动完成以下几步过程：
- 创建一个 String 类型的实例
- 在实例上调用制定的方法
- 销毁这个实例

所以上面的例子可以想象成是按照这样来执行的:
```javascript
let s1 = new String("some text"); 
const s2 = s1.substring(2); 
s1 = null;
```
这样，字符串就变得和对象一样了。这样的自动包装也同样适用于 `Boolean` 和 `Number`。    
虽然和对象一样，但是却不能为这个字符串添加属性或者方法：
```javascript
s1.color = "red";
console.log(s1.color); // undefined
```
这是因为第二行创建了基本包装的实例以后，执行完这一行代码就会立即销毁这个实例，在第三行的时候又会创建一个新的实例，这个新的实例没有 color 属性，所以是 undefined。   