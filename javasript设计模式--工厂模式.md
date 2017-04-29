##javascript设计模式 工厂模式

####什么是工厂模式

```
我们之前创造实例常常采用new关键字和构造函数,可是这样常常会导致两个类产生依赖关系.因此,我们通过创建一个对象的公共接口，我们可以根据相关的参数,控制创造的实例对象.
```

####展现一个简单工厂模式

下面举一个例子,一个简单的蛋糕工厂根据需求生产不同的蛋糕

```javascript
    /**
     * 创建蛋糕的种类
     */
    var VanillaCake = function () {   //创建类香草蛋糕
        this.type = "vanilla cake";
    }
    var ChocolateCake = function () {  //创建类巧克力蛋糕
        this.type = "chocolate cake";
    }
    var CreamCake = function () {  //创建奶油蛋糕
        this.type = "cream cake";
    }
    /**
     * 创建制造蛋糕的工厂
     */
    var CakeFactory = function () {
    };
    CakeFactory.prototype.produceCake = function (cakeType) {
        var cake;
        switch (cakeType) {
            case 'vanilla':
                cake = new VanillaCake();
                break;
            case 'chocolate':
                cake = new ChocolateCake();
                break;
            default:
                cake = new CreamCake();
        }
        cake.display = function () {  // 为这个蛋糕组装相应的外壳(方法)
            alert(this.type);
        }
        return cake;
    }
```

我们如何使用,我们需要对工厂进行实例化,或者在制造工厂的时候,直接以单体的形式存在

```
var cakeFactory = new CakeFactory();
    var cake = cakeFactory.produceCake();
    cake.display();
```

####进一步复杂工厂模式
如果在生产目录中增加一种cake,我们就要在produceCake修改代码,根据面对对象的思想,我们可以把创建cake(实例)的过程交给一个简单的工厂对象 (workshop)

```javascript
    /**
     * 创建蛋糕的种类
     */
    var VanillaCake = function () {   //创建类香草蛋糕
        this.type = "vanilla cake";
    }
    var ChocolateCake = function () {  //创建类巧克力蛋糕
        this.type = "chocolate cake";
    }
    var CreamCake = function () {  //创建奶油蛋糕
        this.type = "cream cake";
    }
    /**
     * 创建制造蛋糕的工厂车间
     */
    var WorkShop = {
        createCake: function (cakeType) {
            var cake;
            switch (cakeType) {
                case 'vanilla':
                    cake = new VanillaCake();
                    break;
                case 'chocolate':
                    cake = new ChocolateCake();
                    break;
                default:
                    cake = new CreamCake();
            }
            return cake;
        }
    }
    /**
     * 创建制造蛋糕的工厂
     */
    var CakeFactory = function () {
    };
    CakeFactory.prototype.produceCake = function (cakeType) {
        var cake = WorkShop.createCake(cakeType);  //将生产cake的任务交与车间
        cake.display = function () {  // 为这个组装相应的零件
            alert(this.type);
        }
        return cake;
    }
    var cakeFactory = new CakeFactory();
    var cake = cakeFactory.produceCake();
    cake.display();
```

WorkShop 是一个单体,将createCake的方法封装在一个命名空间中,这样就可以将cake的信息集中在一个地方管理,接口的内部修改也变得更加容易

####真正的工厂模式

真正的工厂模式与简单的工厂模式的区别在于,它不是用一个类和一个对象来创建,而是使用一个子类

看下面的一个例子,我们在创建蛋糕的时候,由一个具体蛋糕工厂实例化蛋糕.我们将CakeFactory设计成抽象类,让其子类来来生产蛋糕.

```javascript
    /**
     *  创建继承函数 (使用类型继承)
     */
    var extend = function (subClass, superClass) {
        var F = function () {
        };
        F.prototype = superClass.prototype;
        subClass.prototype = new F();
        subClass.prototype.constructor = subClass;
    }
    /**
     * 创建蛋糕的种类
     */
    var VanillaCake = function () {   //创建类香草蛋糕
        this.type = "vanilla cake";
    }
    var ChocolateCake = function () {  //创建类巧克力蛋糕
        this.type = "chocolate cake";
    }
    var CreamCake = function () {  //创建奶油蛋糕
        this.type = "cream cake";
    }
    /**
     * 创建制造抽象蛋糕的工厂
     */
    var CakeFactory = function () {
    };
    CakeFactory.prototype.produceCake = function (cakeType) {
        var cake = this.createCake(cakeType);  //将生产cake的任务交与具体工厂
        cake.display = function () {  // 为这个组装相应的零件
            alert(this.type);
        }
        return cake;
    };
    /**
     * 创建Ak蛋糕工厂(子类)
     * @constructor
     */
    var AkCakeFactory = function () {
    };
    extend(AkCakeFactory, CakeFactory); //Ak蛋糕工厂基于抽象的蛋糕工厂
    AkCakeFactory.prototype.createCake = function (cakeType) {
        var cake;
        switch (cakeType) {
            case 'vanilla':
                cake = new VanillaCake();
                break;
            case 'chocolate':
                cake = new ChocolateCake();
                break;
            default:
                cake = new CreamCake();
        }
        return cake;
    };
    var newCakeFactory = new AkCakeFactory();
    var cake = newCakeFactory.produceCake();
    cake.display();
```

####工厂模式的使用场合

* 对象或者组件设置涉及到高程度级别的复杂度时,节省相关的设置开销。
* 根据所在的环境生成不同对象的实体时,例如创建XHR实体。
* 用许多的小型对象组成一个大对象时。

####工厂模式好处
1.有助于消除对象之间的耦合

2.将所有的实例化代码集中放置在一个位置,简化更换的类和运行过程中动态选择类的工作

3.可以用一个接口的调用取代一个具体的实现,而不是调用 new 关键词,这样有助于模块化代码的实现

####工厂模式弊处
1.过度使用工厂模式,忽视普通构造函数的作用.

2.有些类希望可以公开的实例化,这样使代码清晰易读.

####如何解决弊端

我们通过编写一个对象加载器,由加载器进行对象的实例化.

```javascript
/**
 * @param _class 类的名称
 */
var objectManager=((function () {
    return{
        create:function(_class){
            var _object = {}
            _object.__proto__ =  _class.prototype;
            _object.__proto__.constructor = _class ;
            var args = Array.prototype.slice.call(arguments);  //提取构造器的参数
            args=args.slice(1,args.length);
            _class.apply(_object,args);  //将构造器的参数传入需要实例化类的构造函数中
            return _object;
        }
    }
})());
```
如何利用这个这个加载器

```
/**
 * 声明一个类
 */
var a = function (p, u) {
        this.p = p || 1;
        this.u = u || "u";
    }
//实例化类a
var test = objectManager.create(a, 13, 99);   
```




