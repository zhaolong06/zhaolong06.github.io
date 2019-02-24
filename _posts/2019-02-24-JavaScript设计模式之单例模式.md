---
layout: post
title: JavaScript设计模式之单例模式
date: 2019-02-24
categories: 设计模式, 前端, JavaScript
tags: 设计模式
---

* content
{:toc}

### 单例模式的定义

单例模式最初的定义出现于《设计模式》（艾迪生维斯理, 1994）：“保证一个类仅有一个实例，并提供一个访问它的全局访问点。”

### 实现单例模式的几种方式

#### 类方式

```javascript

    var Singleton = function(name) {
        this.name = name;
        this.instance = instance;
    }

    Singleton.getInstance = function(name) {
        if(!this.instance) {
            this.instance = new Singleton(name);
        }
        return this.instance;
    }

```
这种方式跟Java里面的静态类方法有点类似，通过类提供的一个获取实例的方法来返回单例类的实例。

#### 闭包方式

```javascript

    var Singleton = function(name) {
        this.name = name;
    }

    Singleton.getInstance = (function() {
        var instance = null;
        return function(name) {
            if(!instance) {
                instance = new Singleton(name);
            }
            return instance;
        }
    })();

```
闭包的方式是通过一个闭包来维护单例对象，在需要的时候返回该对象。

#### 代理方式

```javascript

    var Singleton = function(name) {
        this.name = name;
    }

    var ProxySingleton = (function() {
        var instance = null;
        return function(name) {
            if(!instance) {
                instance = new Singleton(name);
            }
            return instance;
        }
    })();

```
代理模式跟闭包的方式有些类似，只不过获取该单例类的实例不再是放在类的方法上，而是单独提供另一个类来维护单例。

#### 对象字面量方式

```javascript

    var singleton = {};

```
在JavaScript中，最简单的单例便是一个对象字面量方式，这样每一个对象都是唯一的。但这样做也有弊端，你无法保证别人不会修改该对象，因此，可以适当的使用命名空间的方式来避免全局变量的污染。



### 小结

在JavaScript中，由于语言特性的原因，在实现单例的时候会有很多灵活性，但正是因为这些灵活性，也使得代码很容易就被绕过去，而这些只能靠团队的规范来避免。
