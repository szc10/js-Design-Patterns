##javascript设计模式 单体模式

####什么是单体模式

```
它将代码组织为一个逻辑单元的手段，这个逻辑单元中的代码可以通过单一变量进行访问。其中单体对象只存在一份实例
```

####最简单的单体结构

最简单的单体结构是一个对象指针，它把一批有关联的方法和属性组织在一起

```javascript
var Singleton = {
    a : 'this is a',
    b : 'this is b',
    method : function(){
        alert('this is a method');
    }
};
```
在这个实例中，我们直接可以通过变量名Singleton来访问单体

```javascript
var a=Singleton.a;   //this is a
Singleton.method();   // alert this is a method
```
####使用闭包的单体结构
使用闭包可以向单体结构中写入私有成员，并且保证外部的代码不会对其进行修改

```javascript
var Singleton = (function(){
    var privateAttribute1 = false;
    var privateAttribute2 = true;
    function privateMethod(){
        console.log(privateAttribute1);
    }
    return {
        publicAttribute:10,
        publicMethod:function(){
            console.log(privateAttribute1);
        }
    }
})();
```
这种单体模式又称为模块模式，它可以把一批相关方法和属性组织为模块，并且可以起到命名空间的作用

####单体加载过程中的多项实例化

在单体对象加载的过程中，有些单体是资源密集型或者是配置开销甚大的单体，因此比较合理做法为将实例化推迟到需要加载它的时候——惰性加载

惰性加载的奥妙之处，访问对象的一个静态方法（例如getInstance），方法会检查该单体是否已经实例化，如果还没有，那么它将创建并且返回其实例

```javascript
    var Singleton = (function() {
        var uniqueInstance;   //private attribute that holds the single instance
        function constructor() {  //All of the normal singleton code goes here
            var privateAttribute1 = false;
            var privateAttribute2 = true;
            function privateMethod() {
                console.log(privateAttribute1);
            }
            return {
                publicAttribute: 10,
                publicMethod: function () {
                    console.log(privateAttribute1);
                }
            }
        }
        return {
            getInstance: function () {
                if (!uniqueInstance) {
                    uniqueInstance = constructor();
                }
                return uniqueInstance;
            }
        }
    }());
```
**Singleton的使用**

```javascript
Singleton.getInstance().publicMethod();  //false
```
####单体的使用场合

```
1.创建命名空间来减少全局变量的数量
2.使用那些开销较大却使用较少的组件,可以进行惰性实例化
```