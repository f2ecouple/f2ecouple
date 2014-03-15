title: 加速CSS
date: 2014-03-15 13:41:52
tags: css
categories: xiong
---

##关于平滑
当人们感觉到“平滑”时，一般是每一秒在屏幕上绘画的帧数超过了某一个值（FPS),
有一个非官方的值，即超过了30或者是更多。
本地应用要比Web apps感觉良好，很大的原因在于执行swiping, tapping, pinching等保持一个较高的FPS值。
但是现在很多的浏览器厂商都对一些特定的CSS属性提供了图形硬件加速。

###repaint, reflow/relayout, restyle
不同的浏览器有不同的渲染过程，但是大部分浏览器都拥有下面的过程：
 * parse HTML source code -> DOM tree
 * parse CSS代码，在这个部分就会剔出掉一些无用的前缀样式，比如-moz, -webkit等
 *下面就到了最有兴趣的一部分了，rendering tree。render tree更像是缩短版的，但是也有一些不一样的地方,
  render tree 是知道样式信息的，所以你对一个div设置display: none，它是不会出现在render tree 中的。同样的
  还有其它的不可见的元素，比如head和head里面的元素。
####reflow & repaint
当页面完成默认的绘制过后，当改变创建render tree的某些输入信息是会导致reflow或者是repaint
1.render tree的一部分需要重新验证或者是node的位置信息需要被重新计算，会导致发生reflow.
2.当屏幕的一部分需要被更新的时候，不管是一个节点的几何属性发生更改或者是样式更改。这种屏幕更新的
操作叫做repaint.

``` css
var bStyle = document.body.style;
bStyle.padding = "20px"; // reflow, repaint;
bStyle.border = "10px solid red" //another reflow 和repaint
bStyle.color = "blue" // repaint only
```

###可以启动GPU加速的属性
 * 常规的布局(layout)组合
 * CSS3transitions
 * CSS3 3D transforms
 * Canvas 绘画
 * WebGL 3D 绘画



