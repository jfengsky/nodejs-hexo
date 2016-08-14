layout: backbone
title: Backbone Model
date: 2016-08-14 12:04:02
tags: backbone
categories: javascript
---
### 定义一个数据模型
    var Student = Backbone.Model.extend({
        // 默认数据
        defaults: {
            name: '',
            sex: '',
            age: 0
        }
    })

### 实例化一个数据
*   通过<code>new</code>关键字，传入数据并替换默认数据


    var xiaoming = new Student({
        name: '小明',
        sex: 'male',
        age: 12
    })

### 读写数据
*   get()读数据
*   set()方法修改数据
*   unset()方法删除对象中制定的属性和数据
*   clear()方法删除模型中所有的属性和数据
*   toJSON()方法获取原始数据
*   set unset clear都会触发事件


    // 读 编码防止乱码和XSS问题
    var name = xiaoming.escape('name');
    var sex = xiaoming.get('sex')
    var age = xiaoming.get('age')

    // 修改
    xiaoming.set('name', 'zhangxiaoming')
    xiaoming.set('age', 14)
    xiaoming.unset('name')
    xiaoming.clear()

### 给读写数据绑定事件
*   只有变化的数据才会被修改和触发事件
*   先触发属性事件，再触发所有属性事件


    // 监听所有属性变化绑定事件
    xiaoming.on('change', function(model){
        console.log('model changed')
    })

    // 监听name变化事件
    xiaoming.on('change:name', function(model){
        console.log('name change')
    })

    // 更新数据
    xiaoming.set({
        name: 'xiaoming',
        age: 18
    })

### 数据验证
*   绑定<code>invalid</code>事件
*   通过save()方法触发


    // 首先给模型添加验证方法
    var Student = Backbone.Model.extend({
        ...
        validate: function(data, options){
            if(data.age < 1){
                return '年龄不能为0'
            }
        }
    })

    xiaoming.on('invalid', function(model, invalid){
        console.log(invalid)
    })

    xiaoming.save({
        age: 0
    })

### 与服务器保持数据同步
*   save()方法向服务器提交数据
*   fetch()从服务器获取数据
*   destroy()方法从服务器移除数据