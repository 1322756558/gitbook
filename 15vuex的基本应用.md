### vuex的基本使用

首先从安装开始, 进入文件夹 `npm install vuex`

在main.js中引入vuex

```js
import Vue from 'vue'
import App from './App.vue'
// 引入
import Vuex from 'vuex'

Vue.use(Vuex);
Vue.config.productionTip = false

const store = new Vuex.Store({
  state: {
    count: 0,
  },
  // 操作数据
  mutations: {
    increment(state, n){
      state.count+=n;
    }
  },
  // 异步操作
  actions: {
    increment({commit}) {
      setTimeout(()=>{
        commit('increment', 5)
      }, 3000)
    }
  },
  // 缓存
  getters: {
    doubleCount(state){
      return state.count *2;
    }
  }

});

new Vue({
  // 注册
  store,
  render: h => h(App),
}).$mount('#app')

```

```html
<template>
  <div id="app">
    {{count}}
    {{$store.getters.doubleCount}}
    <button @click="$store.commit('increment', 2)">mutation count++</button>
    <button @click="$store.dispatch('increment')">disable count++</button>
  </div>
</template>

<script>

export default {
  name: 'app',
  computed: {
    count() {
      return this.$store.state.count
    }
  }
}
</script>

<style>
</style>

```

结合之前的结构图, 我们可以知道:

- state中保存数据
- mutations对state中的数据进行操作
- 当有异步操作的时候在actions中进行

这里还有个getters没有见过, 这里的getters是对数据进行缓存类似于computed watch

要注意的是Vuex 的 actions 应该避免直接操作 state，state 的更改应该由 mutations 去修改，不然 vue-devtools 插件记录不到 state 变更，actions 可以根据当前 state 进一步处理数据，计算或请求后端接口，然后通过 commit 的形式提交给 mutations 去处理。