### vue 中.sync写法

首先标明官方文档的位置[https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-修饰符)



.sync修饰符是2.3.0新增的写法, 它的作用其实就是解决父子组件之间数值的双向绑定, 情况下子组件不应该修改父组件的值, 这样做我们很难定位到数值到底在哪里被修改, 不利于代码的后期维护及开发, 所以我们一般使用.sync修饰符将要修改的值回调到父组件中修改



.sync其实是一种简化的写法, 这里我们想把isShow与父组件中的show进行绑定

```vue
<test v-on:update:visible="show"/>
简写为:
<test :visible.sync='show'/>

methods:{
            close(){
                this.isShow = false;
                this.$emit('update:visible', false);
            }
        }
```

