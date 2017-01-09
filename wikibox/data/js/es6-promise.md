# Promise

[home](index) > [js](js:index) > [Promise](js:es6-promise)

1.概述

+ Promise 对象用于实现异步操作。

2.创建实例

    let promise = new Promise((resolve, reject) => {
        // code
    });

Promise() 构造函数接受一个函数作为参数，该函数有两个参数，这两个参数也是函数，分别对应不同的状态变化。异步操作成功时，执行 resolve 函数，异步操作失败时，执行 reject 函数。传入这两个函数的参数，可以在函数外部中获取。

3.Promise.prototype.then()

    function loadImageAsync(url) {
        return new Promise((resolve, reject) => {
            let image = new Image();

            image.onload = () => {resolve(image)};
            image.onerror = () => {reject(new Error("Could not find image at " + url))};

            image.src = url;
        });
    }

    loadImageAsync("dest/a.jpg").then((value) => {
        document.body.appendChild(value); // value 即 resolve 的参数：image
    }, (error) => console.log(error)); // error 即 reject 的参数：Error 的实例

实例方法 then 接收两个函数参数，这两个函数是否执行取决于异步操作的成功与否。如果构造函数中的 resolve 函数能够执行，then 的第一个函数参数就会执行，并能获得传入resolve函数的参数作为参数；如果构造函数中的 reject 函数能够执行，then 的第二个函数参数就会执行，并能获得传入reject函数的参数作为参数。then 方法返回一个新的 Promise 实例。当链式调用 then 方法时，上一个 then 方法的返回值将作为下一个 then 方法的参数。
