JavaScript基本概念
-------------------------

### 一、语法
1. 标识符(变量，函数，参数等)：一般建议采用驼峰
2. 严格模式：**"use script"**

### 二、关键字和保留字
常用关键字：`break`, `case`, `catch`, `default`, `delete`, `do`, `else`, `for`, `function`, `if`, `instanceof`, `new`, `return`, `switch`, `this`, `throw`, `try`, `typeof`, `var`, `void`, `while`。   
常用保留字：`import`, `const`, `class`, `extends`, `static`, `super`, `throws`, `let`, `yield` (let 和 yield 是第五版新增的保留字)。   
**关键字和保留字都不能用作标识符。**   

### 三、变量
如果不用 var 定义一个变量，就会创建一个全局变量
```javascript
function foo() {
        message = "global variable";
    }
foo();
console.log(message);
```
这样虽然可以创建一个全局变量，但是有很多限制，因为，只有将 foo 调用以后才会创建 message 全局变量，否则在调用 foo 之前打印 message 就会报错，这样对应的变量不会马上有定义就会造成混乱。   

### 四、数据类型
在 ES6 之后，有 7 种数据类型：`Undefined`, `Null`, `Boolean`, `Number`, `String`, `Object` 和 `Symbol`

使用 typeof 运算符检测变量的类型的时候，有以下几种情况：   
1. typeof 1 ==> number
2. typeof "test" ==> "string"
3. typeof function foo() {} ==> "function"
4. typeof false ==> "boolean"
5. const a = [], typeof a[100] ==> "undefined" 值未定义会返回 undefined
6. typeof **null** ==> **"object"** // null 会被认为是一个空对象的引用
7. typeof **[]** ==> **"object"**
8. typeof **new Array()** ==> **"object"**
9. typeof **{}** ==> **"object"**

判断一个值是不是 undefined：
1. a === undefined
2. typeof a === "undefined"

用 == 判断 null 和 undefined 总是会返回 true，这是因为 undefined 值实际上是派生自 null 的。   
```javascript
console.log(null == undefined);
```
**注意：如果定义的变量将来是准备保存一个对象，那么初始化的时候建议将其设为 null，因为 null 表示一个空对象指针**。   

JS 的几种数据类型都可以转为 Boolean 类型，可以手动转换，也可以自动转换，具体转换后对应值如下：   
1. undefined ==> false
2. null ==> false
3. "" ==> false
4. 0 ==> false
5. NaN ==> false

剩下的所有任何类型的值转完都是 true    
手动转换可以通过 `Boolean` 函数进行转换，如：
```javascript
console.log(Boolean(undefined)); // false
console.log(Boolean(null)); // false
console.log(Boolean("test")); // true
```
自动转换有很多场景，最常见的比如果条件控制语句和 **!** 运算符，如：
```javascript
const message = "test";
if (message) { alert("message is true") }

console.log(!undefined); // true
```
上面的代码中， if 里面的 message 被自动转换成为了 true，所以会弹出 message is true。    

<hr />

**JS 中的 Number 类型**

1. js 中的数值进制   
    1. 十进制
    2. 八进制： 0开头，如 070, 023, 如果数字中有超过 7(八进制数字范围 0 - 7 ) 的，那么前导 0 被忽略，该数会被当做十进制数解析。    
        如： 079 会被解析为十进制的 79
    3. 十六进制： 0x开头，如：0xAB,0X1F    
    
**在进行算数运算时，十六进制和八进制数都会被转为十进制数参与运算**

2. 浮点数
    1. 表示： 1.1, .5, 3.125e7, 2.5e-7 ( e 表示科学计数法 )
    2. 误差： 0.1 + 0.2 == 0.3 得到的是 false，因为 0.1 + 0.3 = 0.30000000000000004
    
3. 范围：
    1. ECMAScript 能表示的最大的数为 `Number.MAX_VALUE`，在大多数浏览器中其值为： `1.7976931348623157e+308`       
    2. ECMAScript 能表示的最小的数为 `Number.MIN_VALUE`，在大多数浏览器中其值为： `5e-324`
    3. 超出了这两个范围的数会用 `Infinity` 表示，正数表示为： `Infinity`，负数表示为：`-Infinity`
    
4. NaN (非数值)
    1. 当一个本来要返回数值的操作没有返回数值的时候，就会返回 NaN，NaN 的任何其他操作都会返回 NaN
        ```JAVASCRIPT
        console.log(undefined / 10); // NaN
        console.log(NaN * 100); // NaN
        ```
    2. NaN 与任何值都不相等，包括他自己
    3. isNaN: 判断传入参数是否是数值，包括这个参数能不能被转为数值，如果参数本身是数值或者能被转为数值，则返回**false**，否则返回 **true**
        ```javascript
        console.log(isNaN(NaN)); // true
        console.log(isNaN(10)); // false
        console.log(isNaN("10")); // false
        console.log(isNaN("message")); // true
        console.log(isNaN(true)); // false
        ```
        
