# Generator

[home](index) > [js](js:index) > [Symbol](js:es6-generator)

1.概述

Generator 函数内部保存有多种状态。
Generator 函数被调用后不执行代码块，不返回其 return 后的值。
Generator 函数是一个遍历器生成函数，调用后返回一个遍历器对象。
Generator 函数返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

2.写法

    function * generator() {}; // 函数声明
    let generator = function * () {}; // 表达式

3.yield 语句

yield 语句只能在 Generator 函数中出现。
一条 yield 语句代表 Generator 函数的一个状态。
Generator 函数是分段执行，分段的节点就是 yield 语句。
Generator 函数的每段执行到 yield 语句为止，返回 yield 后面的值。
Generator 函数的分段执行启动器是遍历器对象的 next 方法。

    function * foo() {
        yield 1;
        yield 2;
        return 3;
    }
    var bar = foo();
    bar.next(); // 1
    bar.next(); // 2
    bar.next(); // 3
    bar.next(); // undefined

4.next() 方法的参数

yield 语句没有返回值或者说返回 undefined，当我们为 next() 方法添加一个参数，该参数即为上一条 yield 语句的返回值。

    function * foo(arg) {
        var x = yield arg * 2;
        var y = yield (x / 2);
        var z = yield (y + 10);
        return x + y + z
    }
    var bar = foo(2);
    bar.next(); // 4
    bar.next(4); // 2
    bar.next(8); // 18
    bar.next(10); // 22

5.总结

+ Generator 函数利用 yield 语句实现函数的暂停执行与恢复执行。
+ Generator 函数利用 next() 实现函数体内外的数据交换。
+ Generator 函数具备良好的错误捕获能力。
