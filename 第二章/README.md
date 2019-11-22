在 HTML 中使用 JavaScript
------------------------

### script 元素
- script 标签的 4 个属性
    1. async: 立刻异步下载外部脚本文件，不影响页面中的其他操作
    2. defer: 脚本文件可以等到整个 HTML 文档被解析和显示完了再执行
    3. src: 外部脚本文件地址
    4. type: 脚本语言的内容类型，一般就是 text/javascript
注意，在解释器对 script 中的代码或者外部脚本文件中的代码进行解析的时候，页面中的其他内容不会被浏览器加载或显示，这也是为什么最好把 script 标签放在 body 的最后面，这样可以减少白屏时间。    
```html
<!DOCTYPE html> 
<html> 
    <head> 
    <!-- head info-->
    </head> 
    <body> 
    <!-- 这里放内容 --> 
    <script type="text/javascript" src="example2.js"></script> 
    </body> 
</html> 
```
浏览器会按照 script 标签在页面中出现的先后顺序来依次对它们进行解析(从上至下的顺序)。   

在 script 标签中添加 defer 属性会让脚本在被解释器遇到的时候立即下载但是不会执行，而是直到解释器遇到 `</html>` 标签以后才开始执行(立即下载，延迟执行)，而且，标记为 defer 的延迟脚本
执行的时候会按照在 html 文档中的顺序来执行。    

### 嵌入代码与外部文件
- 使用外部的脚本文件相比于内嵌于 html 中的 js 代码，有以下优点：
    1. 可维护性: js 文件统一放到一个文件夹下，更方便进行管理，尤其是涉及到跨页面的操作。
    2. 可缓存: 浏览器能够根据具体的设置缓存链接的所有外部 JavaScript 文件。也就是说，如果有两个页面都使用同一个文件，那么这个文件只需下载一次。因此，最终结果就是能够加快页面加载的速度。
    3. 适应未来
    
### 文档模型 (DOCTYPE)
文档模式分为**标准模式**和**混杂模式**。两种模式主要影响 css 样式的呈现，某些情况下也会影响到 JavaScript 的解释执行。      
对于标准模式，可以通过使用下面任何一种文档类型来开启:   
```html
<!-- HTML 4.01 严格型 --> 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" 
"http://www.w3.org/TR/html4/strict.dtd">

<!-- HTML 5 --> 
<!DOCTYPE html>
```
### noscript 脚本
在浏览器不支持或者 JavaScript 被禁用的情况下，会解释执行 noscript 之间的代码，noscript 元素之间可以包含除 script 以外的任何 html 标签。
**注意：只有浏览器不支持 js 或者 js 被禁用，才会显示 noscript 里面的内容**

