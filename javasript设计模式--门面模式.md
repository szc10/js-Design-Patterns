##javascript设计模式 门面模式

####什么是门面模式

```
将一个复杂的功能简化一个简单的函数调用,简单的来说,我使用榨汁机榨苹果,我只要将苹果放进去榨汁机就可以了,不需要了解榨汁机的内部设计.这一模式提供了面向一种更大型的代码体提供了一个的更高级别的舒适的接口，隐藏了其真正的潜在复杂性,方便的开发者的调用.例如jquery的$("").val()的实现就是一个门面模式
```

####门面模式的作用

```
1.简化接口的调用
2.消除类与使用它的客户代码之间的耦合 (解耦)
```

####简单的门面模式

在实现html元素跨浏览器事件监听的实现,我们在使用这个函数时无需考虑这个浏览器是chrome还是IE,都可以完成对dom的监听.

```javascript
   var addEvent = function (ele, type, fn) {
        if (window.addEventListener) {
            ele.addEventListener(type, fn, false);  //实现webkit内核,Gecko内核事件监听
        }
        else if (window.attachEvent) {
            ele.attachEvent("on" + type, fn);  //实现IE事件监听
        }
        else {
            ele["on" + type] = fn;  //不知名浏览器实现事件监听
        }
    }
```
PS:webkit内核和IE内核实现时间监听的接口不一致

####更复杂的门面模式
门面模式可以通过对一系列函数进行组合,来达到简化API的复杂性

下面举一个例子(在一个单体中实现阻止冒泡过程,和阻止默认事件)

```
    var DED = {
        /**
         * 阻止冒泡事件
         */
        stopBubble: function (ev) {
            if (ev.stopPropagation) {
                ev.stopPropagation();  //w3c interface
            }
            else {
                ev.cancelable = true;  //IE interface
            }
        },
        /**
         * 阻止默认事件
         */
        stopDefault: function (ev) {
            if (ev.preventDefault) {
                ev.preventDefault(); //w3c interface
            }
            else {
                ev.returnValue = false;  //IE interface
            }
        },
        /**
         *  通过一个组合将这两个事件,放在一个接口中
         */
        stopEvent: function (ev) {
            DED.stopBubble(ev);
            DED.stopDefault(ev);
        }
    };
```

####门面模式的使用场合

```
1.用于反复成组出现的代码
2.创建的自己的工具库,在使用时无需考虑到浏览器的变化,如上例的addEvent()
```

####门面模式的好处

```
1.编写一次的组合代码,可以反复的使用它
2.降低了对外部代码的依赖性,为应用系统的开发增加了灵活性
3.减少了模块之间的耦合,方便对整个系统进行修改
```

####门面模式的弊端

```
1.一个可以庞大的门面模式,在使用的时候,可能会常常执行一些我们不需要的任务
```