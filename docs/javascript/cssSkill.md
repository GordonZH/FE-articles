### 1.移动端字体大小适配
利用媒体查询器设置根元素的字体大小，并在开发时使用`rem`作为单位
```css
@media screen and (min-width: 320px) {
    html {font-size: 14px;}
}
@media screen and (min-width: 360px) {
    html {font-size: 16px;}
}
@media screen and (min-width: 400px) {
    html {font-size: 18px;}
}
@media screen and (min-width: 440px) {
    html {font-size: 20px;}
}
@media screen and (min-width: 480px) {
    html {font-size: 22px;}
}
@media screen and (min-width: 640px) {
    html {font-size: 28px;}
}
```

### 2.sticky footer
footer始终在页面的最下方：内容不足时，footer在页面的底部，内容超过一页时，footer在页面所有内容的下方。
主要实现：三个容器：
1. main-wrap：设置高度`min-height：100%`
2. content：设置高度`height:100%` `padding-bootom:100px `,padding值等于footer高度，相当于给footer **腾出** 一个位置
3.  footer：设置高度`height:100px;` `margin-top:100px;` footer的实际位置是在main-wrap下面。当内容不足一页时，它在页面下方是看不见的，但是使用一个负边距使其进入可视区域。这个负边距正好等于content的padding-bottom，所以不会挡住content中的内容。内容满了一页时，自动content撑大main-wrap，将footer向下顶。

```css
/*css*/
html, body {
    height: 100%;
    margin: 0;
    padding: 0;
}
.main-wrap {
    min-height: 100%;
    width: 100%;
    height: auto!important;
}
.content {
    width: 100%;
    padding-bottom: 100px;/*等于footer高度*/
}
.footer {
    height: 100px;
    margin-top: -100px;/*等于高度*/
    background-color: yellow;
    clear: both;
}
```
```html
<!--结构-->
<div class="main-wrap clearfix">
    <div class="content">
        <!--内容-->
    </div>
</div>
<div class="footer">
    <!--页尾始终在页面最下方-->
</div>
```

### 3.fixed header &footer 固定顶部&底部
在最外层容器中，使用三个部分，`header`、`content`、`footer`，由于都是`body`的直接子元素，可以使用绝对定位，然后设置content的`top` `bottom`值，使其等于`header` `footer`的高度，并同时设置`overflow:scroll`,这样页面就可以正常的滚动。此办法可以解决Safari中唤起键盘时，fixed定位的footer错位的问题。
```css
/*css*/
* {
    margin: 0;
    padding: 0;
}
.main-wrap {
    position: absolute;
    width: 100%;
    top: 50px;/*等于header高度*/
    bottom: 100px;/*等于footer高度*/
    overflow: scroll;
    background-color: #00bbff;
}
.header,.footer{
    position: absolute;
    left: 0;
    right: 0;
    background-color: yellow;
    text-align: center;
    font-size: 3em;
}
.header {
    height: 50px;/*等于main-wrap的top值*/
    top: 0;
}
.footer {
    height: 100px;/*等于main-wrap的bottom值*/
    bottom: 0;
}
```
```html
<!--结构-->
<body>
    <div class="header">
        <p>header</p>
    </div>
    <div class="main-wrap clearfix">
        <div class="content">
            <!--内容-->
        </div>
    </div>
    <div class="footer">
        <p>footer</p>
    </div>
</body>
```
### 4.clearfix 伪元素清除浮动
```css
.clearfix:after { 
    content:"\200B"; /*`\200B`是一个零宽度空格*/
    display:block; 
    height:0; 
    clear:both; 
} 
.clearfix {*zoom:1;}/*IE/7/6*/
```

### 5.ios下页面滑动卡顿
当ios上滚动某个元素发生卡顿时，为需要滚动的容器添加如下代码：
```css
.container {
    -webkit-overflow-scrolling: touch;
}
```

### 6.1px 边框实现
由于各厂商的屏幕物理像素与逻辑像素的不同，1px的边框在某些屏幕上显得很粗。使用媒体查询器，条件为当前屏幕的像素密度`min-device-pixel-ratio`，将其缩小。同时边框使用伪元素实现。
```css
.border-1px{
    position: relative
}
.border-1px:after{
    content: '';
    display: block;
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    border-top: 1px solid black;
}
@media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5){
    .border-1px:after{
        -webkit-transform :scaleY(0.7);
        transform :scaleY(0.7);
    }
}
@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){
    .border-1px:after{
        -webkit-transform :scaleY(0.5);
        transform :scaleY(0.5);
    }
}
```