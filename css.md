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


