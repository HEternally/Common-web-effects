# Common-web-effects
前端开发中常用的网页特效


## 1、前言 ##

很久没有发布文章啦，终于今天有点时间，就在下班前把我之前所写的为了学习JS基础编写的一些使用JS实现的网页常用特效分享给大家；相关的代码我之后也会放到[GitHub][1]上，欢迎大家star；

## 2、JS实现特效的概念 ##

JS实现网页特效，其实通俗的说就是通过JS动态地控制css样式，以达到实现动画的效果；
（很多动画特效都可以由css3实现，个人推荐是能用css3就用css3；这里用Js的目的我前面也说了，是为了学习JS基础以及学习如何通过js控制各种节点）。

## 3、案例1--选项卡 ##

![选项卡](https://segmentfault.com/img/bVUAgE)

### 3.1、案例分析 ###

 1. 首先，实现了最基本的功能：点击不同的按钮切换显示不同的内容；
 2. 其次，加了定时器，用的是setInterval，当然用setTimeout也是可以的；
 3. 最后，给body绑定了onmouseover和onmouseout事件,鼠标移入清除定时器，移出又添加定时器。

### 3.2案例实现过程 ###

#### 3.2.1编写网页的内容和样式 ####

 1. HTML

```
<!DOCTYPE html>
<html lang="en">
<head>
<title>点击切换选项卡代码</title>
</head>
<body>
<div class="tab1" id="tab1">
    <div class="menu">
        <ul>
            <li id="one1" onclick="setTab('one',1)">首页</li>
            <li id="one2" onclick="setTab('one',2)">点击看看</li>
            <li id="one3" onclick="setTab('one',3)">会自动的</li>
            <li id="one4" onclick="setTab('one',4)">我的网站</li>
        </ul>
    </div>
    <div class="menudiv">
        <div id="con_one_1">我的网站</div>
        <div id="con_one_2" style="display:none;">JS代码,导航菜单</div>
        <div id="con_one_3" style="display:none;">看到效果了吗？？？</div>
        <div id="con_one_4" style="display:none;">我的网站我做主</div>
    </div>
</div>
<div style="text-align:center;clear:both;"></div>
</body>
</html>
```

 2. CSS

```
* {
    margin: 0;
    padding: 0;
    list-style-type: none;
}
a, img {
    border: 0;
}
body {
    font: 12px/180% Arial, Helvetica, sans-serif, "新宋体";
}
.tab1 {
    width: 401px;
    border-top: #cccccc solid 1px;
    border-bottom: #cccccc solid 1px;
    margin: 50px auto 0 auto;
}
.menu {
    height: 28px;
    border-right: #cccccc solid 1px;
}
.menu li {
    float: left;
    width: 99px;
    text-align: center;
    line-height: 28px;
    height: 28px;
    cursor: pointer;
    border-left: #cccccc solid 1px;
    color: #666;
    font-size: 14px;
    overflow: hidden;
    background: #E0E2EB;
}
.menu li.off {
    background: #FFFFFF;
    color: #336699;
    font-weight: bold;
}
.menudiv {
    height: 200px;
    border-left: #ccc solid 1px;
    border-right: #ccc solid 1px;
    border-top: 0;
    background: #fefefe
}
.menudiv div {
    padding: 15px;
    line-height: 28px;
}
```

> 因为都是很基础的，我就主要讲解一下JS部分，后面的案例也一样，请理解。

#### 3.2.2实现特效 ####

```
var name_0='one';
var cursel_0=1;
var ScrollTime=3000;//循环周期（毫秒）
var links_len,iIntervalId;
window.onload=function(){
    var links = document.getElementById("tab1").getElementsByTagName('li')
    links_len=links.length;
    for(var i=0; i<links_len; i++){
        links[i].onmouseover=function(){
            clearInterval(iIntervalId);
            this.onmouseout=function(){
                iIntervalId = setInterval(Next,ScrollTime);;
            }
        }
    }
    document.getElementById("con_"+name_0+"_"+links_len).parentNode.onmouseover=function(){
        clearInterval(iIntervalId);
        this.onmouseout=function(){
            iIntervalId = setInterval(Next,ScrollTime);;
        }
    }
    setTab(name_0,cursel_0);
    iIntervalId = setInterval(Next,ScrollTime);
}
function setTab(name,cursel){
    cursel_0=cursel;
    for(var i=1; i<=links_len; i++){
        var menu = document.getElementById(name+i);
        var menudiv = document.getElementById("con_"+name+"_"+i);
        if(i==cursel){
            menu.className="off";
            menudiv.style.display="block";
        }
        else{
            menu.className="";
            menudiv.style.display="none";
        }
    }
}
function Next(){
    cursel_0++;
    if (cursel_0>links_len)cursel_0=1
    setTab(name_0,cursel_0);
}
```

> **注意**：onload 事件会在页面或图像加载完成后立即发生，类似与jQuery中的ready事件，推荐都加上，或者也可以使用立即执行函数，这就看个人喜欢；变量最好是在最开始的时候就声明，因为使用var声明的变量会发生“声明提前”，即使你在变量声明之前调用也不会报错；但是在es6中的let就很好的纠正了这一现象，所以我建议大家要养成一个好的习惯，先声明，在调用；


## 4、案例2--图片弹窗 ##



![图片弹窗](https://segmentfault.com/img/bVUAH0)



### 4.1、案例分析 ###

 1. 一个图片弹窗特效，点击图片会显示大图，并且可以读取图片的alt信息显示出来；点击关闭按钮会关闭弹窗。

### 4.2、案例实现过程 ###

#### 4.2.1编写网页的内容和样式 ####

 1. HTML

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>javascript图片弹窗</title>
    </head>
<body>
<!-- 触发弹窗，图片改为你的图片地址 -->
    <img id="myImg" src="images/img.jpg" alt="文本描述信息" width="300" height="200">
<!-- 弹窗 -->
<div class="modal" id="myModal">
    <!-- 关闭按钮 -->
    <span class="close" onclick="document.getElementById('myModal').style.display='none'">&times;</span>
    <!-- 弹窗内容 -->
    <img class="modal-content" id="img01">
        <!-- 文本描述 -->
        <div id="caption"></div>
</div>
</body>
</html>
```

 2. CSS

```
/* 触发弹窗图片的样式*/
    #myImg{
        border-radius: 5px;
        cursor:pointer;
        transition:0.3s;
    }
    #myImg:hover{opacity:0.7;}
    /* 弹窗背景*/
    .modal{
        display:none;   /*Hidden by default*/
        position: fixed;/* Sit in place*/
        z-index: 1;/* Sit on top*/
        padding-top:100px;/* Location of the box*/
        left:0;
        top: 0;
        width:100%;/* Full width*/
        height: 100%;/*  Full width*/
        overflow: auto;
        background-color: rgba(0,0,0,0.9);/*  Black w/ opactity*/
    }
    /* 图片*/
    .modal-content{
        margin: auto;
        display: block;
        width:80%;
        max-width: 700px;
    }
    /* 文本内容*/
    #caption{
        margin: auto;
        display: block;
        width:80%;
        max-width: 700px;
        text-align:center;
        color:#ccc;
        padding:10px 0;
        height: 150px;
    }
    /* 添加动画*/
    .modal-content, #caption { 
    -webkit-animation-name: zoom;           /*定义一个或多个动画名称*/
    -webkit-animation-duration: 0.6s;          /*指定对象动画的持续时间*/
    animation-name: zoom;
    animation-duration: 0.6s;
}
 
