---
title: mixin
date: 2017-08-29 11:23:51
tags:  React javascript
---

# mixin

```$xslt
虽然在ES6的语法中已经对mixin进行了废弃，但是本着追源的实质，
我决定对mixin再进行一下研究，看看当时的语法思想
```
------

## mixin允许我们定义在多个组件共用的方法


### 1.什么是mixin

```javascript
//常规写法
var Timer = React.createClass({

    getInitialState:function(){
      return {secondElapsed: 0}
    },
    
    tick:function(){
      this.setState({secondElapsed:this.state.secondElapsed+1})
    },
    
    componentDidMount:function(){
      this.interval = setInterval(this.tick,1000);
    },
    
    componentWillMount:function(){
      clearInterval(this.interval);
    },
    
    render:function(){
      return(
        <div>
            Seconds Elapsed : {this.state.secondElapsed}
        </div>
      )
    }
})
```
