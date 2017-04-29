##javascript设计模式 适配器模式

####什么是适配器模式

```
适配器模式（Adapter）是将一个类（对象）的接口（方法或属性）转化成客户希望的另外一个接口（方法或属性）,适配器模式使得原本由于接口不兼容而不能一起工作的那些类（对象）可以一些工作,速成包装器（wrapper）。
```

####适配器模式的作用

适配器的作用很像是电源适配器,香港的充电器是不能直接插在内地的,需要一个电源转换器

```
1.适配器可以协调两个不同的接口
2.适配器可以使原有不直观的接口,改造成更加简洁或丰富的接口
```

####一个简单的适配器的例子
这里有一个例子,一个展示学生信息的API
```javascript
    /**
     * 一个展示学生信息的接口
     * @param id
     * @param name
     * @param sex
     * @param age
     */
    function displayStudentInf(id,name,sex,age){
        document.write("id:"+id+" name:"+name+" +sex"+sex+" age"+age);
    }
```

我们在使用这个接口,总是要参数按照接口参数表,很容易出错.因此,我们可以进行API包装一下,我们在使用代码和应用接口之间设置一个代码薄层



```javascript
	/**
     * 将之前的学生信息接口通过一个适配器包装起来,简化接口参数
     * @param student 传入一个学生的对象数组
     */
    function displayStudentInfAdapter(student){
        displayStudentInf(student.id,student.name,student.sex,student.age);
    }
```



适配器的调用



```javascript
	/**
     * 如何使用适配器接口
     * @type {{id: number, name: string, sex: string, age: number}}
     */
    var stu={
        id:51389222,
        name:'jhon',
        sex:'boy',
        age:12
    };
    displayStudentInfAdapter(stu);
```

####适配器的使用场合
适用于客户系统期待的接口与现有的API提供的接口不兼容,它只能协调语法上的差异