5. 数值转换
    1. Number(param)
        Number 函数可以把任意类型的入参转为数字，具体规则如下：
        - 参数是 Boolean，则 true => 1, false => 0
        - 参数是 null, 返回 0 
        - 参数是数字，直接返回其对应的十进制数( 16 => 10, 8 => 10 )
        - 参数是 undefined，返回 NaN
        - 参数是字符串
            - 空字符串：返回 0
            - 只含有数字的字符串(包括十进制、十六进制、整数，浮点数)：十进制原封不动的返回，十六进制返回对应的十进制数字，**如果前面有 0 ，则忽略前面的 0 ，返回后面的数字，也就是说，不会转字符串中的八进制数**
            - 含有其他字符的字符串： 返回 NaN
        - 参数是对象
            先调用对象的 `valueOf` 方法，根据 `valueOf` 返回的值按照上面的规则来转，如果转换的结果是 NaN，则调用对象的 `toString()` 方法，然后再次依照前面的规则转换返回的字符串值。  
            
    ```javascript
    console.log(Number("123a")); // NaN
    console.log(Number("10")); // 10
    console.log(Number("00000123")); // 123
    console.log(Number("0123")); // 123
    console.log(Number("0xA")); // 10 ( 16进制 A 对应 10进制的 10 )
    console.log(Number("")); // 0
    console.log(Number(null)); // 0
    console.log(Number(true)); // 1
    console.log(Number({})); // NaN
    console.log(Number({valueOf: function(){return 1234}})); // 1234
    ```
    
    **注： 一元操作符 + 也可以起到和 Number() 函数一样的作用，例如  +"12"  ==>  12， 换句话说，Number(param) <==> +param**     
    
    2. parseInt(param)     
        parseInt 函数只能转字符串( 当然还有 Number )，转所有其他的类型都会返回 NaN。它的转换规则如下：   
        - 空字符串返回 NaN
        - 忽略字符串前面的空格
        - 如果字符串首字符不是数字，返回 NaN
        - 如果是数字，会向后查找，直到遇到一个不是数字的(包括小数点)，返回期间的数字并忽略后面的内容
        - 十六进制转为其对应的十进制
        - 在 ES5 的引擎中，parseInt 不在具有解析八进制的字符串的能力，会直接忽略前面的 0 ，返回后面的数字
            ```javascript
            console.log(parseInt("1234abc")); // 1234 (忽略后面的 abc )
            console.log(parseInt("")); // NaN
            console.log(parseInt("59.3")); // 59
            // 转八进制
            console.log(parseInt("0123")); // 123
            console.log(parseInt("0487")); // 487 
            console.log(parseInt("00000000123")); // 123
            ```
        parseInt 还支持第二个参数，表示按照多少进制解析该字符串，默认是按照十进制转字符串，第二个参数在解析八进制的时候会起到作用，不加第二个参数不会解析八进制数字
        ```javascript
        // 可以省略 0
        console.log(parseInt("0123", 8));  // 83
        console.log(parseInt("123", 8)); // 83
        // 默认按照十进制解析
        console.log(parseInt("123")); // 123
        console.log(parseInt("0123")); // 123
        
        // 可以省略 0x
        console.log(parseInt("0x123", 16)); // 291
        console.log(parseInt("123", 16)); // 291
        
        // 注意如果前面是 0x 但是第二个参数是 8 ，那么返回的是 0 ，因为读到 x 以后就停止了
        console.log(parseInt("0x123", 8)); // 0
        ```
        可以看出，添加了第二个参数以后，如果是 16进制 或者 8进制，那么八进制数的前导 0 和十六进制的前导 0x 可以省略
    
    3. parseFloat
        parseFloat 是解析浮点数的，不会忽略字符串中的第一个小数点，但是后面的小数点以会被忽略，转任何十六进制的数都会返回 0 
        ```javascript
        console.log(parseFloat("12.23.34")); // 12.23
        console.log(parseFloat("0xA")); // 0 
        ```
<hr />

**JS 中的 String 类型**

1. ECMAScript 中的字符串的特点: **不可变**。**字符串一旦创建，他们的值就不能再被改变，要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量**
    ```javascript
    let str = "Java";
    str = "Script";
    console.log(str); // Script
    // 不能像数组一样改变字符串中的某一位置的值
    str[1] = "B";
    console.log(str); // Script
    ```
    这个例子中，首先创建一个长度为 6 的新字符串，然后在其中填充 Script，然后删除原来的 Java 字符串。
    
