title: sass基本语法
date: 2014-03-15 23:26:51
categories:  shuang
tags: css
---

Sass中的变量通常以$开头，并且变量名和值之间使用冒号分隔开。如果值后面有!default，则表示该值为默认值。除此之外，声明变量还可以以#{$variable}的形式插入。

<!--more-->
##变量
Sass中的变量通常以$开头，并且变量名和值之间使用冒号分隔开。如果值后面有!default，则表示该值为默认值。除此之外，声明变量还可以以#{$variable}的形式插入。

例如：
``` sass
$color : #ffffff !default ;
$borderDirection : top !default ;
.border-#{$borderDirection} {
    border-#{$borderDirection} : 1px solid red ; 
}
```
当然，一个变量还可以定义多个值，然后通过nth($variables,index)来调用其中的某个值。
例如
``` sass
：$linkColor : #08c #333 !default ; 
// 第一个值为默认值，第二个值为鼠标滑过值
a{
    color : nth($linkColor , 1);
    & : hover{
    color : nth($linkColor , 2);
    }
}
```
##嵌套
Sass中的嵌套有两种，分别为：选择器嵌套和属性嵌套。
###选择器嵌套
选择器的嵌套就是在一个选择器中嵌套别的选择器，从而实现样式的继承。通常在嵌套过程中还可以使用&表示父元素。
例如：
``` sass
div{
                    border : 1px solid red;
                    width:100px;
                    a{
                         color : blue;
                         & : hover{
                                   color : yellow;
                         }
                    }
          }
```
###属性嵌套
属性嵌套是指某些属性以同一个单词开始，如border-width，border-color，都是以border开头的。

例如：
``` sass
div{
                    border : {
                                        style : solid;
                                        left{
                                                  width : 4px;
                                                  color : #888;
                                        }
                                        right{
                                                  width : 2px;
                                                  color : #ccc;
                                        }
                    }
          }
```
##混合（mixin）
mixin是Sass中最强大的特性，它可以将一部分样式抽出，作为单独定义的模块，从而能被很多选择器使用。除此之外，mixin最强大的功能在于它可以传递参数。
 * 声明mixin（@mixin）
声明mixin时使用@mixin，然后紧跟@mixin的名字。参数名通常以$开始，格式为：$参数名：参数值。当有多个参数时，用逗号分隔开。
例如：
``` sass
@mixin opacity($opacity : 50){
               opacity : $opacity / 100;
               filter : alpha(opacity = $opacity);
          }
```
 * 调用mixin（@include）
调用定义好的mixin需要使用@include，然后紧跟其要调用的mixin名。如果使用默认的参数值，则可以省略括号。
例如：
``` sass
.opacity{
                         @include opacity;
          }
          .opacity-80{
                         @include opacity(80);
          }
```
@content
 * @content使mixin接受一整块样式，接受的样式从@content开始。
例如：
``` sass
@mixin max-screen($res){
               @media only screen and (max-width : $res){
                    @content;
               }
          }
          @include max-screen(480px){
               body{
                         color : red;
               }
          }
          // CSS style
          @media only screen and (max-width : 480px){
               body{
                         color : red;
               }
          }
```


##继承
Sass中，选择器继承使选择器继承另一个选择器的所有样式，并联合申明。使用@extend实现选择器的继承，后面紧跟需要继承的选择器。
例如：
``` sass
h1{
                    border : 4px solid red;
          }
          div{
                    @extend h1;
                    border-width : 2px;
          }
```
###占位选择器
占位选择器用%定义，使用这种选择器的好处在于，当不调用时不会产生多余的CSS文件。
例如：
``` sass
%ir{
                    color : transparent;
                    text-shadow : none;
                    background-color : transparent;
                    border : 0;
          }
          %clearfix{
                    @if $lte7{
                              *zoom : 1;
                    }
                    & : before,
                    & : after{
                                   content : "";
                                   display : table;
                                   font : 0/0 a;
                    }
                    & : after{
                                   clear : both;
                    }
          }
          #header{
                    h1{
                              @extend %ir;
                              width : 300px;
                    }
          }
          .ir{
               @extend %ir;
          }
          // CSS style
          #header h1,
          .ir{
               color : transparent;
               text-shadow : none;
               background-color : transparent;
               border : 0;
          }
          #header h1{
               width : 300px;
          }
```
##运算
Sass具有运算特性，可以对数值型的Value（如：数字、颜色、变量等）。
例如：
``` sass
$baseFontSize : 14px !default;
          $baseLineHeight : 1.5 !default;
          $baseGap : $baseFontSize * $baseLineHeight !default;
          $halfBaseGap : $baseGap / 2 !default;
          $smallFontSize : $baseFontSize - 2px !default;
```

##函数
Sass定义了很多函数，常用的是颜色函数，而颜色函数中lighten（减淡）和darken（加深）用的最多。其调用方法为：
>lighten($color , $amount)

>darken($color , $amount)

其中第一个参数$color是颜色值，而第二个参数$amount则是百分比。
当然，在Sass中也可以自己定义函数，以@function开始。
例如：
``` sass
$baseFontSize : 10px !default;
          $gray : #ccc !default;
          
          // 自己定义的函数：将px转换为em
          @function pxToRem($px){
               @return $px / baseFontSize * 1rem;
          }
          .test{
                    font-size : pxToRem(16px);
                    color : darken($gray , 10%);
          }
```

##变量范围
Sass的变量范围有3种情况：
 1. 若定义了一个全局变量，值为A，在某个选择器中覆盖了该全局变量的值，新的值为B，则之后凡是用到该全局变量的地方，值都是B。
 2. 若只在某个选择器中定义了一个变量，那么外面的其他选择器是不能调用该变量的。
 3. 若定义了有!default默认值的变量，它会先找全局中有没有对该变量重新定义。如果没有，则使用默认值；如果有，则使用重新定义的值。
例如：
``` sass
$color : #fff;
          .test{
                    $color : red;
                    color : $color;
          }
          .test2{
                    color : $color;
          }
          // CSS style
          .test{
                    color : red;
          }
          .test2{
                    color : red;
          }
```

##其他
 * 条件判断
@if可一个条件单独使用，也可和@else结合多条件使用。
例如：
``` sass 
$type : monster;
          p{
               @if $type == ocean{
                    color : blue;
               }
               @else if $type == matador{
                    color : red;
               }
               @else if $type == monster{
                    color : green;
               }
               @else{
                    color : black;
               }
          }
```
 * for循环
for循环有两种形式，分别为：
 > @for $var from <start> through <end>
 > @for $var from <start> to <end>

其中，$var是变量，start是起始值，end是结束值。
*注：两种形式的区别在于：through要包含end，而to不包含end。
例如：

``` sass
@for $i from 1 through 3{
               .item-#{$i}{
                    width : 2em * $i;
               }
          }
          // CSS style
          .item-1{
               width : 2em;
          }
          .item-2{
               width : 4em;
          }
          .item-3{
               width : 6em;
          }
```

 * 三目判断
 >if($condition , $if_true, $if_false)，

三个参数分别表示条件、条件为真的值，条件为假的值。
例如：
``` sass
 if(true , 1px , 2px)      // 值为1px
          if(false , 1px , 2px)     // 值为2px
```










