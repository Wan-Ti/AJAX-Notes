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

## 异步操作和Promise

**以AJAX为例：**

request.send()之后，并不能直接得到response。必须等到readyState变为4后，浏览器回头调用request.onreadystatechange函数，这样才能得到request.response.

回调函数的意思是：自己写的函数却不调用而是给别人调用函数。

举例：
```
function f1(){}
function f2(fn){
  fn()
}
f2(f1) //f1是回调
```

**异步和回调的关系**

* 关联

  异步任务需要在得到结果时通知JS来拿结果；</br>
  可以让JS写留一个函数地址给浏览器，异步任务完成时浏览器调用该函数地址即可，同时把结果作为参数传给该函数。这个函数是程序员写给浏览器调用的，所以称为回调函数。
  
* 区别
  
  异步任务需要用到回调函数来通知结果，但回调函数不一定只用在异步任务里，回调可以用到同步任务里。`array.forEach(n=>console.log(n))`就是同步回调
  
* 总结

异步任务不能拿到结果;

于是我们传一个回调给异步任务;

异步任务完成时调用回调；

调用的时候把结果作为参数；

**如果异步任务有两个结果成功或失败**

两个结果：

* 方法一：回调时接受两个参数
```
fs.readFile('./1.txt',(error,data)=>{
  if(error){ console.log('失败');return }
  console.log(data.toString()) //成功
 })
 ```
 * 方法二：设置两个回调
 ```
 ajax('get','./1.json',data=>{},error=>{})
 //前面函数是成功回调，后面函数是失败回调
 ajax('get','./1.json',{
    success:()=>{},fail:()=>{}
 })
 //接受一个对象，对象有两个key表示成功和失败
 ```
 
 **如何解决回调函数**
 
 以AJAX的封装为例来解释Promise的用法：
 
 ```
 ajax = (method,url,options)=>{
    const  {success,fail} = options //析构赋值
    const requeset = new XMLHttpRequest()
    request.open(method,url)
    request.onreadystatechange = () =>{
      //成功就调用success,失败就调用fail
    if(request.status < 400){
 success.call(null,request.response)
 }else if(request.status >= 400){
   fail.call(null,request,request.status)
  }
 }
}
request.send()
}

ajax('get','/xxx',{
 success(response){},fail:(request,status)=>{}
 })
 //左边是function缩写，右边是箭头函数
      
 ```
 更改为Promise写法
 
 将
 ```
 ajax('get','./xxx',{
   successs(response){},fail:(request,status)=>{}
   })
 ```
 改写为
 ```
 ajax('get','/xxx')
    .then((response)=>{},(request,status)=>{})
 ```
 
 **小结**
 
 * 第一步：</br>
 
 return new Promise((resolve.reject)=>{...});</br>
 任务失败则调用resolve(result);</br>
 任务失败则调用reject(error);</br>
 resolve和reject会再去调用成功和失败函数；</br>
 
 * 第二步：
 使用.then(success,fail)传入成功和失败函数
 
 **我们封装的ajax的缺点**
 
 * post无法上传数据
   
   request.send(这里可以上传数据)；
  
  * 不能设置请求头
   
   request.setRequestHeader(key,value)
   
  **axios高级用法**
  
  * JSON自动处理
    
    axios如何发现响应的Content-Type是json,就会自动调用JSON.parse，所以说正确设置Content-Type是好习惯。
  
  * 请求拦截器
    
    可以在所有请求里加些东西，比如加查询参数；
    
 * 响应拦截器：
 
   可以在所有响应里加些东西，甚至更改内容。

* 可以生成不同实例(对象)
  
  不同实例可以设置不同的配置，用于复杂场景。
