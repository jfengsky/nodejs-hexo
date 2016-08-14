---
title: Backbone View
date: 2016-08-14 15:23:04
tags: backbone
categories: javascript
---
#### 初始化一个View
这里结合model来初始化一个view

    var student1 = {
        name: 'xiaoming',
        age: 12
    }
    var Model = Backbone.Model.extend()
    var studentModel = new Model(student1)

    // 创建一个View对象
    var View = Backbone.View.extend()

    // 生成一个View实例
    var studentView1 = new View({
        model: student1
    })

#### 把View写入页面中
重新修改View对象

    // 创建一个View对象
    var View = Backbone.View.extend({
        tagName: 'p',
        className: 'item-name',
        initialize: function(){
            this.render().$el.appendTo('body')
        },
        render: function(){
            var json = this.model.toJSON();
            this.$el.html('<h3>' + json.name + '</h3><span>'+ json.age +'</span>')
            return this
        }
    })

    // 生成一个View实例
    new View({
        model: studentModel
    })

#### 绑定事件，当model改变时重新渲染DOM
修改View对象，在initialize里用listenTo绑定事件

    var View = Backbone.View.extend({
        tagName: 'p',
        className: 'item-name',
        initialize: function(){
            this.render().$el.appendTo('body')

            // 监听model
            this.listenTo(this.model, 'change', this.render)
        },
        render: function(){
            var json = this.model.toJSON();
            this.$el.html('<h3>' + json.name + '</h3><span>'+ json.age +'</span>')
            return this
        }
    })

    // 修改model
    studentModel.set('name', 'zhangsan')

#### 给view添加各种事件
修改View对象，在events里添加各种事件

    var View = Backbone.View.extend({
        tagName: 'p',
        className: 'item-name',
        initialize: function(){

            this.render().$el.appendTo('body')
            this.listenTo(this.model, 'change', this.render)
        },

        // 这里给view绑定各种事件
        events: {
            // 事件名 选择器: 事件字符串
            'click span': 'handleSpanClick',
            'mouseenter': 'handleHover'
        },

        // 绑定的事件
        handleSpanClick: function(ev){
            console.log(ev)
        },
        render: function(){
            var json = this.model.toJSON();
            this.$el.html('<h3>' + json.name + '</h3><span>'+ json.age +'</span>')
            return this
        }
    })

#### 修改View先修改Model

    var View = Backbone.View.extend({
        tagName: 'p',
        className: 'item-name',
        initialize: function(){
            this.render().$el.appendTo('body')
            this.listenTo(this.model, 'change', this.render)

            // 用backbone内置方法remove删除view
            this.listenTo(this.model, 'destroy', this.remove)
        },
        events: {
            'click span': 'handleSpanClick'
        },
        handleSpanClick: function(){

            // 用backbone内置方法destroy删除model
            this.model.destroy()
        },
        render: function(){
            var json = this.model.toJSON();
            this.$el.html('<h3>' + json.name + '</h3><span>'+ json.age +'</span>')
            return this
        }
    })