# redis3x-flash
基于redis3x-session的Express服务端session中间件, 使用它可以在单机或多集群部署服务的条件下，使用flash保存临时消息，在发生重定向或者再次请求服务端时，能够一次性获取，并在获取值的同时清空该消息。适用于用户登录过程中的消息提示以及客户一次性数据存储。

# 依赖
该中间件依赖redis3x-session, 使用该中间件时，需先使用redis3x-session中间件

# 使用

## 引用中间件
```javascript
var express = require('express'),
    app = express(),
    redis3xFlash = require('redis3x-flash');

// 服务开启flash中间件
app.use(redis3xFlash());
```

## 示例

引用该中间件后，所有请求对象req，均包含flash方法，可用于临时消息的存取：
```javascript
app.get('/flash', function(req, res){
  // 通过key设置消息
  req.flash('info', '我是flash消息内容!')
  res.redirect('/');
});

app.get('/', function(req, res){
  // 通过key获取消息（同时flash中间件负责清空该消息）
  res.render('index', { messages: req.flash('info') });
});
```



