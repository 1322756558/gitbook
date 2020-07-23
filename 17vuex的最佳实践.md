### 使用常量替代Mutation事件类型

```js
// mutation-types.js
export const SOME_MUTATION = "SOME_MUTATION"

// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
    state: {...},
    mutations: {
        // 我们可以使用ES2015 风格的计算属性命名功能来使用一个常量来作为函数名
        [SOME_MUTATION](state){
            // mutate state
        }
    }
})
```



### 命名空间

Module

- 开启命名空间 namespace: true
- 嵌套模块不要过深, 尽量扁平化(可以选择只有一级, 配合路由)
- 灵活应用 createNamespaceHelpers, 辅助函数