@-webkit-keyframes zoom {                     
    from {-webkit-transform:scale(0)}  /*transform属性向元素应用2D或3D转换，scale是元素缩放*/
    to {-webkit-transform:scale(1)}
}
 
@keyframes zoom {
    from {transform:scale(0)} 
    to {transform:scale(1)}
}
 
/* 关闭按钮 */
.close {
    position: absolute;
    top: 15px;
    right: 35px;
    color: #f1f1f1;
    font-size: 40px;
    font-weight: bold;
    transition: 0.3s;
}
 
.close:hover,.close:focus {
    color: #bbb;
    text-decoration: none;
    cursor: pointer;
}
 
/* 小屏幕中图片宽度为 100% */
@media only screen and (max-width: 700px){
    .modal-content {
        width: 100%;
    }
}
```

> 简单讲解一下CSS代码，部分注解我写在了注释中；那在这里就重点说一下css3中的动画，可以使用@keyframes规则创建动画。创建动画是通过逐步改变从一个CSS样式设定到另一个。在动画过程中，您可以更改CSS样式的设定多次。指定的变化时发生时使用％，或关键字"from"和"to"，这是和0％到100％相同。0％是开头动画，100％是当动画完成；然后使用animation添加动画。记得要注意兼容性，在webkit内核的浏览器中要加前缀。

 3. JS

```
 // 获取弹窗
var modal = document.getElementById('myModal');
 
// 获取图片插入到弹窗 - 使用 "alt" 属性作为文本部分的内容
var img = document.getElementById('myImg');
var modalImg = document.getElementById("img01");
var captionText = document.getElementById("caption");
img.onclick = function(){
    modal.style.display = "block";
    modalImg.src = this.src;
    captionText.innerHTML = this.alt;
}
 
