# AJAX-Notes

AJAX指的是：使用JS发送和接受请求

* 浏览器可以发送请求以及接受响应；
* 浏览器在window上添加一个XMLHttpRequest函数；
* 用该构造函数可以构造一个对象；
* JS通过该对象实现发送请求，接收响应

## 使用AJAX加载CSS

* 创建HttpRequest对象(全称是XMLHttpRequest)
* 调用对象的open方法
* 监听对象的onload&onerror事件
* 调用对象的send方法(发送请求)

## 使用AJAX加载JS

* 创建HttpRquest对象
* 调用对象的open方法
* 监听对象的onreadystatechange事件
* 调用对象的send方法(发送请求)

## 使用AJAX加载HTML
