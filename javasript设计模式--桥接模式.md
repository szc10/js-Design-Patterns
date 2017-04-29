##javascript设计模式 桥接模式

####什么是桥接模式

```
讲事件的实现部分和抽象部分分离开来,以便两者可以独立的变化
```

####桥接模式的作用

```
 将抽象与其实现隔离开来,以便两者可以独立的变化
```

####简单的桥接模式

abstractFn是抽象部分， 而actualFn是实现部分。 他们完全可以独自变化互不影响。 

```javascript
     /**
     * 定义一个实际的接口,处理判断一个数的奇偶
     * @param temp
     * @returns {*}
     */
    var actualFn = function (temp) {
        if (temp % 2 != 0)
            return "the num is odd";
        else
            return "The num is even";
    }
    /**
     * 定义一个抽象的接口,通过传入相关的处理函数,来判断一个数的奇偶
     * @param num
     * @param fn
     */
    var abstractFn = function (num, fn) {
        var result = fn.call(window, num);
        console.log(result);
    }
    // 接口的数据调用
    abstractFn(6, actualFn);
```

####桥接模式的方法访问类中的私有成员

```javascript
    /**
     * 定义一个类
     * @constructor
     */
    var Public = function () {
        var secret = 3;
        /**
         * 通过桥接模式访问类中的私有变量
         * @returns {number}
         */
        this.privilegedGetter = function () {
            return secret;
        }
    }
```
####用桥接模式连接多个类
```javascript
    /**
     * 定义类1
     * @param a
     * @param b
     * @constructor
     */
    var Class1 = function (a, b) {
        this.a = a;
        this.b = b;
    };
    /**
     * 定义类2
     * @param c
     * @constructor
     */
    var Class2 = function (c) {
        this.c = c;
    }
    /**
     * 定义一个相关的桥接类
     * @param a
     * @param b
     * @param c
     * @constructor
     */
    var BridgeClass = function (a, b, c) {
        this.class1 = new Class1(a, b);
        this.class2 = new Class2(c);
    }
```

其中的桥接类将相关的数据分发给相关的处理类,而处理类与桥接类可以进行相互独立的改变.

####桥接模式的使用场合

```
js编程常常基于事件驱动开发,因此接口“可桥接”，实际上也就是可适配。
```

####桥接模式的好处

```
1.将功能的抽象和其实现隔离开,有助于独立地管理软件的各组成部分
2.分离了抽象与实现,有助于bug的寻找和数据的追踪
```

####桥接模式的弊端

```
使用桥接元素,每次都要增加一次函数调用,对应用程序的性能会产生一定的影响
```