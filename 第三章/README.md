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

JS 中的 Number 类型
<hr />

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
        - 参数是 undefined，返回 NaN
        - 参数是字符串
            - 空字符串：返回 0
            - 只含有数字的字符串(包括十进制、十六进制和八进制、整数，浮点数)： 八进制忽略前导 0 返回剩下的，十进制原封不动的返回，十六进制返回对应的十进制数字
            - 含有其他字符的字符串： 返回 NaN
        - 参数是对象
            先调用对象的 `valueOf` 方法，根据 `valueOf` 返回的值按照上面的规则来转，如果转换的结果是 NaN，则调用对象的 `toString()` 方法，然后再次依照前面的规则转换返回的字符串值。  
            
    ```javascript
    console.log(Number("123a")); // NaN
    console.log(Number("10")); // 10
    console.log(Number("00000123")); // 123
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
        - 如果是数字，会向后查找，知道遇到一个不是数字的(包括小数点)，返回期间的数字并忽略后面的内容
        - 十六进制转为其对应的十进制，八进制忽略前面的 0 ，返回对应的十进制(注意八进制的数字范围在 0 - 7 ，如果超出 7 ，那么就会原封不动返回 0 后面的)
            ```javascript
            console.log(parseInt("1234abc")); // 1234 (忽略后面的 abc )
            console.log(parseInt("")); // NaN
            console.log(parseInt("59.3")); // 59
            // 转八进制
            console.log(parseInt("0123")); // 83
            console.log(parseInt("0487")); // 487 ( 8 > 7 )
            console.log(parseInt("00000000123")); // 123
            ```
        parseInt 还支持第二个参数，表示按照多少进制解析该字符串，默认是按照十进制转字符串
        ```javascript
        // 可以省略 0
        console.log(parseInt("0123", 8));  // 83
        console.log(parseInt("123", 8)); // 83
        // 默认按照十进制解析
        console.log(parseInt("123")); // 123
        
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
        
JS 中的 String 类型
<hr />