2. 字符串的转换
    - toString() 方法     
        除了 null 和 undefined ，剩下所有类型都有一个 toString 方法
        ```javascript
        console.log(Number.prototype.toString);     // ƒ toString() { [native code] }
        console.log(Object.prototype.toString);     // ƒ toString() { [native code] }
        console.log(String.prototype.toString);     // ƒ toString() { [native code] }
        console.log(Boolean.prototype.toString);    // ƒ toString() { [native code] }
        ```
        在转换数值类型的时候，toString 接收一个参数，表示将该数值转换为几进制对应的字符串：
        ```javascript
        const num = 10;
        console.log(num.toString(16)); // "a"
        console.log(num.toString(8)); // "12"
        console.log(num.toString(2)); // "1010"
        ```
        **注意上面没有直接写 `10.toString()` 这样会报错，因为数字后面的 "." 会被解析为小数点而不是方法的调用。要想直接用数字的话，可以这么写： **
        ```javascript
        console.log(10..toString(2)); // "1010"
        ```
        这样可以是因为第一个**点**被解析为小数点，和前面的数字连起来被解析为 10，第二个 **点** 被解析为方法的调用，所以可以出结果
    - 转型函数 String(param)
        - 如果被转换的值有 `toString()` 方法，就调用这个方法
        - 如果是 null， 就返回 "null"
        - 如果是 undefined，就返回 "undefined"

<hr />

**JS 中的 Object 类型**

- Object 的每一个实例都具有以下方法:
    - toString()
    - valueOf()
    - constructor()
    - hasOwnProperty(propertyName): 检查对象有没有传入的属性
    - isPrototypeOf(object)
    - toLocalString()
    - propertyIsEnumerable(propertyName)
    
后面的章节有详细的 Object 对象的笔记。

<hr />

### 操作符

**一元操作符**    

- 递增与递减运算符(++, --): 前置和后置操作就不说了，和 C 语言一样，主要看一下他们对其他类型的值的操作
    - 布尔值： true 变为 1， false 变为 0 ，再执行自增或自减的操作
    - 字符串
        - 空字符串：转成 0 以后再操作
        - 只含有数字的字符串，转为数字以后再操作
        - 包含其他字符：返回 NaN    
            ```javascript
            let s = "12";
            let s1 = "012";
            let s2 = "12a";
            let s3 = "";
            console.log(s++); // 12
            console.log(++s); // 13
            console.log(++s1); // 13
            console.log(s2++); // NaN
            console.log(s3++); // 0 (空字符串被转为 0 )
            ```
            **注意，执行过以后，原来的字符串类型现在就变成数字类型了**
    - 对象: 调用 `valueOf()` 方法，根据其返回值进行操作(按照上面的规则)，如果得到 NaN ，再调用 `toString()` 方法 
    - 浮点数: 执行加减一的操作
- 一元加(+)和一元减(-)操作符
    - 一元加( + )操作符
        - 针对 Number 类型：没有任何操作，返回原数字
        - 对于其他类型与 `Number()` 函数的操作一样，具体见上面 `Number()` 函数的转换规则 
    - 一元减( - )操作符
        - 针对 Number 类型：简单的取反(返回对应的负数)
        - 针对其他类型，按照 `Number()` 函数的规则先转换为数字，再取反
        
<hr />

**位操作符**
- 按位非 (NOT) ===>  **~**
    - 返回反码(每一位按位取反)
    - 相当于取反再减一
    ```javascript
    console.log(~123); // -124
    console.log(~24); // -25
    ```
- 按位与(AND) ===> **&**
    - 两个数每一位进行与操作(在两个数值的对应位都是 1 时才返回 1，任何一位是 0，结果都是 0)
- 按位或(OR) ===> **|**
    - 两个数每一位进行与操作(在两个数值的对应位都是 0 时才返回 0，任何一位是 1，结果都是 1)
- 按位异或(XOR) ===> **^**
    - 两个数对应位相同返回 0 ，不同返回 1
- 左移 ===> **<<**
    - 左移 n 位相当于乘以 2 的 n 次方 ( 2 左移 5 位变为 64 )
    - 空出来的位补零
- 带符号右移 ===> **>>**
    - 右移 n 位相当于除以 2 的 n 次幂
    - 空出来的位补符号位
- 无符号右移 ===> **>>>**
    - 对于正数，与带符号数的右移一样
    - 对于负数，是以 0 来填充空位，会把负数的二进制码当成正数的二进制码。
    
**想比于乘 2 与除 2，右移(带符号)和左移比直接的乘法要快，因为是直接操纵的底层的二进制数，同样按位非也比直接取反后再减一快。**

<hr />

**布尔操作符**
- 逻辑非  ===> **!**: 对于任何类型的值，最终都会返回一个布尔值   

