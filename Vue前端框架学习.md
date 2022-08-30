# Vue前端框架学习



直接\<script> 引入使用

```javascript
<script type="text/javascript" src="../js/vue.js"></script>
```



初体验

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初识Vue</title>
    <!-- 全局引入vue.js -->
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    
    <!-- 一个容器 -->
    <div id="root">
        <h1>Hello,{{message}}</h1>
        <h1>我的年龄是：{{age}}</h1>
    </div>

    <script>
        // 创建vue实例
        var a = new Vue({
            el:'#root', // el通常指定当前vue实例为那个容器服务，指定让vue实例与容器建立联系
            data:{ // data中用于存储数据，数据供el属性所指定的容器去使用，值我们暂时先写成一个对象
                message:'RAIN7',
                age:28
            }
        })
    </script>
    
</body>
</html>
```



- 1、想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象{el：data:}
- 2、root容器中的代码依然是html，只不过混入了一些特殊的模板语法
- 3、root容器中的代码称为 Vue 模板（template）



##  Vue 语法指令



- v-text 

  > 相当于 innertext,填充text内容，内容通常放在 Vue实例的data中



- v-html

  > 相当于 innerHTML,填充html语句，内容通常放在 Vue实例的data中



- v-on 

  > 绑定事件，就是绑定一个函数，这个函数写在 Vue实例的 methods 中



事件的简化绑定

> v-on:"click"="a（）",这种形式的代码可以写成 @click
>
> v-on:"mouseover"="b()"，@mouseover



- v-if   v-elif v- else

  > 如果后面的为真，那么才进行执行显示这里的标签



- v-show

  > 绑定 style的display属性，只要为真，那么显示，只要为假，那么隐藏



- v-bind

  > 绑定该标签的除value外的所有属性 v-bind:"font-size"=""



- v-for

  > 执行遍历操作，所属的标签的下级标签可以直接引用对象进行遍历
  >
  > ```
  > value,key,index in user// 遍历对象
  > item in items // 遍历数组
  > ```



- v-model

  > 绑定该标签的value属性，一般用于表格 input



## el

相当于元素选择器，绑定容器



## data

里面放入定义好的数据，data:{name:"",list:[{id:"",name:""}]},全部都写成json格式的



## methods

里面放的都是方法



## computed

computed是实例中的属性，将一些需要多次重复计算的函数放到这里，多次取的时候实际只算一次，其余每次相当于都缓存



## 事件修饰符



- v-on:"stop" 

> 阻止绑定的标签发生事件冒泡



- v-on:"prevent"

> 用来阻止标签的默认行为，就是写了个href，不跳转，只执行事件



- v-on:"self"

> 只监听自身标签触发的事件



- v-on:"once"

> 自身标签触发的事件只发生一次



## 按键修饰符



用来对键盘按键进行修饰，修饰符

@keyUp: 键盘任意键抬起发生事件

@keyDown： 键盘任意键按下发生事件



- @keyUp:"enter"

> 对键盘回车键进行修饰



- @KeyUp:"tab"

> 对键盘切换tab键修饰



## Vue实例的生命周期



**什么是Vue的生命周期？**



> Vue实例对象从创建到运行，再到销毁的过程 



**什么是生命周期钩子？**



> 就是生命周期函数。特点：伴随着Vue实例的生命周期过程自动触发的，不需要认为手动触发的函数



![在这里插入图片描述](https://img-blog.csdnimg.cn/53e263f308694c16b5b5abddd5353ce1.png)



&emsp;&emsp;

生命周期方法在vue实例中，created()在渲染之前，一般方法内部发送axios请求返回后端数据给Vue中的data数据进行赋值，以实现前后端数据的交互。



```javascript
<script>
    // 创建vue实例

    var a=new Vue({
        el:"",
        data:{},
        methods:{},
        computed:{},

        // 初始化阶段
        beforeCreate(){// 第一个生命周期函数，这个函数执行时，仅仅完成自身内部事件以及生命周期函数的注入工作
           console.log("beforeCreate:==========")
        },
        created(){// 第二个生命周期函数，这个函数执行时，完成自身内部事件、生命周期函数、自定义数据data\methods\computed注入以及语法校验工作
            console.log("created:==========")
        },
        beforeMount(){// 第三个生命周期函数，这个函数执行的时候，el执行HTML还是一个原始模板，并没有完成数据渲染工作（模板指定代码与 vue数据不绑定）
            console.log("beforeMount:==========")
        },
        mounted(){// 第四个生命周期函数，vue已经完成了 对 template 和 el的渲染工作
            console.log("mount:==========")
        },

        // 运行阶段
        beforeupdate(){// 第五个生命周期函数，这个函数执行时，data数据发生改变，此时页面数据还是原始数据
            console.log("beforeUpdate:==========")
        },
        updated(){//第六个生命周期函数，这个函数执行时，data数据 已经和 页面数据 保持一致了
            console.log("updated:==========")
        },

        // 销毁阶段 ， vue并不会自动销毁，只有手动调用 a.$destory()才会销毁
        beforeDestroy(){// 销毁的第一个生命周期函数。这个函数指定的时候，vue实例才刚刚开始销毁
            console.log("beforeDestroy:==========")
        },
        destroyed(){// 销毁阶段的第二个生命周期函数，这个函数执行，vue实例已经销毁全部
            console.log("destroyed:==========")
        }

    })
</script>
```

