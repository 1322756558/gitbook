### 核心概念

State        ---- this.$store.state.xxx 取值

Getter      ---- this.$store.getters.xxx 取值

Mutation ---- this.$store.commit("xxx") 赋值

Action      ---- this.$store.dispatch("xxx") 赋值

Module

| 方法     | 写法                        | 作用               | 原理                                |
| -------- | --------------------------- | ------------------ | ----------------------------------- |
| State    | this.$store.state.xxx       | 取值               | 提供一个响应式数据                  |
| Getter   | this.$store.getters.xxx     | 取值               | 借助Vue的计算属性computed来实现缓存 |
| Mutation | this.$store.commit("xxx")   | 赋值               | 更改state方法                       |
| Action   | this.$store.dispatch("xxx") | 赋值               | 触发mutation方法                    |
| Module   |                             | 模块化状态组织管理 | Vue.set动态添加state到响应式数据中  |



手动去实现vuex的功能

```js
import Vue from 'vue';
const Store = function Store (options = {}) {
    const {state = {}, mutations={}} = options;
    this._vm = new Vue({
        data: {
            $$state: state
        },
    });
    this._mutations = mutations;
};
Store.prototype.commit = function (type, payload) {
    if (this._mutations[type]){
        this._mutations[type](this.state, payload);
    }
};
Object.defineProperties(Store.prototype, {
    state: {
        get: function () {
            // vue的data对外不暴露, 需要使用_data进行访问
            return this._vm._data.$$state;
        }
    }
});
```