| 操作数 | 布尔值 | 取反(!操作数) |
|------ |-------| ----------- |
| 任意对象 | true | false |
| 空字符串 | false | true |
|非空字符串 | true | false |
| 0 | false | true |
| 任意非零数值(包括 Infinity) | true | false |
| null | false | true |
| undefined | false | true |
| NaN | false | true |

```javascript
console.log(!""); // true
console.log(!undefined); // true
console.log(!"message"); // false
console.log(!10); // false
console.log(!NaN); // true
```
可以使用两个逻辑非( **!!** )操作符来实现与 `Boolean()` 函数一样的效果，即把任意一个类型的值转为其对应的布尔值。   

- 逻辑与 ===> **&&**: 双目运算符，只有两个运算符的布尔值都为 true，结果才是 true，否则是 false(同样适用于任何类型)

逻辑与操作属于短路操作，即如果通过第一个操作数已经得到了整个表达式的结果，那么久不会再对后面的操作数进行计算，也就是说，如果第一个操作数是 false，那么不会计算第二个操作数的值而是直接返回 false。    
逻辑与操作总会返回操作数对应的表达式，也就是说，如果第一个是 true，那么会返回第二个操作数，如果第一个操作数的**求值结果**为 false 则直接返回第一个操作数:
```javascript
const a = !0 && { username: "", password: "" }
console.log(a); // { username: "", password: "" }

const arr = [];
const b = arr[10] && { username: "", password: "" } // undefined
```

- 逻辑或 ===> **||**: 只有两个运算符的布尔值都为 false，结果才是 false，否则是 true(同样适用于任何类型)

同样逻辑或也是短路操作，如果第一个操作数是 true，那么不再往后计算，直接返回第一个操作数

可以使用逻辑或来避免一些赋值给成了 undefined 的问题：
```javascript
// 假设用户数据是从后端获取的，存在 userData 里面
const user = userData || {}
// 如果 userData 是 undefined(后端获取数据失败或者对象 . 运算符没有对应的属性)，那么就会将一个空对象 {} 赋值给 user
```

<hr />

**相等操作符**

- 相等和不相等( **==** && **!=** ): 两个操作符都会先转换操作数（通常称为强制转型），然后再比较它们的相等性
    - 一个操作数是布尔值，则先将其转换为数值(false 转换为 0， true 转换为 1)
    - 一个操作数是字符串，一个是数值，则将字符串转为数值再比较
    - 一个操作数是对象，另一个不是，则调用对象的 `valueOf()` 方法，将其返回值与另一个操作数比较
    - 两个操作数都是对象，则比较他们是不是同一个对象，如果指向同一个对象，返回 true
    - null 和 undefined 是相等的 ( null == undefined )
    - 任何值和 NaN 比较返回的都是 false，包括它自己，任何值和 NaN 比较不等( != )返回的都是 true， 包括 NaN 自己
    - 要比较相等性之前，不能将 null 和 undefined 转换成其他任何值
        ```javascript
        console.log(null == 0); // false
        console.log(null == undefined); // true
        console.log(undefined == 0); // false
        ```
- 全等和不全等( **===** && **!==** ): **比较的时候不转换操作数**，也就是说，如果两个操作数的类型不一样，也会返回 false

<hr />

**逗号操作符**    
逗号操作符除了可以在一条语句中执行多个操作以外，还可以用于赋值：
```javascript
const num = (5, 1, 0, 6); // num 的值为 6 
```
在用于赋值时，逗号操作符总会返回表达式中的最后一项。

### 语句

**for-in语句**

作用：枚举对象的属性
```javascript
const propArr = [];
for (let propname in window){
  propArr.push(propname);
}
console.log(propArr);
```
**注意：如果被循环的对象是 undefined 或者 null， 就不会执行循环体，所以在用 for-in 循环的时候，最好先判断一下对象是不是 null 或者 undefined。**

<hr />

**switch-case语句**

ECMAScript 中的 switch 语句中可以使用任何类型，而不是像 C 语言一样只能使用数字：
```javascript
// 假设是 redux 环境
switch(action.type) {
  case  "CONSTANT_NUM":
    // do something
    break
  default: 
    // default things...
}
```
**switch里面的case在比较值的时候使用的是全等(===)，所以不会发生类型转换。**

<hr />

### 函数

函数内部有一个 arguments 对象会接受所有传进来的参数，可以通过下标访问到传入的参数，也可以通过 length 属性查看传入的参数的数量，但 arguments 不是一个数组的实例，它没有数组原型上的方法，只是一个类数组的对象。    
arguments对象中的值会自动反映到对应的命名参数，所以修改 arguments 中对应的项，就会返回到对应的参数上，始终保持二者的值是一致的:
```javascript
// 一旦执行了 arguments[1] = 10;  以后，num2 的值也会变为 10 
function doAdd(num1, num2) { 
 arguments[1] = 10; 
 alert(arguments[0] + num2); 
}
```