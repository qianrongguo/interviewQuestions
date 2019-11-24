1. JavaScript 里有哪些数据类型，解释清楚 null 和 undefined，解释清楚原始数据类型和引用数据类型。比如讲一下 1 和 Number(1)的区别
   
    undefined类型、Null类型、Boolean类型、Number类型、String类型、Object类型。

    null值表示一个空对象指针。（Null类型，代表“空值”，代表一个空对象指针，使用typeof运算得到 “object”，所以你可以认为它是一个特殊的对象值）。
    
    未初始化对象会自动赋予undefined。（Undefined类型，当一个声明了一个变量未初始化时，得到的就是undefined）

    >基本数据类型：string（字符串） number（数值类型） boolean（布尔类型） null（空） undefined（未定义）
引用数据类型：Date（日期） Array（数组） Object（对象） Function（函数） RegExp（正则表达式）


        基本数据类型数据存储发生在栈内存中；
        引用类型数据存储，分两步，在堆中保存数据，在栈中保存数据的地址(堆地址)；
        引用类型赋值，传递的是地址；基本类型赋值，传递的是值。

1. 讲一下 prototype 是什么东西，原型链的理解，什么时候用 prototype

        在JavaScript中，prototype对象是实现面向对象的一个重要机制。
        　　每个函数就是一个对象（Function），函数对象都有一个子对象 prototype对象，类是以函数的形式来定义的。prototype表示该函数的原型，也表示一个类的成员的集合。
        
        要弄清楚原型链就要先弄清楚 function 类型，在JavaScript中没有类的概念，都是函数，所以它是一门函数式的编程语言。类有一个很重要的特性，就是它可以根据它的构造函数来创建以它为模板的对象。在javascript中，函数就有2个功能
        第一、 作为一般函数调用
        第二、 作为它原型对象的构造函数 也就new（）
        
        凡是通过 new Function() 创建的对象都是函数对象，其他的都是普通对象
        JS在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做__proto__的内置属性，用于指向创建它的函数对象的原型对象prototype。
        1.原型和原型链是JS实现继承的一种模型。
        2.原型链的形成是真正是靠__proto__ 而非prototype
        将方法定义到构造方法的prototype上，这样的好处是，通过该构造函数生成的实例所拥有的方法都是指向一个函数的索引，这样可以节省内存

2. 函数里的this什么含义，什么情况下，怎么用。

        this是Javascript语言的一个关键字。它代表函数运行时，自动生成的一个内部对象，只能在函数内部使用。随着函数使用场合的不同，this的值会发生变化。但是有一个总的原则，那就是this指的是，调用函数的那个对象。

         情况一：纯粹的函数调用(这是函数的最通常用法，属于全局性调用，因此this就代表全局对象Global。)

        情况二：作为对象方法的调用(函数还可以作为某个对象的方法调用，这时this就指这个上级对象。)

        情况三： 作为构造函数调用(所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。)

        情况四： apply调用(apply()是函数对象的一个方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数的对象。因此，this指的就是这第一个参数。)

3. apply和 call 什么含义，什么区别?什么时候用。(我有篇文章 重点分析过)

        call和apply都用于函数调用，和使用函数名直接调用不同，call和apply可以指定一个额外的参数作为函数体内的this对象。
 
        call采用不定长的参数列表，而apply使用一个参数数组。
 
        call和apply都用于函数调用，和使用函数名直接调用不同，call和apply可以指定一个额外的参数作为函数体内的this对象。
 
        call采用不定长的参数列表，而apply使用一个参数数组。
 
　　    由于call和apply可以改变函数体内的this指向，因此通常被用来将一个对象原型上的方法应用到另一个对象上。一个常见的应用是处理函数的arguments，将其转换为Array类型

4. 数组对象有哪些原生方法，列举一下，分别是什么含义，比如连接两个数组用哪个方法，删除数组的指定项和重新组装数组(操作数据的重点)。

    - 转换方法

    >toLocaleString(),toString(),valueOf()方法。

    - 栈方法(后进先出)

    >push(),pop()。

    - 队列方法(先进先出)

    >shift(),unshift()

    - 重排序方法

    >reserse()和sort()

    - 操作方法

    >删除:splice(0,2)  插入splice(2,0,"red","green"),替换splice(2,1,"red","green")

    - 迭代方法

    >every();
    filter();
    forEach();
    map();
    some();

    - 缩小方法

    >reduce()和reduceRight()


5. 怎样避免全局变量污染?ES5严格模式的作用，ES6箭头函数和ES5普通函数一样吗?

    1,定义全局变量做容器
    ```js
      var contain = {}; var contain.fn = function() { // .... }
    ```

    2.使用自执行函数
    ```js
    (function() { // ... })()
    例子1:
    (function(){
    var tmp= {};
    var name = 'jack';
    tmp.method = function(){
        return name;
    }
    window.tmp= tmp;
    })()
    console.log(tmp.method());

    例子2:
    (function(obj){
        var count = {};
        var interim;

        count.name = 'jack';
        count.method = function(){
            console.log('内部方法');
        } 

        //把count对象挂到obj下
        obj.count = count
    })(window)
 ```
