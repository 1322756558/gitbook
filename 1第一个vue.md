### 第一个vue项目



*使用引入的方式引入vue.js*​

```js
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
```



这样的引用方式可以使用的功能:

**数据的双向绑定,及组件的定义**

```js
<div id="app">
    {{msg}}

    <div>
        <!--双向绑定 v-model 同步更新到 info 中-->
        <input type="text" v-model="info">
        <!--v-on: 简写为 @-->
        <button @click="handleClick">添加</button>
    </div>
    <ul>
        <!--<li v-for="item in list">{{item}}</li>-->
        <!--v-bind: 简写为 ":" 组件中的props: ['item']-->
        <todo-item v-for="item in list" :item="item"></todo-item>
    </ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    Vue.component(
        'todo-item',
        {
            // 这里使用差值绑定 使用 props 进行绑定
            props: ['item'],
            template: '<li class="item">{{item}}</li>',
        }
    );
    new Vue({
        el: '#app',
        data(){
            return {
                info: "",
                msg: "hello geettime",
                list: [],
            }
        },
        methods: {
            handleClick(){
                console.log(this.info);
                this.list.push(this.info);
                // 清空输入栏
                this.info = '';
 
            }
        }
    })
</script>
```

**但是这样的写法面临着很多问题**:

1. 组件的作用域为全局
2. 组件无法为组件单独定制样式(class全局)



*解决方式为使用单文件形式的组件开发*

需要安装node.js 以及 npm(随node.js安装)