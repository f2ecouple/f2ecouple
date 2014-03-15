title: snap面试
date: 2014-03-15 14:02:07
tags: javascript
categories: xiong
---

下午去某实验室面试了他们家的前端工程师的职位，这应该是除了当时面试实习生后的第一次前端方面的面试，虽然十有八九都挂了，但有些东西还是值得记录一下的。
<!--more-->

先说一下关于这次面试的体验吧，一共经历了笔试和三个人的面试，面试官对新的东西都了解的比较多，其中有一个哥们儿不是很喜欢。一副看什么都很easy的样子。
总之这次面试还是暴露出自己不少的问题：

 * 对事件的理解不够透彻
 * CSS3相关属性的优缺点掌握不牢靠
 * HTML5只是部分了解，但是还是缺少深入的了解，居然把标签名记错了   


##面试题   

1.下面函数的输出为什么？

``` javascript
    var a = true;
    function whatever () {
        if (a) {
        var a = false;
        console.log(a);
        }
        a = true;
    }
    whatever();
    console.log(a);
```   



2.输出的值为false还是true, 如果为false请将其改为true   

``` javascript
    function Person (name) {
        this.name = name;
    }
    var stu = new Person("test");
    var sto = Person("test2");
    console.log((stu instanceof Person) && (sto instanceof Persion));
```   

3.实现mixin的方法，并说明他和继承的区别   

4.实行一个debounce(fn, waiting)的方法，即当一个函数被连续调用时，只执行最近的一个方法。比如var test = debounce(fn, 300)绑定在scroll上，当scroll时
只执行最近的一个300ms   

5.实现一个slide的来回切换和fadein的效果   

6.未知大小图片在固定大小框内的垂直居中以及等比例缩放。   

7.当在页面种出发点击事件时，输出为什么？

``` javascript
    document.addEventListener('click', function (e) {
        console.log(e.type, 1);
    });
    document.addEventListener('click', function (e) {
        console.log(e.type, 2);
    });
    document.addEventListener('click', function (e) {
        console.log(e.type, 3);
    });

``` 

8.当在页面种出发点击事件时，输出为什么？

``` javascript
    document.addEventListener('click', function (e) {
        console.log(e.type, "A");
    });
    document.body.addEventListener('click', function (e) {
        console.log(e.type, "B");
    });
    document.addEventListener('click', function (e) {
        console.log(e.type, "C");
    });
```   

9.当在页面种出发点击事件时，输出为什么？


``` javascript
    document.addEventListener('click', function (e) {
        console.log(e.type, "A");
    });
    document.body.addEventListener('click', function (e) {
        console.log(e.type, "B");
        e.stopPropagation();
    }, true);
    document.addEventListener('click', function (e) {
        console.log(e.type, "C");
    });
```   

10.当页面的布局为body>div和body>div>div时，布局分别是怎样的？

``` html5
    div {
        width: 100px;
        height: 100px;

        position: absolute;
        top: 10px;
        bottom: 10px;
        left: 10px;
        right: 10px;

        margin: 50px;
        border: 10px solid black;
        padding: 10px;

        float: left;
    }
    当为body > div 和body > div > div时，页面的表现分别是怎样的？
```

11.下哪些注释可以实现继承

``` javascript
function Person (name) {
    this._name = name;
}
Person.prototype.getName = function () {
    return this.name;
}

function Student(name, score) {
    Person.call(this, name);
    this._score = score;
}

// Student.prototype = Person.prototype;
// Student.prototype = new Person();
// Student.prototype = Object.create(Person.prototype);
Student.prototype.getScore = function () {
    return this.name + ":" + this.score;
}
```


设计一套Restful的API,以及API缓存相关的知识。
