1. CSS 盒子模型，**绝对定位和相对定位**
```text 
当对一个文档进行布局（lay out）的时候，浏览器的渲染引擎会根据标准之一的 CSS 基础框盒模型（CSS basic box model），将所有元素表示为一个个矩形的盒子（box）。
```
- 相对定位：定位是相对于自身位置定位（设置偏移量的时候，会相对于自身所在的位置偏移）。设置了relative的元素仍然处在文档流中，元素的宽高不变，设置偏移量也不会影响其他元素的位置。最外层容器设置为relative定位，在没有设置宽度的情况下，宽度是整个浏览器的宽度。
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      .box1 {
        height: 100px;
        background-color:red;
      }
      .box2{
        height: 100px;
        position: relative;
        background-color:rgb(23, 207, 152);
        left: 50px;
        top: 50px;
      }
      .box3{
        height: 100px;
        background-color:rgb(52, 40, 155);
      }
      .box4{
        height: 100px;
        background-color:rgb(195, 209, 117);
      }
      .box4{
        height: 100px;
        background-color:rgb(235, 124, 80);
      }
    </style>
  </head>
  <body>
    <div class="box1">box1box1box1box1box1box1box1box1box1</div>
    <div class="box2">box1box1box1box1box1box1box1box1box1</div>
    <div class="box3">box1box1box1box1box1box1box1box1box1</div>
    <div class="box4">box1box1box1box1box1box1box1box1box1</div>
    <div class="box5">box1box1box1box1box1box1box1box1box1</div>
  </body>
</html>
```
- absolute：定位是相对于离元素最近的设置了绝对或相对定位的父元素决定的，如果没有父元素设置绝对或相对定位，则元素相对于根元素即html元素定位。设置了absolute的元素脱了了文档流，元素在没有设置宽度的情况下，宽度由元素里面的内容决定。脱离后原来的位置相当于是空的，下面的元素会来占据位置。

> - 绝对定位：absolute 和 fixed 统称为绝对定位 
> - 相对定位：relative
> - 默认值：static
2. 清除浮动，什么时候需要清除浮动，清除浮动都有哪些方法?
    当对元素进行了float时，我们的元素就会脱离文档流，像一只小船一样漂流在文档之上。（任何元素都可以浮动。浮动元素会生成一个块级框，不论他本身是何种元素。）
 - 当内层元素浮动后，就出现影响：
  >父盒子的margin受到影响，无法实现左右居中。
  >没有给父盒子设置高度，浮动后父盒子的高度没有被撑开。
##### 清除浮动的方法
  - 新添加的元素应用clear:both;
  ```html
  <div class="outer">
    <div class="div1">1</div>
    <div class="div2">2</div>
    <div class="div3">3</div>
    <div class="clear"></div>
</div> 
```
```css
.clear{clear:both; height: 0; line-height: 0; font-size: 0}
```
>使用空标签清除浮动
```html
<div class="outer">
    <div class="div1">1</div>
    <div class="div2">2</div>
    <div class="div3">3</div>
    <div style="clear:both"></div>
</div> 
```
  - 父级div定义overflow:auto(注意：是父级div也就是这里的div.outer)
```html
<div class="outer">
                <div class="div1">1</div>
                <div class="div2">2</div>
                <div class="div3">3</div>
            </div>
```
```css
 .outer{overflow: auto; zoom: 1;} //zoom:1;是为了兼容IE6
```
>**原理**： 使用overflow属性来清除浮动有一点需要注意，overflow属性共有三个属性值：hidden,auto,visible。我们可以使用hiddent和auto值来清除浮动，但切记不能使用visible值，如果使用这个值将无法达到清除浮动效果，其他两个值都可以。
- :after方法
>**原理**： 利用:after和:before来在元素内部插入两个元素块，从而达到清除浮动的效果。
  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
      <style>
        .outer {
          border: 1px solid #ccc;
          background: #fc9;
          color: #fff;
          margin: 50px auto;
          padding: 50px;
        }

        .clearfix:after {
          content: "";
          display: block;
          clear: both;
          visibility: hidden;
          zoom: 1;
        }
        .div1 {
          width: 80px;
          height: 80px;
          background: red;
          float: left;
        }
        .div2 {
          width: 80px;
          height: 80px;
          background: blue;
          float: left;
        }
        .div3 {
          width: 80px;
          height: 80px;
          background: sienna;
          float: left;
        }
      </style>
    </head>
    <body>
      <div class="outer clearfix">
        <div class="div1">1</div>
        <div class="div2">2</div>
        <div class="div3">3</div>
      </div>
    </body>
  </html>
  ```
