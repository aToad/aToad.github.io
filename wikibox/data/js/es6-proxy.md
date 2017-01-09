# Proxy

[home](index) > [js](js:index) > [Proxy](js:es6-proxy)

##### 1.概述
Proxy 用于修改某些操作的默认行为。比如，我们可以修改“访问一个对象的属性，然后获得该属性的值”这个过程中的一些行为。

    let o = {name: "toad"};
    console.log(o.name); // 访问对象 o 的 name 属性

以上代码是我们很熟悉的属性值获取操作。有了 Proxy，我们可以在这个操作中做点手脚。

    let o = {name: "toad"},
        proxy = new Proxy(o, {
            get(target, key) {
                return "属性" + key + "已经被做了手脚，返回了意料外的值";
            }
        });
    console.log(proxy.name); //

proxy 对象可以理解我们自己定制的 o 的对象，官方说法是：proxy 是 o 的代理。代理 proxy 和 目标对象 o 是两个不同但相互联系的对象。利用 proxy，我们可以更好地操作 o。上例的意思就是：你如果要访问 o 的 name 属性，就要经过 get() 方法的拦截。

##### 2.代理的创建

    let proxy = new Proxy(target, handler);

##### 3.代理能修改的操作

3.1 属性获取：get 代理

    let proxy = new Proxy({}, {
        get(target, key) {
            return "获得了属性"
        }   
    })；

3.2 设置属性：set 代理

    let proxy = new Proxy({}, {
        set(target, key, value, receiver) {
            target[key] = "proxy " + value;
        }
    });

3.3 当代理的对象是函数时，可以使用 apply 代理

    let target = function() {console.log("I am the target.")};
    let handler = {
        apply(target, ctx, handler) {
            console.log("I am the proxy.");
        }
    };
    var proxy = new Proxy(target, handler);
    proxy(); // I am the proxy.

3.4 判断对象是否有某个属性时，可以使用 has 代理

    let obj = {
        $prop: "foo",
        prop: "foo"
    };
    let handler = {
        has(target, key) {
            if (key[0] === "$") {
                return false;
            }
            return key in target;
        }
    };
    let proxy = new Proxy(obj, handler);
    console.log("$prop" in proxy);

3.5 当代理的对象是函数，可以使用 construct 代理，其返回值必须是对象。此代理在 new 代理对象时生效。

    var proxy = new Proxy(function() {}, {
        construct(target, args) { // args: 参数数组
            console.log(args.forEach);
            return {value: args[0]}
        }
    });

    let o = new proxy(1, 2, 3);
    console.log(o);

3.6 当要删除某个属性时，可以使用 deleteProperty 代理。如果此代理返回 false 或者报错，该属性无法被删除。

    var target = {
    	"$prop": "foo",
    	"prop": "foo"
    };
    var handler = {
        deleteProperty(target, key) {
            if (key[0] === "$") {
            	throw new Error(`Could not delete private property "${key}"`);
            }
            delete target[key];
        }
    }
    var proxy = new Proxy(target, handler);

    console.log(proxy);
    delete proxy.prop;
    console.log(proxy);

    delete proxy["$prop"]; // 报错

3.7 当要设置属性时，我们还可以使用 defineProperty 代理。

    var target = {};
    var handler = {
        defineProperty(target, key, descriptor) {
            console.log(key);
            return false;
        }
    }
    var proxy = new Proxy(target, handler);

    proxy.foo = "bar";
    console.log(proxy.foo); // undefined

3.8 当要获取属性的描述符对象时，可以使用 getOwnPropertyDescrptor 代理。

    var target = {"name": "Toad", "$name": "Toad"};
    var handler = {
        getOwnPropertyDescriptor(target, key) {
            if (key[0] === "$") {
                return;
            }
            return Object.getOwnPropertyDescriptor(target, key);
        }
    };
    var proxy = new Proxy(target, handler);

    console.log(Object.getOwnPropertyDescriptor(proxy, "name"));
    console.log(Object.getOwnPropertyDescriptor(proxy, "$name")); // undefined

3.9 当要获得某个对象的原型对象时，可以使用 getPrototypeOf 代理，此代理必须返回对象。

    var prototype = {foo: "bar"},
        target = Object.create(prototype),
        handler = {
            getPrototypeOf(target) {
                return {"abc": "def"};
            }
        },
        proxy = new Proxy(target, handler);

    console.log(Object.getPrototypeOf(proxy));

3.10 当要设置某个对象的原型对象时，可以使用 setPrototpyeOf() 代理。

##### 4.Proxy.revocable()
此方法返回一个对象，该对象的 proxy 属性是 Proxy 实例对象，revoke 属性是一个方法，用于取消代理。

    let target = {},
        handler = {};

    let {proxy, revoke} = Proxy.revocable(target, handler);
    proxy.foo = "bar";
    console.log(proxy.foo);

    revoke(); // 取消代理
    console.log(proxy.foo); // 报错

##### 5.对某个对象使用代理后，代理对象的方法中的 this 指向代理对象
