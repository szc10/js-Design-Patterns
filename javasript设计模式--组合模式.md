##javascript设计模式 组合模式

####什么是组合模式

```
组合模式（Composite）将对象组合成树形结构以表示“部分-整体”的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。
```

####组合模式的作用

```
1.可以使用同样的方法处理对象的集合与其中的特定子对象,组合对象与组成它的对象实现了同一批操作,对组合对象执行的这些操作将向下传递到所有的组合对象.
2.可以用来把一批子对象组织成树形结构,并且使整个树都可以被遍历,所有的组合对象都实现了一个用来获取其子对象的方法
```

####使用组合模式的两个条件
ps: 只有同时符合这两个条件才可以使用组合模式

```
1.存在一批组织成某种层次体系的对象
2.希望对这批对象或其中的一部分对象实施一个操作
```


####一个简单的例子
```javascript
/**
     * 构造组合对象的一个管理类,并且初始化
     * @param formId
     * @constructor
     */
    var CompositeForm = function(formId){
        this.formComponents=[];

        this.formElement=document.createElement('form');
        this.formElement.id=formId;
        document.getElementsByTagName('body')[0].appendChild(this.formElement);
    };
    /**
     * 添加组合对象的叶对象
     * @param child
     */
    CompositeForm.prototype.add=function(child){
        this.formComponents.push(child);
        this.formElement.appendChild(child.getElement());
    };
    /**
     * 移除组合对象的叶对象
     * @param childId
     */
    CompositeForm.prototype.remove=function(childId){
        for(var i=0;i<this.formComponents.length;i++){
            if(this.formComponents[i].id==childId){
                this.formComponents.splice(i,1);
                this.formElement.removeChild(document.getElementById(childId));
                break;
            }
        }
    };
    /**
     * 判断组合对象中的各个叶对象的值是否为空
     * 通过建立统一的方法遍历叶对象的方法来判断
     */
    CompositeForm.prototype.validate=function(){
        for(var i=0; i<this.formComponents.length;i++){
            if(this.formComponents[i].validate()){
                console.log(this.formComponents[i].id+"="+this.formComponents[i].getValue());
            } else{
                console.log(this.formComponents[i].id+" is no value");
            }
        }
    }

    /**
     * 建立文本框的叶对象
     * @param inputId
     * @param placeholder
     * @constructor
     */
    var InputField = function(inputId,placeholder){
        this.input=document.createElement('input');
        this.id=inputId;
        this.input.placeholder="test" || placeholder;
    };

    InputField.prototype.getElement = function(){
        return this.input;
    };
    InputField.prototype.getValue = function(){
        return this.input.value;
    };
    InputField.prototype.validate=function(){
        if(this.getValue()){
            return true;
        } else{
            return false;
        }
    }
```


测试代码



```javascript
	/**
     * 建立测试数据
     * @type {CompositeForm}
     */
    var compositeForm = new CompositeForm('testForm');
    compositeForm.add(new InputField("name"));
    compositeForm.add(new InputField("passwd"));
    compositeForm.validate();
```



#### 组合模式的好处
1.易于代码的编写,程序员编写一个组合代码,就可以反复的使用它
2.提供常见,简化的对模块的处理接口


