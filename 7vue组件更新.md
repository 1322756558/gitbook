## vue组件更新

首先vue是一个以数据驱动的端架, **不管发生什么都不要直接修改页面的DOM结点**, 对于vue的组件更新就是数据的改变驱动页面的重新渲染叫做组件更新

对于数据的来源主要有三种

- 来自父元素 - 属性props
- 来自自身状态data
- 来自状态管理器, 如vuex, Vue.observable

然而对于属性和状态来说都未必能触发组件的更新

**只有经过注册并且页面上被调用的数据, 修改的时候才会使得vue组件的更新**

```vue
// 子组件
<template>
  <div>
    <p>props.info: {{ info }}</p>
    <p>props.name: {{ name }}</p>
    <p>props.list: {{ list }}</p>
    <p>data.a: {{ a }}</p>
    <p>
      <button @click="handleBChange">change data.b</button>
    </p>
  </div>
</template>
<script>
export default {
  name: "PropsAndData",
  props: {
    info: Object,
    name: String,
    list: Array
  },
  data() {
    return {
      a: "hello",
      b: "world"
    };
  },
  /**
   * 钩子声明周期函数
   * 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
   */
  updated() {
    console.log("触发 PropsAndData updated");
  },
  methods: {
    handleBChange() {
      this.b = "vue" + Date.now();
      console.log("data.b 发生了变化，但是并没有触发组件更新", this.b);
    }
  }
};
</script>

// 调用页面
<template>
  <div>
    <p>
      <button @click="handleNameChange">change this.name</button>
      <button @click="handleInfoChange">change this.info</button>
      <button @click="handleListChange">change this.list</button>
    </p>
    <PropsAndData :name="name" :info="info" :list="list" />
  </div>
</template>
<script>
import PropsAndData from "./PropsAndData";
let name = "world";
export default {
  components: {
    PropsAndData
  },
  data() {
      this.name = name;
      return {
      // name: name,
      info: {},
      // info: {
      //   number: undefined
      // },
      list: []
    };
  },
  methods: {
    handleNameChange() {
      this.name = "vue" + Date.now();
      console.log("this.name 发生了变化，但是并没有触发子组件更新", this.name);
    },
    handleInfoChange() {
      // this.info.number = 1;
      // this.$set(this.info, 'number', 1);
      console.log("this.info 发生了变化，但是并没有触发子组件更新", this.info);
      console.log(this.info);
    },
    handleListChange() {
      this.list.push(1, 2, 3);
      console.log("this.list 并没有发生变化，但是触发了子组件更新", this.list);
    }
  }
};
</script>

```

可以到上面代码中的info注册了但是info.number以及name都没有注册, 所以handleNameChange()以及handleInfoChange()两个事件在调用的时候都不会触发更新, 而list经过注册所以调用handleListChange()的时候会触发组件更新

还有子组件中的handleBChange()由于b并没有在页面中被显示或调用所以调用handleBChange()的时候也不会触发vue组件更新



### 从vue的原理上去解析这个问题

![1569211257695](https://github.com/1322756558/gitbook_vue_note/blob/master/组件更新.png)

从这张图中可以看到, vue有一个数据中间层去间接对页面进行渲染的管理, 对于上面的情况如果数据没有注册(setter)或者调用(getter)时是不会使得页面重新去渲染的

当然你可以用$forceUpdate() 来强制刷新, 不过绝绝绝大多数情况下你是用不到的