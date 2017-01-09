# Symbol

[home](index) > [js](js:index) > [Symbol](js:es6-symbol)

1.概述

+ Symbol 类型是 ES6 中新增的数据类型，是 JavaScript 中的第 7 种数据类型。
+ Symbol 类型是一种类字符串的数据类型，表示值是独一无二的。

2.创建标记

+ Symbol() 函数用于新建一个新的标记。
+ Symbol() 接受一个字符串参数，该参数是对新标记的描述。
+ 尽管两个标记的描述字符串相同，但是这两个标记依然不相等。
+ 如果要创建两个相同的标记，可以使用 Symbol.for(string) 来创建新标记。
+ Symbol.keyFor()获得由 Symbol.for() 创建的标记的描述字符串。

3.作为对象属性的标记

+ 使用标记作为对象属性名可以保证属性名不会起冲突。
+ 在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
+ 如果要遍历 Symbol 属性名，可以使用 Object.getOwnPropertySymbols() 或 Reflect.ownKeys()。

4.内置的Symbol值