**补充**
>使用after伪对象清除浮动

>after伪对象非IE浏览器支持，所以并不影响到IE/WIN浏览器。具体写法可参照以下示例。使用中需注意以下几点。

>a、该方法中必须为需要清除浮动元素的伪对象中设置height:0，否则该元素会比实际高出若干像素；

>b、content属性是必须的，但其值可以为空，蓝色理想讨论该方法的时候content属性的值设为”.”

3. 如何保持浮层水平垂直居中?
 - 利用绝对定位和transform
    >Transform属性应用于元素的2D或3D转换。这个属性允许你将元素旋转，缩放，移动，倾斜等。
    ```html
    <div class="parent">
        <div class="children">

        </div>
    </div>
    ```
    ```css
     .children{
        position: absolute;
        top: 50%;
        left: 50%;
        -webkit-transform: translate(-50%,-50%);
        background: black;
    }
    ```
 - 利用flexbox
    ```html
        <div class="parent">
          <div class="children">
            被vbbmbhjjghg
          </div>
        </div>
      ```
      ```css
      .parent {
            display: -webkit-flex;
            justify-content: center;
            align-items: center;
          }
          .children {
            width: 100px;
            background: black;
          }
      ```
 - 当子元素的宽高固定，父元素内含有除居中元素外其它元素（空标签也行）或者父元素的高度不为0时。
      ```css
      .parent {
            height: 400px; 
            position: relative;
            background: red;
          }
          .children {
            width: 200px;
            height: 200px;
            margin: -100px 0 0 -100px;
            background: black;
            position: absolute;
            top: 50%;
            left: 50%;
          }
      ```
      ```html
      <div class="parent">
          <div class="children">
            被vbbmbhjjghg
          </div>
          <span></span>
        </div>
      ```
 - display:table-cell
    CSS中有一个用于竖直居中的属性vertical-align，但只有 当父元素为td或者th时，这个vertical-align属性才会生效，对于其他块级元素，例如 div、p等，默认情况下是不支持vertical-align属性的，可以设置块级元素的display类型为table-cell，激活vertical-align属性，但display:table-cell存在兼容性问题，所以这种方法没办法跨浏览器兼容。
    ```css
    .parent{
    width: 400px;
    height: 100px;
    background: black;
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    }

    .child{
        backgroung: red;
        display: inline-block
    }
    ```
    ```html
      <div class="parent">
    　　  <div class="child">哈哈</div>
      </div>
    ```
 - 利用定位与margin: auto;
    ```css
    .parent {
        width: 600px;
        height: 400px;
        background: red;
        position: relative;
      }
      .child {
        width: 200px;
        height: 200px;
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        margin: auto;
        background: black;
      }
    ```
    ```html
      <div class="parent">
        <div class="child">哈哈</div>
      </div>
    ```
  
    >**原因**： 原理：因为 parent 宽度等于 child宽度 + left + right + marginLeft + marginRight，当设置了left:0;right:0;margin: auto;时候，
  就相当于左右平分了宽度，所以会水平居中，垂直方向也是一样的道理
 -  position 和 display 的取值和各自的意思和用法
    >**position**: 1.static: 没有定位，正常状态下。可以快速取消定位，让top、right、bottom、left 失效。
    2.relative: 相对于其在正常流中的位置偏移，原本占据的空间依然会保留。
    3.absolute: 相对于第一个position属性不为static的父类定位，会脱离正常文档流，不占据空间位置。
    4.fixed: 定位原点相对于浏览器窗口，而且不能变。
    5.inherit: 从父类继承position属性的值，但是任何版本的IE都不支持该属性。
    6.sticky: 该元素并不脱离文档流，仍然保留元素原本在文档流中的位置，这个属性的兼容性还不是很好，目前仍是一个试验性的属性，并不是W3C推荐的标准。

    >z-index属性是针对以上定位属性而出现的，它只在定位元素上有效。

    >**display**:1. display:none和visiability: hidden 都可以隐藏div，前者不占据文档的空间，后者占据文档的位置。2. inline: 行内元素，以水平方式布局，垂直方向的margin和padding都是无效的，大小和内容一样，且无法设置宽高。
    3.block: 块元素，独占一行，可以使用margin来控制元素之间间距
    4.inline-block: 既具有block的宽度高度特性又具有inline的同行特性。
 - 样式的层级关系，选择器优先级，样式冲突，以及抽离样式模块怎么写，说出思路，有无实践经验
    ```text
    ID 选择器， 如 #id{}
    类选择器， 如 .class{}
    属性选择器， 如 a[href="segmentfault.com"]{}
    伪类选择器， 如 :hover{}
    伪元素选择器， 如 ::before{}
    标签选择器， 如 span{}
    通配选择器， 如 *{}
    ```
    **CSS 的继承性**
      - CSS 优先规则1： 最近的祖先样式比其他祖先样式优先级高。
      - CSS 优先规则2："直接样式"比"祖先样式"优先级高。
    **选择器的优先级**
      - CSS 优先规则3：优先级关系：内联样式 > ID 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签选择器 = 伪元素选择器
      - CSS 优先规则4：计算选择符中 ID 选择器的个数（a），计算选择符中类选择器、属性选择器以及伪类选择器的个数之和（b），计算选择符中标签选择器和伪元素选择器的个数之和（c）。按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个的选择符中 a、b、c 都相等，则按照"就近原则"来判断。
      - CSS 优先规则5：属性后插有 !important 的属性拥有最高优先级。若同时插有 !important，则再利用规则 3、4 判断优先级。
      > ID 选择器权值为 100，类选择器权值为 10，标签选择器权值为 1，当一个选择器由多个 ID 选择器、类选择器或标签选择器组成时，则将所有权值相加，然后再比较权值。这种说法其实是有问题的。比如一个由 11 个类选择器组成的选择器和一个由 1 个 ID 选择器组成的选择器指向同一个标签，按理说 110 > 100，应该应用前者的样式，然而事实是应用后者的样式。错误的原因是：选择器的权值不能进位。还是拿刚刚的例子说明。11 个类选择器组成的选择器的总权值为 110，但因为 11 个均为类选择器，所以其实总权值最多不能超过 100， 你可以理解为 99.99，所以最终应用后者样式。
 - css3动画效果属性，canvas、svg的区别，CSS3中新增伪类举例
    - transition 过渡
    >transition需要事件触发，不可以在网页加载时自动发生
     transition是一次性的，不能重复发生，除非是一再触发
    - animation 动画
    - keyframes
    - transform 变形

     **canvas、svg的区别**


     **CSS3中新增伪类举例**

        -  p:first-of-type 选择属于其父元素的首个元素的每个 元素。
        - p:last-of-type  选择属于其父元素的最后元素的每个元素。
        - p:only-of-type  选择属于其父元素唯一的元素的每个元素。
        - p:only-child    选择属于其父元素的唯一子元素的每个 元素。
        - p:nth-child(2)  选择属于其父元素的第二个子元素的每个元素。
        - :enabled、:disabled 控制表单控件的禁用状态。
        - :checked，单选框或复选框被选中。
