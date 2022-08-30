## Axios 前后端交互工具学习



##  引言

&emsp;&emsp;Axios是一个异步请求技术，核心作用就是用来在页面中发送异步请求，并获取对应数据在页面中进行渲染，页面局部更新技术Ajax.



## 引入Axios



cdn的方式引入

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



## 介绍



axios有get（）、delete（）、put（）、post（）,这个几个方法



## GET请求的方式

```javascript
<script>
 // axios发送一个异步请求之 GET请求
         axios.get("http://localhost:9090/get?id=1")
             .then(// 代表请求成功之后的一个正确处理
                 function (response) {// 返回的response是一个对象
                     console.log(response.data);})
             .catch(// 请求失败的一个处理
                 function (err) {
                     console.log(err);
                 }
         )
</script>
```

get里面的参数就是一个url，传递参数的时候直接拼接到url中



### then方法

这个就相当于回调函数，在ajax中 有一个success:function(data){},可以进行回调，而这里通过 then进行对请求返回的响应数据进行一个处理，内部是一个函数，函数中的参数是返回的响应（包含响应头、响应数据、相应格式等，通过 response.data 能拿到返回数据）



### catch方法

这个就像与异常返回的函数，在ajax中有一个  error:function(){},，返回的服务器异常错误的响应数据



## POST请求的方式

```javascript
        // axios发送各种方式的异步请求,post方法的第一个参数是 url，第二个参数是 在body中的 json对象
       // 第二个参数自动转化成json数据，后端可以直接接收
        axios.post("http://localhost:9090/post",{id:"1"}).then(function (response) {
             console.log(response.data);
        }).catch(function (error) {
             console.log("服务器异常!")
        })

```



怎么在body中传数据？

&emsp;&emsp;在post方法中第一个参数是请求的url，第二个请求的参数写成JSON格式的数据，后端可以直接通过 @RequestBody进行接收，后面的then、catch就照旧了。



## PUT 请求的方式

```javascript
    axios.put("http://localhost:9090/put",{id:"1",name:"rain7"}).then(function (response) {
            console.log(response.data);
        }).catch(function (error) {
            console.log("接口访问失败!");
        })
```



## Axios的基本配置



说一个问题，axios可以对一些事情进行基本的设置，在前后端分离的时候

比如说我们在进行异步请求的时候，要设置全局的一个baseUrl，以后每次写的时候url就不要写那么长了

还有请求发送超过5s，认为发送请求失败



这些配置在axios创建实例的时候就通过create进行配置，我们之后用instance进行发送axios请求

```javascript
var instance = axios.create({
    baseURL:"http://localhost:8080/",
    timeout:"1000",//以毫秒为单位
});
```



配置还有很多，可以进axios的官网进行查看，每个配置都有注释





## Axios的拦截器



&emsp;&emsp;可以在发送请求之前进行拦截（token身份验证）、也可以在返回响应之后进行拦截（服务器异常统一处理）,官网都有处理的代码以及各种拦截的方式!



```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {// config就是axios的配置信息，可以去官网看看
    if(config.url.indexOf("?")===-1){// 如果没有？，那么说明之前没参数
        url+="?token=1234";
    }else{
        url+="&token=1234";
    }
    return config;
  }, function (error) {// 可以不写这个处理函数
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    if(response.status!==200){
        alert("服务器出现错误!");
    }
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```





## Vue 怎么和 Axios配合呢？



&emsp;&emsp;要想和vue进行配合，请先理解Vue的生命周期的这个知识点，在vue的所有data数据被加载后，在created() 阶段就可以之间请求数据进行加载了，如果在 beforCounted、count阶段进行请求的话，那么就相当于先渲染为空，然后再请求后端再次渲染，二次渲染不太好



&emsp;&emsp;总之 Axios的请求 要写在 Vue的生命周期函数 create() 函数中，如果axios内部要拿到data中的数据，不能使用this,因为axios内部的this指的是axios这个对象，不是vue实例，所以在axios外面，create()内部定义 _this = this，在axios内部使用 _this 代替 this 即可



```javascript
   var a = new Vue({
        el:'', // el通常指定当前vue实例为那个容器服务，指定让vue实例与容器建立联系
        data:{ // data中用于存储数据，数据供el属性所指定的容器去使用，值我们暂时先写成一个对象
        },methods:{},
       create(){// 生命周期的第二个函数，此时上面的data、methods数据已经加载过且校验了
           var _this = this;
           var instance = axios.create({
                baseURL:"http://localhost:8080/",
    			timeout:"1000",//以毫秒为单位
           })
           instance.get("/url").then(function(){}).catch(function(){});
       }
    })
```