// 获取 <span> 元素，设置关闭按钮
var span = document.getElementsByClassName("close")[0];
 
// 当点击 (x), 关闭弹窗
span.onclick = function() { 
  modal.style.display = "none";
}
```

> 这个特效使用到的js就比较简单，给图片和关闭按钮绑定click事件就可以了。

## 5、案例3--瀑布流 ##


![瀑布流布局](https://segmentfault.com/img/bVUAN1)

### 5.1、案例分析 ###

 1. 瀑布流布局被广泛运用于图片展示，但是展示的图片需要宽度相等，而高度可以自适应；
 2. 使用js获取图片宽度、获取窗帘宽度，计算一行能容纳的图片个数，然后获取图片的高度，控制从第二行开始的图片先排在上一行中高度最低的下面，以此形成瀑布流的布局；
 3. 使用window.onresize，当窗口大小变化的时候重新计算窗口宽度，以重新进行排版；
 4. 使用window.onscroll，当滚动滚轮时，判断是否显示到最后一张图片，在这之前会从模拟的数据中读取图片地址，再添加到页面里，使得页面可以一直滚动下去，模拟了百度图片的效果；
 5. 在css中加入了过渡，使得图片位置变化的时候有过渡效果；

> 本特效的网页内容和样式都很简单就不写在这了，主要讲解一下js代码，下面上代码

```
window.onload = function(){ //判断页面内容与样式是否加载完毕
    imglocation("container","box");
    var imgData = {"data":[{"src":"img2.jpg"},{"src":"img3.jpg"},{"src":"img4.jpg"},{"src":"img5.jpg"},{"src":"img6.jpg"}]};//模拟的图片地址
    
    window.onscroll = function(){//监听滚轮事件
      if(checkFlag()){//checkFlag()是判断是否快要显示最后一张图，如果是返回true，否则返回false
        for(var i = 0;i<imgData.data.length;i++) {//js创建节点，并设置属性
          var ccontent = document.createElement("div");
          ccontent.className = "box";
          cparent.appendChild(ccontent);
          var boximg = document.createElement("div");
          boximg.className = "box_img";
          ccontent.appendChild(boximg);
          var Img = document.createElement("img");
          Img.src = "img/" + imgData.data[i].src;
          boximg.appendChild(Img);
        }
      }
      imglocation("container","box");//排版函数
    }

    window.onresize = function() {//当窗口改变时再次调用排版函数
      imglocation("container","box");
    }
  }
```

 **排版函数**

```
function imglocation(parent,content) {
    //将parent中内容全部取出,获取所有的图片
    var ccontent = getChildElement(cparent,content);
    var imgWidth = ccontent[0].clientWidth;//图片的宽度
    var OWidth = document.documentElement.clientWidth;//窗口宽度
    var num = Math.floor(OWidth/imgWidth);//计算一行能摆放的图片数
    cparent.style.cssText = "width:" + imgWidth * num + "px;margin: 0 auto;";
    //设置css样式
    var BoxheightArr = [];
    for(var i = 0;i<ccontent.length;i++){
      if(i<num){
        BoxheightArr[i] = ccontent[i].offsetHeight;
        ccontent[i].style = "";
      } else {
        var minHeight = Math.min.apply(null,BoxheightArr);
        var minIndex = getminheightLocation(BoxheightArr,minHeight);
        ccontent[i].style.position = "absolute";
        ccontent[i].style.top = minHeight + "px";
        ccontent[i].style.left = ccontent[minIndex].offsetLeft + "px";
        BoxheightArr[minIndex] = BoxheightArr[minIndex] + ccontent[i].offsetHeight;
      }
    }
  }
```

> 完整代码请到[GitHub][2]上观看，欢迎star

## 总结 ##

虽然我这里写的是js实现特效，但是如果能用css实现，那么最后是用css，这就看哪种方法方便了，择优嘛；好啦，今天就写这三个案例吧，希望大家看完这三个案例，能够有些想法，最后是要动手写写，光看是没有用的。
最后，谢谢各位的观看，想看源代码的可以去我的[GitHub][3]上，欢迎star或者添加更多的js特效；如果有哪里写的不好也请指出来，我会马上去改正；如果感到对你有用欢迎收藏点赞！谢谢！


  [1]: https://github.com/HEternally/Common-web-effects
  [2]: https://github.com/HEternally/Common-web-effects
  [3]: https://github.com/HEternally/Common-web-effects