- px和em和rem的区别，CSS中link 和@import的区别是?
    >px是像素，设置字体大小时，比较稳定和精确。但是如果改变了浏览器的缩放，页面布局会被打破。因此，这时就提出了使用'em'来定义Web页面的字体。
    em是根据基准来缩放字体的大小。em实质是一个相对值，而非具体的数值。这种技术需要一个参考点，一般都是以自己的或父级的'font-size'为基准。
    rem：em是相对于其父元素来设置字体大小的，这样就会存在一个问题，进行任何元素设置，都有可能需要知道它父元素的大小。而rem是相对于根元素<html>，这样就意味着我们只需要在根元素确定一个参考值。

    **CSS中link 和@import的区别是?**
    - 1、 link 除了可以加载CSS外， 还可以定义RSS, 定义rel 连接属性等其他作用；@import只能加载CSS。

    - 2. 加载顺序：link 引用的CSS会在页面被加载的时候同时加载；@import 引用的CSS会等到页面全部被下载完再被加载。

    - 3. 兼容性的差别。 @import 是CSS2.1 提出的，老的浏览器不支持，IE5 以上的才能识别（不过现在来说，已经不是问题了，应该很少有使用IE5及以下的浏览器了）。 link 浏览器都支持。

    - 4. 使用javascript 可以控制到 link, 但@import 却无法控制。

-  了解过flex吗?


  