1. JS 模块包装格式都用过哪些，CommonJS、AMD、CMD。定义一个JS 模块代码，最精简的格式是怎样。

    **CommonJS**(nodejs,最早)：这种写法适合服务端，因为在服务器读取模块都是在本地磁盘，加载速度很快。
    ```js
    var modA = require('modA');
    modA.start();
    ```
    **AMD**:这种规范是异步的加载的模块。先定义所有依赖，然后在加载模块的回调函数中执行。
    ```js
    require([module],callback);
    //用AMD写一个模块
    require（['modA'],function(modA){
        modA.start();
    });
    ```
    CMD是seajs推崇的规范。用的时候再require。
    ```js
    define(function(require,exports,module){
        var modA = require('modA');
        modA.start();
    });
    ```
    AMD和CMD最大区别是依赖模块的执行时机不同，而不是加载的实机或者方式不同，二者皆为异步加载模块。

2. JS 怎么实现一个类。怎么实例化这个类。

```js
class Point {
  // ...
}
```
类的数据类型就是函数，类本身就指向构造函数。

使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致。
```js
var b = new Point();
```

3. 理解闭包吗?请讲一讲闭包在实际开发中的作用;闭包建议频繁使用吗?

    >闭包是指有权访问另一个函数作用域中变量的函数。

    >闭包所保存的是整个变量对象，而不是某个特殊的变量。
       读取函数内部变量
        让变量值始终保持在内存里
 ```js
    function f1(){
　　　　var n=999;
　　　　nAdd=function(){n+=1}
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　nAdd();
　　result(); // 1000   
        
```
**需要注意的：**

>由于闭包内的部分资源无法自动释放，容易造成内存泄露 解决方法是，在退出函数之前，将不使用的局部变量全部删除。

>闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

用于闭包会携带包含它的函数的作用域，应词汇比其他函数占用更多的内存。过渡使用闭包可能会导致内存占用过多，建议只在绝对必要时在考虑使用闭包。

4. 说一下了解的js 设计模式，解释一下单例、工厂、观察者。

**工厂模式**
```js
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}

var person1 = createPerson("Nicholas",29,"SoftwareEnginer");
var person2 = createPerson("Greg",27,"Doctor");
```
可以无数次的调用这个函数，而每次他都返回一个包含三个属性一个方法的对象。工厂模式虽然解决了多个相似对象的问题，但却没有解决对象是别的问题。
**构造函数模式**
```js
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(thia.name);
    };
}
var person1 = createPerson("Nicholas",29,"SoftwareEnginer");
var person2 = createPerson("Greg",27,"Doctor");
```
以这种方式定义的构造函数是定义在Global对象。

- 构造函数问题

    使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。
**原型模式**
```js
function Person(){}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Enginer";
Person.prototype.sayName = function(){
    alert(thia.name);
};

var person1 = new Person();
person1.sayName();

var  person2 = new Person();
person2.sayName()
alert(person1.sayName == person2.sayName);  //true

person.name = "Grep";
alert(person1.name);   //"Grep" 来自实例
alert(person2.name);   //"Nicholas" 来自原型
```
>当为对象实例添加一个属性，这个属性就会屏蔽原型对象中保存的同名属性。

hasOwnProperty()方法可以检测一个属性是存在于实例中，还是存在于原型中。
```js
function Person(){}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Enginer";
Person.prototype.job = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();
alert(person1.hasOwnProperty("name")); //false（来自原型）
alert("name" in person1);   //true   ---来自实例
alert("name" in person2);   //true   ---来自原型
person1.name = "Grep";
alert(person1.hasOwnProperty("name"));   //true(来自实例)
//同时使用
```
- 原型与in操作符hasOwnProperty()方法和in操作符，就可以确定该属性到底存在与对象中还是存在于原型中。
```js
function hasOwnPropertyProperty(object,name){
    return !object.hasOwnProperty(name) && (name in object);
}
```

**组合使用构造函数模式和原型模式**
```js
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby","Court"];
}
Person.prototype = {
    constructor:Person,
    sayName:function(){
        alert(this.name);
    }
}
var person1 = createPerson("Nicholas",29,"SoftwareEnginer");
var person2 = createPerson("Greg",27,"Doctor");

person1.friends.push("Van");
person1.friends   //"Shelby","Court","Van"
person2.friends   //"Shelby","Court"
```
**动态原型模式**
```js
function Person(name,age,job){
    //属性
    this.name = name;
    this.age = age;
    this.job = job;
    //方法
    if (typeof this.sayName != "function"){
        Person.prototype.sayName = function(){
            alert(this.name);
        };
    }
}
var friend = new Person("Nicholas",29,"SoftwareEnginer");
friend.sayName();
```
>在使用动态模型时，不能使用对象字面量重写原型。如果在已经创建了实力的情况下重写原型，那么就会切断现有实力与新原型之间的联系。

**寄生构造函数模式**
```js
function SpecialArray(){
    //创建数组
    var values = new Array();
    //添加值
    values.push.apply(values,arguments);
    //添加方法
    values.toPipedString  = function(){
        return this.join("|");
    };
    //返回数组
    return values;
}
var colors = new SpecialArray("red","blue","green");
colors.toPipedString();   //"red|blue|green"
```
返回的对象与构造函数或者与构造函数的原型属性之间没有关系；
**稳妥构造函数模式**
```js
function Person(name,age,job){
    //创建要返回的对象
    var o = new Object();
    //可以在这里定义私有变量和函数
    //添加方法
    o.sayName = function(){
        alert(name);
    };
    //返回对象
    return o;
}
var friend = new Person("Nicholas",29,"SoftwareEnginer");
friend.sayName();
```

新创建对象的实例方法不引用this;二是不使用new操作符调用构造函数。

1. ajax 跨域有哪些方法，jsonp 的原理是什么，如果页面编码和被请求的资源编码不一致如何处理?

**CORS**

>CORS背后的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

服务器端对于CORS的支持，主要就是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。

**图像Ping**

我们也可以动态的创建图像，使用它们的onload和onerror事件处理成西来确定是否接收到了响应。
动态创建图像经常用于图像Ping。
```js
var img = new Image();
img.onload = img.onerror = function () {
    console.log('Done');
};
img.src = 'http://www.example.com/test?name=Nico';
```
图像Ping最常用于跟踪用户点击页面或动态广告曝光次数。
>图像Ping有两个主要的缺点：
    只能发送GET请求。
    无法访问服务器的响应文本。
**图像Ping只能用于浏览器与服务器间的单向通信。**

**JSONP**

JSONP由两部分组成：回调函数和数据。
JSONP是通过动script元素来使用的，使用时可以为src属性指定一个跨域URL。

**CORS和JSONP对比**

    JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
    使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
    SONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS。
